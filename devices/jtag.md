# JTAG

# Jtag programming from a Linux host (Ubuntu)

## AMD (former Xilinx)

### 2023.2


To bringup a board using the Petalinux software the following needs to be done:

-   Install petalinux tools
-   Create petalinux project
-   Build Petalinux project


#### Install

```
./petalinux-v2023.2-10121855-installer.run --dir /opt/Xilinx/Petalinux/
```

#### Create petalinux project
For this step to work, a hardware definition file (XSA) has to be reated. This is done via setting up a project in **Vivado** and implement it. One can than export hardware from _File -> Export -> Export Hardware_

```
source /opt/Xilinx/Petalinux/settings.sh
petalinux-create --type project --template zynqMP --name rainbee
petalinux-config --get-hw-description <hardware_definition_file>.xsa
petalinux-build
```

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

