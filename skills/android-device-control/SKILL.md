---
name: android-device-control
description: Use when installing, launching, testing, or collecting logs from a physical Android device over ADB. For unattended Quest runs, follow the headset preparation procedure before the first launch. Use it when Guardian holds focus or an immersive launch stalls during window placement.
---

# Android Device Control

Identify the target with `adb devices -l` or, for a Meta Quest headset, `metavr device list`. Use its serial or device ID for device-specific commands.

## Recover a lost ADB connection

1. On the host, run `adb kill-server`, then `adb start-server`. For an explicitly connected Wi-Fi device, reconnect to its known ADB endpoint with `adb connect <host:port>` if needed.
2. Run `adb devices -l`. Resume when the intended device is listed in state `device`.
3. If the device is still unavailable, wait 30 seconds, then repeat the restart and connection check once.
4. If the second attempt fails, ask the user for help. Report the target's last observed state and any pending device cleanup.

This recovery sequence applies to every physical Android device using ADB, including Quest headsets.

## Prepare a Quest headset for unattended work

For unattended Quest runs, follow this procedure before the first launch. Use it when Guardian holds focus or an immersive launch stalls during window placement.

On completion, failure, or cancellation, restore each setting to its recorded state on the same headset, including after a partial setup. Use `--enable` for settings that were enabled before the run and check both settings again with `--status`. If the ADB connection drops, run the recovery sequence and finish restoration as soon as the headset reconnects.

Before starting, record both settings on the target headset. The proximity sensor controls User Presence Detection.

```sh
metavr device proximity -d <device-id> --status
metavr device boundary -d <device-id> --status
```

Keep the headset unworn during unattended work. Disable both settings through `metavr` and verify their status before proceeding.

```sh
metavr device proximity -d <device-id> --disable
metavr device boundary -d <device-id> --disable
```

[Meta VR CLI device commands](https://developers.meta.com/vr/essentials/metavr-devices-and-apps/)
