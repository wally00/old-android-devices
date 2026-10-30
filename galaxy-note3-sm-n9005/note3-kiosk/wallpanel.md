# WallPanel Dashboard Findings

## Package

```text
xyz.wallpanel.app
```

## Original problem

With WebView 61, WallPanel loaded a legacy Home Assistant frontend:

- Slow dashboard loading
- No normal sidebar/navigation
- Inconsistent theming
- UI very different from Chrome and modern devices

## After WebView 138

WallPanel now loads the modern Home Assistant UI successfully.

### Rendering sample

```text
Total frames rendered: 550
Janky frames: 207 (37.64%)
50th percentile: 14 ms
90th percentile: 36 ms
95th percentile: 46 ms
99th percentile: 129 ms
Slow UI thread: 53
Slow issue draw commands: 193
```

## Interpretation

- Loading is dramatically improved.
- The modern Home Assistant UI is now usable.
- The Note 3 hardware is the limiting factor for smooth animation and complex dashboards.
- Most jank comes from draw/GPU work in the modern frontend.

## Optimization recommendations

- Use a single-panel dashboard where possible.
- Reduce the number of visible cards.
- Avoid graphs, maps, camera cards, animations, shadows, and blur effects on the default view.
- Increase update intervals for cards that support it.
- Disable Android animations:

```sh
adb shell 'settings put global window_animation_scale 0'
adb shell 'settings put global transition_animation_scale 0'
adb shell 'settings put global animator_duration_scale 0'
```

## Measurement commands

```sh
adb shell 'dumpsys gfxinfo xyz.wallpanel.app reset'
# use the dashboard for 20-30 seconds
adb shell 'dumpsys gfxinfo xyz.wallpanel.app'
adb shell 'top -n 1 | head -n 30'
```
