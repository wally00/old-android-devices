# WebView Upgrade — 61 to 138

## Original WebView

The ROM originally used:

```text
com.android.webview 61.0.3163.98
```

This caused Home Assistant to serve a legacy frontend inside WallPanel:

- No normal sidebar
- Missing or inconsistent theming
- Very slow dashboard loading
- UI different from Chrome and modern devices

## Chrome is not a WebView provider

Chrome 89 was installed, but Android rejected it as a WebView provider:

```text
Invalid package com.android.chrome
Reason: No WebView-library manifest flag
```

WallPanel cannot use Chrome directly; it uses the selected Android WebView provider.

## Installed WebView

Installed Google Android System WebView:

- Package: `com.google.android.webview`
- Version: 138.0.7204.181
- Architecture: `arm-v7a`
- Minimum Android: 8.0+

It was accepted and automatically selected as the active WebView provider.

## Result

WallPanel now renders the modern Home Assistant frontend, including proper navigation and theming.

## Version ceiling

WebView 138 is the last Google WebView release supporting Android 8 and 9.

WebView 139 and newer require Android 10+.

## Useful commands

```sh
adb shell 'dumpsys webviewupdate'
adb shell 'cmd webviewupdate set-webview-implementation com.google.android.webview'
adb shell 'am force-stop xyz.wallpanel.app'
```
