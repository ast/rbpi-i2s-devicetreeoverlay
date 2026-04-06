# rbpi-i2s-devicetreeoverlay

Device tree overlay that exposes the Raspberry Pi I2S peripheral as a full-duplex ALSA sound card (playback + capture). Uses `linux,spdif-dit` and `linux,spdif-dir` as dummy codec drivers so no external codec kernel module is needed — the I2S bus is driven directly by userspace via ALSA.

Typical use case is streaming IQ samples to/from an external DAC/ADC connected to a ham radio transceiver, but it works for any I2S device (microphone ADCs, audio DACs, etc.).

## Build

```
dtc -@ -W no-unit_address_vs_reg -I dts -O dtb -o rbpi-i2s.dtbo rbpi-i2s.dts
```

## Install

```
sudo cp rbpi-i2s.dtbo /boot/firmware/overlays/
```

Add to `/boot/firmware/config.txt`:

```
dtparam=i2s=on
dtoverlay=rbpi-i2s
```

After reboot, verify with `aplay -l` and `arecord -l` — the device should appear as `rbpi-i2s`.

## Testing without reboot

```
sudo dtoverlay rbpi-i2s
```

## Links

- https://www.raspberrypi.com/documentation/computers/configuration.html#device-trees-overlays-and-parameters
