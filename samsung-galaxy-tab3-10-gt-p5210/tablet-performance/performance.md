# Performance Findings and Mitigations

## Symptom

Some applications perform well while others are slow, especially after the tablet has been idle.

Even the Trebuchet home launcher can be slow after the tablet has sat for a while.

## Launcher cold-start measurement

First launch after idle:

```text
ThisTime: 894 ms
TotalTime: 894 ms
WaitTime: 958 ms
```

Immediate subsequent launches:

```text
ThisTime: 0 ms
TotalTime: 0 ms
WaitTime: 6-14 ms
```

This confirms that the launcher was killed while idle and had to be cold-started.

## Memory-pressure evidence

The log contains repeated process kills:

```text
Killing ... empty #17
Killing ... from cached
lowmemorykiller errors
```

Examples:

```text
Killing 3664:xyz.wallpanel.app/u0a72 (adj 906): empty #17
Killing 5297:com.android.dreams.phototable/u0a55 (adj 900): 70923k from cached
Killing 6926:com.google.android.googlequicksearchbox:search/u0a37 (adj 902): 81341k from cached
Killing 9169:com.android.vending/u0a34 (adj 902): 66204k from cached
```

WebView-dependent apps were also killed when WebView was trimmed:

```text
Killing 4889:io.homeassistant.companion.android/u0a69: dep: com.android.webview
Killing 6598:org.lineageos.jelly/u0a51: dep: com.android.webview
```

## Conclusions

1. ARM translation is ruled out.
2. The primary issue is aggressive process eviction on a 1 GB RAM device.
3. Cold starts from eMMC make killed apps feel slow.
4. WebView-based apps may suffer twice: their own process and the WebView service can both be killed.
5. WebView 100 is relatively old and may contribute to poor web-app performance.

## Mitigations

### 1. Reduce idle memory consumers

Disable the Photo Table daydream, which was repeatedly consuming about 70 MB:

```sh
adb shell 'pm disable-user --user 0 com.android.dreams.phototable'
```

To restore it:

```sh
adb shell 'pm enable com.android.dreams.phototable'
```

### 2. Avoid keeping multiple heavy apps resident

Do not keep Chrome, Jelly, WallPanel, and Play Store open at the same time. Close or swipe away unused recent apps.

### 3. Disable animations

This reduces UI overhead and makes the old GPU/CPU feel more responsive:

```sh
adb shell 'settings put global window_animation_scale 0'
adb shell 'settings put global transition_animation_scale 0'
adb shell 'settings put global animator_duration_scale 0'
```

### 4. Test a newer WebView

The installed WebView is 100.0.4896.127. Android 7.1.2 may support Google WebView 119.x, the final major version supporting Android 7/8/9.

Any replacement must be the `x86` variant, not ARM.

Check whether Google WebView can be selected:

```sh
adb shell 'cmd webviewupdate set-webview-implementation com.google.android.webview'
```

### 5. Re-measure after changes

Let the tablet idle for at least 10 minutes, then measure launcher launch time:

```sh
adb shell 'am start -W -n com.cyanogenmod.trebuchet/com.android.launcher3.Launcher'
```

Also check memory:

```sh
adb shell 'cat /proc/meminfo | head -n 12'
adb shell 'dumpsys meminfo | head -n 25'
```
