
# AMD (Former Xilinx)
## Building a Linux image based on the Petalinux

### 2023.2


Setup and build Petalinux:

-   Install petalinux tools
-   Create petalinux project
-   Build Petalinux project


#### Install petalinux tools

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

The board can then be programmed with a RAMFS using the following [this](../devices/jtag.md#programming-the-board):


