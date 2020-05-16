# USBaspLoader

Modded USBaspLoader to support with out changes in the code different
programmers and devices.

#### Note than the bootloaderconfig.h is currently configured for Punk75 keyboard
D- being port D3 and BOOT(jumper) being port D4. You may need to change those to
match your wiring.

## Wiring

## Flashing

To flash the USBasp bootloader to the DEVICE use the following command:

*note than DEVICE, PROGRAMMER and PORT could be different for you. The supported*
*programmers and ports are: (default device is atmega32)*
```
PROGRAMMER  | PORT
------------+--------------
avrisp      | /dev/ttyACM*
usbasp      | not required - default
usbtiny     | not required
pony-stk200 | not required
```

```
make flash DEVICE=atmega32 PROGRAMMER=avrisp PORT=/dev/ttyACM0
```

then configure the fuses on the device with:

```
make fuse DEVICE=atmega32 PROGRAMMER=avrisp PORT=/dev/ttyACM0
```

## Using a pro micro as ISP flasher

A regular pro micro or similars can be used as ISP flasher to install the
bootlader in the DEVICE, although some changes and preparations are required.

### Prepare the pro micro

To use the pro micro as an ISP flasher first we need to flash the ISP flashing
code to it. To do so flash as regular [this file from the](https://github.com/qmk/qmk_firmware/blob/master/util/pro_micro_ISP_B6_10.hex) [QMK repo](https://github.com/qmk/qmk_firmware/) to your pro
micro.

### Wiring the pro micro

For most of the part the wiring is the same as regular, but instead of connecting
the pro micro's RESET port to our DEVICE reset we'll connect the B10 port. These
is the connection table:

```
Pro Micro 10 (B6)  <-> Keyboard RESET
Pro Micro 15 (B1)  <-> Keyboard B1 (SCLK)
Pro Micro 16 (B2)  <-> Keyboard B2 (MOSI)
Pro Micro 14 (B3)  <-> Keyboard B3 (MISO)
Pro Micro VCC      <-> Keyboard VCC
Pro Micro GND      <-> Keyboard GND
```

Now that we have prepared our pro micro and our DEVICE we can proceed to flash
the bootloader.

### Troubleshooting with the pro micro

* avrdude: ser_open(): can't open device "/dev/ttyACM0": No such file or directory
/dev/ttyACM0 is not the correct port, check with `dmesg -w` when connecting the
pro micro which is the port used.

* avrdude: ser_open(): can't open device "/dev/ttyACM0": Permission denied
Use sudo with the `make` command.

* avrdude stk500_getsync() not in sync resp=0x00
Odly the pro micro seems to be in sync only a brief moment after being plugged,
so unplug it, plug it again and inmediatelly after run the make `command`.
