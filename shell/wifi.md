# Wi-Fi Related Shell Commands for Linux

This document contains a list of useful shell commands for managing Wi-Fi on Linux systems. It includes commands for viewing Wi-Fi details, configuring interfaces, scanning networks, and monitoring connection quality using `iw`, `iwconfig`, `iwlist`, `nmcli`, and other networking tools.  

---  

## 1. General Interface Information  
### List Network Interfaces and Status  

Use the `ip` command to list all network interfaces and their statuses.  ```ip link show   ```

This command displays the current status (up or down), MAC addresses, and interface names for all network interfaces on your system.

### Bring Up or Down a Wi-Fi Interface

You can use ip to enable or disable a specific Wi-Fi interface.

```
ip link set dev wlan0 up   # Bring up the interface  
ip link set dev wlan0 down # Bring down the interface
```
Replace wlan0 with your actual Wi-Fi interface name (found using ip link show).

2\. Display Detailed Wi-Fi Connection Info
------------------------------------------

### Show Current Wi-Fi Connection Details

The iw command provides detailed information about the current Wi-Fi connection.

```
iw dev wlan0 link
```

**Output includes:**

*   Connected SSID (network name)
    
*   Signal strength (RSSI) in dBm
    
*   RX and TX bitrate (transmit and receive speeds)
    

3\. Viewing Wi-Fi Metrics (Signal Quality, Bitrate, etc.)
---------------------------------------------------------

### Get Signal Quality, Bitrate, and Link Quality

Use iwconfig to check Wi-Fi interface signal quality, bitrate, and link quality.

```
iwconfig wlan0
```

**Output includes:**

*   **Bit Rate**: Current transmission speed
    
*   **Link Quality**: Signal quality as a fraction
    
*   **Signal Level**: Signal strength (RSSI) in dBm
    

### Display Detailed Station Statistics

If supported, iw can retrieve advanced metrics on Wi-Fi connection quality.

```
iw dev wlan0 station dump
```

**Output includes:**

*   Signal strength (RSSI)
    
*   TX bitrate (transmission speed)
    
*   Additional values like RSRP and RSRQ if supported (common for cellular modules)
    

4\. Scanning Nearby Wi-Fi Networks
----------------------------------

### Scan for Available Wi-Fi Networks

Use iwlist to scan for available Wi-Fi networks and view signal strengths.

```
sudo iwlist wlan0 scanning | grep -E 'SSID|Signal'
```

This command lists the SSIDs (network names) and signal levels (RSSI in dBm) of nearby Wi-Fi networks.

### View All Network Scan Details

To see the complete output of nearby Wi-Fi networks, including frequency and encryption details, use:

```
sudo iwlist wlan0 scanning
```

5\. Connecting to a Wi-Fi Network
---------------------------------

### Connect to Wi-Fi Using nmcli

The nmcli command from NetworkManager can be used to connect to a Wi-Fi network.

```
nmcli device wifi connect "SSID_NAME" password "YOUR_PASSWORD"
```

*   Replace SSID\_NAME with the Wi-Fi network name.
    
*   Replace YOUR\_PASSWORD with the network password.
    

### List Saved Wi-Fi Networks

To see all saved Wi-Fi networks and connection profiles:

```
nmcli connection show
```

6\. Monitoring Connection Quality
---------------------------------

### Real-Time Signal Strength Monitoring

Use watch with iwconfig to monitor signal strength, link quality, and bitrate in real-time.

```
watch -n 1 "iwconfig wlan0 | grep -i --color 'link quality\|signal level\|bit rate'"
```

### Monitor Connection Details with nmcli

NetworkManager’s nmcli command provides connection details that you can monitor:

```
watch -n 1 nmcli device show wlan0
```

7\. Disconnecting and Reconnecting Wi-Fi
----------------------------------------

### Disconnect from a Wi-Fi Network

To disconnect a specific Wi-Fi interface:

```
nmcli device disconnect wlan0
```

### Reconnect to Wi-Fi

To reconnect to Wi-Fi after disconnection:

```
nmcli device connect wlan0
```

8\. Restarting Network Services
-------------------------------

### Restart NetworkManager Service

If experiencing connection issues, restarting the NetworkManager service can help:

```
sudo systemctl restart NetworkManager
```

### Restart Wi-Fi Interface

To disable and re-enable a specific Wi-Fi interface:

```
sudo ip link set wlan0 down  
sudo ip link set wlan0 up
```

9\. Advanced: Checking Driver and Hardware Information
------------------------------------------------------

### View Wi-Fi Interface Capabilities

Using iw list, you can see the supported features of your Wi-Fi interface.

```
iw list
```

### Display Wi-Fi Hardware Information

To check Wi-Fi interface hardware details:

```
lshw -C network
```

Summary of Tools
----------------

*   **ip**: General interface management and status.
    
*   **iw**: For detailed Wi-Fi connection information.
    
*   **iwconfig**: For signal quality and basic metrics.
    
*   **iwlist**: For scanning available networks.
    
*   **nmcli**: For managing Wi-Fi connections via NetworkManager.