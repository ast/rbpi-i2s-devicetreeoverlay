# rbpi-i2s-devicetreeoverlay

Most (all?) versions of the Raspberry Pi have an I2S digital audio interface. This is however not possible to enable easily by raspi-config or similar tools. Instead one has to make a "Device Tree Overlay" file.

The overlay uses the `linux,spdif-dit` dummy codec driver so that the I2S interface appears as a regular ALSA playback device — no external codec driver is required.

## Build

Compile the dts file with:

```
dtc -@ -I dts -O dtb -o rbpi-i2s.dtbo rbpi-i2s.dts
```

## Install

Copy the compiled overlay into the overlays directory:

```
sudo cp rbpi-i2s.dtbo /boot/firmware/overlays/
```

Then edit `/boot/firmware/config.txt` and add:

```
dtoverlay=rbpi-i2s
```

Also make sure the following line is present and not commented out:

```
dtparam=i2s=on
```

If desirable, the built-in PWM audio interface can be disabled by commenting out:

```
#dtparam=audio=on
```

After reboot, the I2S interface should be visible when typing `aplay -l` or `aplay -L`.

## Testing without reboot

The overlay can be loaded at runtime without rebooting:

```
sudo dtoverlay rbpi-i2s
```

## Links

- https://www.raspberrypi.com/documentation/computers/configuration.html#device-trees-overlays-and-parameters
