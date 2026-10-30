# Telephony Notes — com.android.phone

## Background

The goal was to determine whether `com.android.phone` was causing instability, reboots, or interference with WallPanel.

## Findings

### Disabling the whole package causes a crash loop

Disabling `com.android.phone` removed its telecom permissions and caused a crash loop:

```text
SecurityException ... PhoneAccount
```

The package must remain enabled on this ROM.

### TelephonyDebugService was investigated

An earlier log suggested `TelephonyDebugService` might be involved. However, it could not be disabled:

```text
Error: java.lang.SecurityException: Shell cannot change component state for
com.android.phone/com.android.phone.TelephonyDebugService to 2
```

### Actual boot ANR cause

The reproduced ANR was caused by the boot broadcast to the Visual Voicemail SIM-state receiver:

```text
ANR in com.android.phone
PID: 1028
Reason: Broadcast of Intent {
  act=android.intent.action.BOOT_COMPLETED
  cmp=com.android.phone/.vvm.VvmSimStateTracker
}
```

The process itself was nearly idle during the ANR:

```text
0.1% 1028/com.android.phone
```

### Current status

- `com.android.phone` is enabled and running.
- The ANR is a one-time boot-time timeout, not a continuous crash loop.
- The dialog can be dismissed after boot.
- The receiver cannot be disabled without root or ROM modification.

## Commands used

```sh
adb shell 'ps -A | grep com.android.phone'
adb shell 'pm path com.android.phone'
adb logcat -d -v time | grep -n -B 30 -A 40 "ANR in com.android.phone"
```
