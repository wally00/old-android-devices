# ROM Notes — Resurrection Remix 6.0.0

## Installed ROM

- Device: Samsung Galaxy Note 3 International LTE (`SM-N9005`, `hlte`)
- ROM: Resurrection Remix OS 6.0.0 (Official)
- Android version: 8.1.0
- Build: `RR-O-v6.0.0-20180211-hlte-Official`
- Build fingerprint: `rr_hlte-userdebug 8.1.0 OPM1.171019.018 c8772d9299 test-keys`

## ADB and root

- ADB works normally.
- The ROM does not provide a `su` binary in the ADB environment:

```sh
adb shell 'su -c "id"'
# /system/bin/sh: su: not found
```

- Therefore, protected system components cannot be disabled through `su`.

## Package-manager limitations

- The ROM is based on an older Android package-manager version.
- `pm install-existing` is unavailable.
- Some system-component state changes are blocked even with root ADB:

```text
Error: java.lang.SecurityException: Shell cannot change component state for
com.android.phone/com.android.phone.TelephonyDebugService to 2
```

## WebView provider configuration

The ROM initially used:

```text
com.android.webview 61.0.3163.98
```

Chrome 89 was installed but was rejected as a WebView provider:

```text
Invalid package com.android.chrome ... reason: No WebView-library manifest flag
```

Google WebView (`com.google.android.webview`) could be installed and selected.

## Relevant properties

```text
ro.product.model=SM-N9005
ro.product.device=hlte
ro.build.version.release=8.1.0
ro.build.display.id=rr_hlte-userdebug 8.1.0 OPM1.171019.018 c8772d9299 test-keys
ro.modversion=RR-O-v6.0.0-20180211-hlte-Official
```
