# MT7621 OpenWrt Build with Complete 5G Modem Support

This OpenWrt build for MT7621 includes comprehensive support for 5G modems and cellular connectivity.

## Included Components

### Kernel Drivers

#### USB Support
- `kmod-usb-core` - Core USB support
- `kmod-usb2` - USB 2.0 support
- `kmod-usb3` - USB 3.0 support

#### USB Serial Drivers for Modems
- `kmod-usb-serial` - Base USB serial support
- `kmod-usb-serial-option` - Option USB serial driver (for most USB modems)
- `kmod-usb-serial-wwan` - WWAN USB serial support
- `kmod-usb-acm` - USB ACM (Abstract Control Model) support

#### USB Network Drivers for Data Connections
- `kmod-usb-net` - Base USB networking support
- `kmod-usb-net-cdc-ether` - CDC Ethernet support
- `kmod-usb-net-cdc-mbim` - MBIM (Mobile Broadband Interface Model) support
- `kmod-usb-net-qmi-wwan` - QMI WWAN support for Qualcomm modems
- `kmod-usb-net-rndis` - RNDIS support

#### WWAN Framework
- `kmod-wwan` - Kernel WWAN framework for modern modem support

### Userspace Tools

#### QMI (Qualcomm MSM Interface) Support
- `libqmi` - QMI library for Qualcomm-based modems
- `qmi-utils` - QMI command-line utilities (qmicli, qmi-network, etc.)
- `uqmi` - Lightweight QMI client for OpenWrt

#### MBIM (Mobile Broadband Interface Model) Support
- `libmbim` - MBIM library for modern modems
- `umbim` - Lightweight MBIM client for OpenWrt

#### Advanced Modem Management
- `modemmanager` - Advanced modem management daemon
- `usb-modeswitch` - USB mode switching for modem initialization

#### Connection Management
- `wwan` - WWAN protocol handler for netifd
- `comgt` - AT command tools for modem control
- `ppp` - PPP daemon for dial-up connections
- `chat` - Chat scripting for PPP

## Supported Modem Types

### 5G Modems
- **Qualcomm X55/X60/X65** - Via QMI interface
- **MediaTek T700/T750** - Via MBIM/QMI interface  
- **Unisoc V510/V516** - Via standard drivers

### 4G/LTE Modems
- **Qualcomm-based** (Sierra Wireless, Quectel, etc.) - Via QMI
- **Huawei** - Via NCM/QMI protocols
- **MediaTek** - Via MBIM interface
- **Broadcom/Cypress** - Via standard CDC drivers

### Specific Modem Examples
- Quectel RM500Q-GL, RM520N-GL (5G)
- Sierra Wireless EM9191, EM9291 (5G)
- Huawei ME909u-521, ME936 (4G)
- MediaTek MT5710 (4G)
- And many others...

## Network Configuration

### QMI Configuration Example
```bash
# Configure QMI interface
uci set network.wwan=interface
uci set network.wwan.proto='qmi'
uci set network.wwan.device='/dev/cdc-wdm0'
uci set network.wwan.apn='internet'
uci commit network
```

### MBIM Configuration Example
```bash
# Configure MBIM interface
uci set network.wwan=interface
uci set network.wwan.proto='mbim'
uci set network.wwan.device='/dev/cdc-wdm0'
uci set network.wwan.apn='internet'
uci commit network
```

### ModemManager Configuration
```bash
# Enable ModemManager
/etc/init.d/modemmanager enable
/etc/init.d/modemmanager start

# List available modems
mmcli -L

# Get modem info
mmcli -m 0
```

## Installation and Usage

1. **Flash the firmware** to your MT7621-based device
2. **Connect your 5G modem** via USB
3. **Check modem detection**: `lsusb` or `dmesg | grep usb`
4. **Configure network** using LuCI web interface or UCI commands
5. **Monitor connection**: `uqmi` or `mmcli` commands

## Troubleshooting

### Modem Not Detected
```bash
# Check USB devices
lsusb

# Check kernel logs
dmesg | grep -i modem

# Try USB mode switching
usb_modeswitch -c /etc/usb_modeswitch.conf
```

### Connection Issues
```bash
# Check QMI status
uqmi -d /dev/cdc-wdm0 --get-data-status

# Check MBIM status  
umbim -d /dev/cdc-wdm0 status

# Check ModemManager
mmcli -m 0 --simple-status
```

## Build Information

This firmware was built with:
- Target: `ramips/mt7621`
- All 5G modem drivers included
- ModemManager with full protocol support
- LuCI web interface with modem management

The build includes support for the latest 5G modem technologies and protocols, ensuring compatibility with most modern cellular modems available in the market.