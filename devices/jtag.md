# JTAG

# Jtag programming from a Linux host (Ubuntu)

## AMD (former Xilinx)

### 2023.2


To bring a board up using the Petalinux, software must be [built](../platforms/amd.md)


#### Programming the board

In order to program the board over jtag the following should have been done:

-   Set the SW6 switch on the board to **all on**
-   Run Vivado as **sudo**
-   Connect to target on local host
-   Select the Xilinx board as target

Then on the petalinux project dirctory, run the following:

```
source /opt/Xilinx/Petalinux/settings.sh
petalinux-boot --jtag --kernel --hw_server-url 127.0.0.1:3121
```

This will laod FSBL, ATF, PMU firmwares together with u-boot, kernel, device tree and ramfs. From there we can download images from a network server and stream it into eMMC

