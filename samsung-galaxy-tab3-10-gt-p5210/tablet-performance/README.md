# Samsung Galaxy Tab 3 10.1 — Performance Investigation

Documentation for diagnosing uneven app performance on a Samsung Galaxy Tab 3 10.1 Wi-Fi running LineageOS 7.1.2.

## Device

- Model: Samsung Galaxy Tab 3 10.1 Wi-Fi
- Marketing model: GT-P5210
- Android device name: `santos10wifi`
- CPU: Intel Clover Trail, x86
- RAM: approximately 946 MB
- Display: 1280 x 800, 60 Hz

## ROM

- LineageOS 7.1.2
- Build: `lineage_santos10wifi-userdebug 7.1.2 NJH47F dcbe376eb8 test-keys`
- Android API level: 25

## Current findings

- ARM binary translation is not the cause of slow apps: no installed app has an ARM primary ABI.
- The system WebView is `com.android.webview` 100.0.4896.127.
- The Trebuchet launcher takes about 894 ms to cold-start after sitting idle, but 0 ms when already resident.
- ActivityManager frequently kills cached processes, and lowmemorykiller errors are present.
- The tablet has only about 1 GB RAM, so cached apps are evicted aggressively.

## Documents

- [Device profile](device.md)
- [Performance findings and mitigations](performance.md)
