# CAN

Connect CAN device. Send question to CAN device so both TX and RX can be verified.

```
sudo ip link set can0 type can bitrate 20000
sudo ip link set can0 up
cansend can0 5A1#11.2233.44556677.88
```
