# SPI 

This page describes SPI related commands and settings for different hardware platforms

## AMD (former Xilinx)


In order to communication to a SPI device on AMD platform, **spidev** needs to be enabled in device tree. To do this SPI devices should be enabled in **Vivado Design** tool and a new **xsa** file need to be genertaed and used for building software image. The XSA file instructs the build process to automatically generate a device tree based on the system design. This will activate the desired SPI controller with corresponding pin settings. In order to define secondary devices connected to different chip selects, **system-user.dtsi** file needs to be updated with relevant information. The following defines a generic spidev driver that can bes in userspace to communicate over SPI.

```
/* system-user.dtsi */
&spi0 {
	spidev0: spi@0 {
		compatible = "rohm,dh2228fv";
		spi-max-frequency = <2000000>;
		reg = <0>;
	};

	spidev1: spi@1 {
		compatible = "rohm,dh2228fv";
		spi-max-frequency = <2000000>;
		reg = <1>;
	};

	spidev2: spi@2 {
		compatible = "rohm,dh2228fv";
		spi-max-frequency = <2000000>;
		reg = <2>;
	};
};
```

This will add three entries in /dev directory:

```
crw-------    1 root     root      153,   0 May 23 11:22 /dev/spidev0.0
crw-------    1 root     root      153,   1 May 23 11:22 /dev/spidev0.1
crw-------    1 root     root      153,   2 May 23 11:22 /dev/spidev0.2
```

In bash **spitest** command can be used to write and read over SPI:

```
spidev_test -D /dev/spidev0.2 -v -p "\x47\x00\x00"
spi mode: 0x0
bits per word: 8
max speed: 500000 Hz (500 kHz)
TX | 47 00 00 __ __ __ __ __ __ __ __ __ __ __ __ __ __ __ __ __ __ __ __ __ __ __ __ __ __ __  |G..|
RX | 00 30 DE __ __ __ __ __ __ __ __ __ __ __ __ __ __ __ __ __ __ __ __ __ __ __ __ __ __ __  |.0.|
```

This will send a command (0x47) to read some value from a desired register. In this case the device is returning 0x30DE

> It’s very important to remember that in order to read something we need to write 0x00 so the SPI controller generates clock and the client device can respond to us.

