# Samsung Galaxy Note 3 — Home Assistant WallPanel Kiosk

This project documents the use of a Samsung Galaxy Note 3 as a Home Assistant WallPanel kiosk, including ROM details, telephony issues, WebView upgrade, and dashboard performance findings.

## Device

- Model: Samsung Galaxy Note 3 International LTE
- Marketing model: SM-N9005
- Android device name: `hlte`
- CPU: Qualcomm Snapdragon 800, 32-bit `armeabi-v7a`

## ROM

- ROM: Resurrection Remix OS 6.0.0 (Official)
- Android version: 8.1.0
- Build: `RR-O-v6.0.0-20180211-hlte-Official`
- Build fingerprint: `rr_hlte-userdebug 8.1.0 OPM1.171019.018 c8772d9299 test-keys`

See [rom.md](rom.md).

## Major outcomes

- The original system WebView, `com.android.webview` 61, caused Home Assistant to serve a legacy, incomplete frontend in WallPanel.
- Installing Google Android System WebView 138.0.7204.181 (`com.google.android.webview`) enabled the modern Home Assistant UI in WallPanel.
- WebView 138 is the final Google WebView release supporting Android 8/9; WebView 139+ requires Android 10+.
- The modern dashboard is dramatically better but more demanding on the Note 3 GPU/CPU.
- A separate boot-time ANR affects `com.android.phone` through the Visual Voicemail SIM-state receiver; it can be dismissed and does not prevent WallPanel use.

## Documents

- [ROM notes](rom.md)
- [Telephony and com.android.phone notes](telephony.md)
- [WebView upgrade](webview.md)
- [WallPanel dashboard findings](wallpanel.md)
