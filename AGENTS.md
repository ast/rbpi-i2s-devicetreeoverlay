## Project Overview

Device tree overlay for enabling the I2S audio interface on Raspberry Pi. Uses the `linux,spdif-dit` dummy codec driver to expose I2S as a standard ALSA playback device without requiring an external codec driver.

## Build

```
dtc -@ -I dts -O dtb -o rbpi-i2s.dtbo rbpi-i2s.dts
```

`dtc` (device tree compiler) is typically available on the Raspberry Pi itself (`/usr/bin/dtc`), not on the development host.

## Deploy

```
sudo cp rbpi-i2s.dtbo /boot/firmware/overlays/
```

Runtime test without reboot:
```
sudo dtoverlay rbpi-i2s
```

Verify with `aplay -l` — the device should appear as `rbpi-i2s`.

## Test Target

A Raspberry Pi can be used for testing over SSH. Ask the user for the hostname before connecting.
