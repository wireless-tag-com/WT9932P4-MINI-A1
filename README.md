# WT9932P4 Mini Example Project

[中文版本](./README_CN.md)

## Project Overview

This is a sample project based on the WT9932P4_Mini development board, featuring a smartphone-like user interface built with the ESP-Brookesia UI framework. The project integrates various application functions including audio/video playback, camera, games, calculator, and supports computer vision features such as face detection and pedestrian detection.

- [WT9932P4-MINI_Guide](./docs/ESP32-P4-MINI-Guide_V1.0.pdf)
- [WT9932P4-MINI development Board schematics](./docs/ESP32-P4-MINI-SCH.pdf)
- [Full-featured test firmware](./firmware/all_function)

## Key Features

- 🎯 **Smartphone-style UI Interface** - Built on ESP-Brookesia framework
- 📱 **Multiple Applications** - Calculator, camera app
- 🤖 **AI Vision Features** - Face detection and pedestrian detection
- 🖥️ **High-definition Display** - Supports MIPI DSI interface display
- 🌐 **Network Connectivity** - Ethernet and 4G module support
- 📷 **Camera Support** - 1280x960 resolution camera
- 💾 **USB Storage Support** - MSC USB mass storage device read/write

## Environment Setup

### Hardware Requirements

- **Development Board**: WT9932P4_Mini development board

![WT9932P4-MINI](./docs/WT9932P4-MINI.png)

- **Display**: MIPI DSI interface display (ek79007)
- **Camera**: OV5647
- **Speaker**: 3W
- **Network Options**: 
  - Ethernet PHY (IP101) with RJ45 connector
  - 4G EC20 module (optional)
- **Storage Options**:
  - USB interface supporting MSC mass storage devices

### Software Environment Setup

#### 1. Install ESP-IDF

Please install ESP-IDF v5.5 (commit: cbe9388f45dd8f33fc560c9727d429e8e107d476) or the latest version according to the official documentation:
- [ESP-IDF Getting Started Guide](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/get-started/index.html)

#### 2. Clone Project

```bash
git clone <project_repository_url>
cd phone_wt9932p4_mini
```

#### 3. Configure Environment Variables

```bash
. ${IDF_PATH}/export.sh
```

#### 4. Install Project Dependencies

```bash
# Install managed component dependencies
idf.py reconfigure
```

#### 5. Build and Flash

```bash
# Configure project
idf.py menuconfig

# Build project
idf.py build

# Flash to device
idf.py flash

# View serial output
idf.py monitor
```

## Network Connectivity Options

### Ethernet Usage (Requires external expansion board)

#### Description

- The development board includes an integrated Ethernet PHY (IP101) with RJ45 connector
- Supports standard 10/100 Mbps Ethernet connection
- Automatic configuration via DHCP

#### Pin Configuration

The Ethernet interface uses the following pins:
- **MDC**: GPIO 31
- **MDIO**: GPIO 52
- **PHY Reset**: GPIO 51
- **PHY Address**: 1

#### Usage

Enable Ethernet support in menuconfig:

```bash
(Top) → Example Configuration → Use eth demo
```

### 4G Module Usage (Optional)

#### Description

- Support for 4G EC20 module via USB interface
- Provides cellular network connectivity
- Includes ping functionality for network testing

#### Configuration

Enable 4G support in menuconfig:

```bash
(Top) → Example Configuration → Use 4G EC20 demo
```

### MSC USB Storage Usage

#### Description

- Support for hot-plug USB mass storage devices (MSC)
- Automatic mounting to virtual file system (VFS)
- Support for multiple USB storage devices simultaneously
- Complete file system functionality including file read/write and directory operations
- Includes read/write speed testing and device information display

#### Features

- **Automatic Detection and Mounting**: USB devices are automatically detected and mounted to `/usb0`, `/usb1`, etc. when plugged in
- **Multi-device Support**: Simultaneous support for multiple USB storage devices
- **File Operations**: Support for creating, reading, writing, and deleting files and directories
- **Device Information**: Display device capacity, vendor ID, product ID, and other information
- **Performance Testing**: Built-in read/write speed testing functionality

#### Usage

1. Enable MSC USB support in menuconfig:

```bash
(Top) → Example Configuration → Use MSC USB demo
```

2. After compiling and flashing the firmware, plug in a USB storage device
3. The system will automatically detect the device and mount it to the `/usb0` directory
4. You can view device information and file operation status through serial log output

#### Supported Devices

- USB 2.0/3.0 flash drives
- USB external hard drives
- USB card readers (with storage cards)
- Other storage devices that comply with USB MSC standards

## Project Directory Structure

```
phone_wt9932p4_mini/
├── main/                           # Main program source code
│   ├── main.cpp                    # Program entry point
│   ├── CMakeLists.txt              # Main program build configuration
│   └── idf_component.yml           # Component dependency configuration
├── components/                     # Custom components
│   ├── apps/                       # Application components
│   │   ├── calculator/             # Calculator application
│   │   └── camera/                 # Camera application
│   ├── human_face_detect/          # Face detection component
│   ├── pedestrian_detect/          # Pedestrian detection component
│   └── msc_usb/                    # MSC USB storage device component
├── CMakeLists.txt                  # Top-level build configuration
├── sdkconfig.defaults              # Default SDK configuration
├── partitions.csv                  # Partition table configuration
└── README.md                       # Project documentation
```

### Core Component Description

#### 1. Main Program (`main/`)
- **main.cpp**: Program entry point, initializes system, display, storage, network modules, and starts various applications

#### 2. Applications (`components/apps/`)
- **calculator/**: Calculator application supporting basic arithmetic operations
- **camera/**: Camera application supporting photo capture and video preview

#### 3. AI Vision Components
- **human_face_detect/**: Face detection algorithm implementation
- **pedestrian_detect/**: Pedestrian detection algorithm implementation

#### 4. Storage and Communication Components
- **msc_usb/**: MSC USB mass storage device driver supporting hot-plug and multi-device management

## Partition Configuration

>[!INFO]
>**Custom Partition Table Description (`partitions.csv`)**
>- **nvs** (24KB): Non-volatile storage for configuration data
>- **phy_init** (4KB): RF calibration data
>- **factory** (9MB): Application firmware

## Development and Debugging

### View Logs
```bash
idf.py monitor
```

### Common Configuration Options
Configure through `idf.py menuconfig`:
- Display parameter settings
- Camera resolution configuration
- Network configuration (Ethernet, 4G)
- MSC USB storage device support

## Component Library Version Requirements

### Core Framework Dependencies
| Component Name | Version Requirement | Description |
|----------------|-------------------|-------------|
| **ESP-IDF** | v5.5.0 (commit: cbe9388f45dd8f33fc560c9727d429e8e107d476) | ESP32 development framework |
| **espressif/esp-brookesia** | 0.4.2 | Smartphone-style UI framework |
| **lvgl/lvgl** | 8.4.0 | Lightweight graphics library |
| **espressif/esp_lvgl_port** | 2.6.0 | LVGL porting layer |



### Display and Touch Components
| Component Name | Version Requirement | Target Chips | Description |
|----------------|-------------------|--------------|-------------|
| **espressif/esp_lcd_ek79007** | 1.0.2 | ESP32P4 | EK79007 display driver |
| **espressif/esp_lcd_ili9881c** | 1.0.1 | ESP32P4 | ILI9881C display driver |
| **espressif/esp_lcd_lt8912b** | 0.1.1 | ESP32P4 | LT8912B (MIPI DSI-HDMI) display driver |
| **espressif/esp_lcd_touch** | 1.1.2 | General | Touch screen base driver |
| **espressif/esp_lcd_touch_gt911** | 1.1.3 | General | GT911 touch controller |

### Camera Components
| Component Name | Version Requirement | Target Chips | Description |
|----------------|-------------------|--------------|-------------|
| **espressif/esp_cam_sensor** | 0.9.0 | ESP32P4 | Camera sensor driver |
| **espressif/esp_sccb_intf** | 0.0.5 | General | SCCB interface driver |



### AI and Deep Learning Components
| Component Name | Version Requirement | Target Chips | Description |
|----------------|-------------------|--------------|-------------|
| **espressif/esp-dl** | 3.1.0 | ESP32S3/P4 | ESP deep learning inference framework |

### USB and Storage Components
| Component Name | Version Requirement | Target Chips | Description |
|----------------|-------------------|--------------|-------------|
| **usb_host_msc** | 1.1.1 | ESP32S2/S3/P4 | USB host MSC storage device driver |

### Network and Communication Components
| Component Name | Version Requirement | Target Chips | Description |
|----------------|-------------------|--------------|-------------|
| **espressif/eppp_link** | 0.3.1 | General | PPP protocol link |
| **espressif/esp_serial_slave_link** | 1.1.0~1 | General | Serial slave device link |
| **espressif/iot_usbh_modem** | 1.2.0 | ESP32P4 | USB modem board support |
| **espressif/iot_usbh_cdc** | 2.0.1 | ESP32S2/S3/P4 | USB host CDC driver |
| **espressif/esp_hosted** | 2.0.13 | General | ESP-Hosted MCU driver |
| **espressif/esp_wifi_remote** | 0.14.3 | General | WiFi remote function encapsulation |

### Tools and Utility Components
| Component Name | Version Requirement | Description |
|----------------|-------------------|-------------|
| **espressif/cmake_utilities** | 0.5.3 | CMake build tools |
| **espressif/json_parser** | 1.0.3 | JSON parser |
| **espressif/jsmn** | 1.1.0 | Lightweight JSON parser |

### Version Compatibility Notes

>[!IMPORTANT]
>**Critical Version Requirements**
>1. **ESP-IDF Version**: Recommend using v5.5 specific commit version to ensure compatibility of all components
>2. **Target Chips**: Primarily targets ESP32P4 chip, some components also support ESP32S3
>3. **Dependencies**: Some components have interdependencies, please ensure version matching

>[!CAUTION]
>**Update Notice**: When upgrading component versions, please check dependencies and compatibility to avoid incompatibility issues

### Installing Specific Version Components

To install specific versions of components, specify in `idf_component.yml`:

```yaml
dependencies:
  espressif/esp-brookesia:
    version: "0.4.2"
  espressif/esp_video:
    version: "0.8.0~3"
    rules:
      - if: "target == esp32p4"
```
## log output

```bash
ESP-ROM:esp32p4-eco2-20240710
Build:Jul 10 2024
rst:0x1 (POWERON),boot:0x10f (SPI_FAST_FLASH_BOOT)
SPI mode:DIO, clock div:1
load:0x4ff33ce0,len:0x17a4
load:0x4ff29ed0,len:0xf28
--- 0x4ff29ed0: esp_bootloader_get_description at /home/ferry/esp/idf55/components/esp_bootloader_format/esp_bootloader_desc.c:39

load:0x4ff2cbd0,len:0x3454
--- 0x4ff2cbd0: esp_flash_encryption_enabled at /home/ferry/esp/idf55/components/bootloader_support/src/flash_encrypt.c:89

entry 0x4ff29eda
--- 0x4ff29eda: call_start_cpu0 at /home/ferry/esp/idf55/components/bootloader/subproject/main/bootloader_start.c:25

I (25) boot: ESP-IDF v5.5-beta1-204-gcbe9388f45 2nd stage bootloader
I (26) boot: compile time Jul  7 2025 17:54:32
I (26) boot: Multicore bootloader
I (29) boot: chip revision: v1.0
I (30) boot: efuse block revision: v0.3
I (34) qio_mode: Enabling default flash chip QIO
I (38) boot.esp32p4: SPI Speed      : 80MHz
I (42) boot.esp32p4: SPI Mode       : QIO
I (46) boot.esp32p4: SPI Flash Size : 16MB
I (50) boot: Enabling RNG early entropy source...
I (54) boot: Partition Table:
I (57) boot: ## Label            Usage          Type ST Offset   Length
I (63) boot:  0 nvs              WiFi data        01 02 00009000 00006000
I (70) boot:  1 phy_init         RF data          01 01 0000f000 00001000
I (76) boot:  2 factory          factory app      00 00 00010000 00900000
I (84) boot: End of partition table
I (86) esp_image: segment 0: paddr=00010020 vaddr=481b0020 size=305548h (3167560) map
I (578) esp_image: segment 1: paddr=00315570 vaddr=30100000 size=00088h (   136) load
I (580) esp_image: segment 2: paddr=00315600 vaddr=4ff00000 size=0aa18h ( 43544) load
I (591) esp_image: segment 3: paddr=00320020 vaddr=48000020 size=1a9a60h (1743456) map
I (859) esp_image: segment 4: paddr=004c9a88 vaddr=4ff0aa18 size=11830h ( 71728) load
I (874) esp_image: segment 5: paddr=004db2c0 vaddr=4ff1c280 size=03840h ( 14400) load
I (878) esp_image: segment 6: paddr=004deb08 vaddr=50108080 size=00020h (    32) load
I (886) boot: Loaded app from partition at offset 0x10000
I (886) boot: Disabling RNG early entropy source...
I (899) hex_psram: vendor id    : 0x0d (AP)
I (899) hex_psram: Latency      : 0x01 (Fixed)
I (900) hex_psram: DriveStr.    : 0x00 (25 Ohm)
I (900) hex_psram: dev id       : 0x03 (generation 4)
I (905) hex_psram: density      : 0x07 (256 Mbit)
I (909) hex_psram: good-die     : 0x06 (Pass)
I (913) hex_psram: SRF          : 0x02 (Slow Refresh)
I (918) hex_psram: BurstType    : 0x00 ( Wrap)
I (922) hex_psram: BurstLen     : 0x03 (2048 Byte)
I (927) hex_psram: BitMode      : 0x01 (X16 Mode)
I (931) hex_psram: Readlatency  : 0x04 (14 cycles@Fixed)
I (936) hex_psram: DriveStrength: 0x00 (1/1)
I (941) MSPI DQS: tuning success, best phase id is 0
I (1113) MSPI DQS: tuning success, best delayline id is 16
I esp_psram: Found 32MB PSRAM device
I esp_psram: Speed: 200MHz
I (1265) mmu_psram: .rodata xip on psram
I (1348) mmu_psram: .text xip on psram
I (1348) hex_psram: psram CS IO is dedicated
I (1349) cpu_start: Multicore app
I (1740) esp_psram: SPI SRAM memory test OK
I (1749) cpu_start: Pro cpu start user code
I (1749) cpu_start: cpu freq: 360000000 Hz
I (1750) app_init: Application information:
I (1750) app_init: Project name:     wt9932p4_mini
I (1754) app_init: App version:      1.0.1
I (1758) app_init: Compile time:     Jul  7 2025 17:54:12
I (1763) app_init: ELF file SHA256:  78740a42f...
I (1768) app_init: ESP-IDF:          v5.5-beta1-204-gcbe9388f45
I (1773) efuse_init: Min chip rev:     v0.1
I (1777) efuse_init: Max chip rev:     v1.99 
I (1781) efuse_init: Chip rev:         v1.0
I (1785) heap_init: Initializing. RAM available for dynamic allocation:
I (1792) heap_init: At 4FF227C0 len 00018800 (98 KiB): RAM
I (1797) heap_init: At 4FF3AFC0 len 00004BF0 (18 KiB): RAM
I (1802) heap_init: At 4FF40000 len 00040000 (256 KiB): RAM
I (1808) heap_init: At 501080A0 len 00007F60 (31 KiB): RTCRAM
I (1813) heap_init: At 30100088 len 00001F78 (7 KiB): TCM
I (1818) esp_psram: Adding pool of 27904K of PSRAM memory to heap allocator
I (1825) esp_psram: Adding pool of 25K of PSRAM memory gap generated due to end address alignment of irom to the heap allocator
I (1836) esp_psram: Adding pool of 42K of PSRAM memory gap generated due to end address alignment of drom to the heap allocator
I (1847) spi_flash: detected chip: generic
I (1851) spi_flash: flash io: qio
I (1854) host_init: ESP Hosted : Host chip_ip[18]
I (1868) H_API: ESP-Hosted starting. Hosted_Tasks: prio:23, stack: 5120 RPC_task_stack: 5120
sdio_mempool_create free:28769416 min-free:28769416 lfb-def:28311552 lfb-8bit:28311552

I (1875) H_API: ** add_esp_wifi_remote_channels **
I (1879) transport: Add ESP-Hosted channel IF[1]: S[0] Tx[0x4800cc1c] Rx[0x4801bb26]
--- 0x4800cc1c: transport_drv_sta_tx at /home/ferry/workpro/phone_wt9932p4_mini/managed_components/espressif__esp_hosted/host/drivers/transport/transport_drv.c:219
--- 0x4801bb26: esp_wifi_remote_channel_rx at /home/ferry/workpro/phone_wt9932p4_mini/managed_components/espressif__esp_wifi_remote/esp_wifi_remote_net.c:19

I (1886) transport: Add ESP-Hosted channel IF[2]: S[0] Tx[0x4800cb62] Rx[0x4801bb26]
--- 0x4800cb62: transport_drv_ap_tx at /home/ferry/workpro/phone_wt9932p4_mini/managed_components/espressif__esp_hosted/host/drivers/transport/transport_drv.c:249
--- 0x4801bb26: esp_wifi_remote_channel_rx at /home/ferry/workpro/phone_wt9932p4_mini/managed_components/espressif__esp_wifi_remote/esp_wifi_remote_net.c:19

I (1894) main_task: Started on CPU0
I (1897) main_task: Calling app_main()
I (1904) modem_board: iot_usbh_modem, version: 1.2.0
I (1905) modem_board: Force reset modem board....
I (1909) modem_board: Resetting modem using io=5, level=1
I (2115) modem_board: Waiting for modem initialize ready
I (7115) USBH_CDC: iot usbh cdc version: 2.0.1
I (7145) esp-modem: --------- Modem PreDefined Info ------------------
I (7145) esp-modem: Model: EC20_CE
I (7145) esp-modem: Modem itf 2
I (7146) esp-modem: ----------------------------------------------------
W (7533) EXT_HUB: Transaction Translator has not been implemented yet
W (7533) EXT_HUB: Device has several Interfaces, selection has not been implemented yet. Using first.
Found NOTIF endpoint: 133
Found IN endpoint: 132
Found OUT endpoint: 3
*** Device descriptor ***
bLength 18
bDescriptorType 1
bcdUSB 2.00
bDeviceClass 0xef
bDeviceSubClass 0x2
bDeviceProtocol 0x1
bMaxPacketSize0 64
idVendor 0x2c7c
idProduct 0x125
bcdDevice 3.10
iManufacturer 1
iProduct 2
iSerialNumber 3
bNumConfigurations 1
*** Configuration descriptor ***
bLength 9
bDescriptorType 2
wTotalLength 209
bNumInterfaces 5
bConfigurationValue 1
iConfiguration 0
bmAttributes 0xa0
bMaxPower 500mA
        *** Interface descriptor ***
        bLength 9
        bDescriptorType 4
        bInterfaceNumber 0
        bAlternateSetting 0
        bNumEndpoints 2
        bInterfaceClass 0xff
        bInterfaceSubClass 0xff
        bInterfaceProtocol 0xff
        iInterface 0
                *** Endpoint descriptor ***
                bLength 7
                bDescriptorType 5
                bEndpointAddress 0x81   EP 1 IN
                bmAttributes 0x2        BULK
                wMaxPacketSize 512
                bInterval 0
                *** Endpoint descriptor ***
                bLength 7
                bDescriptorType 5
                bEndpointAddress 0x1    EP 1 OUT
                bmAttributes 0x2        BULK
                wMaxPacketSize 512
                bInterval 0
        *** Interface descriptor ***
        bLength 9
        bDescriptorType 4
        bInterfaceNumber 1
        bAlternateSetting 0
        bNumEndpoints 3
        bInterfaceClass 0xff
        bInterfaceSubClass 0x0
        bInterfaceProtocol 0x0
        iInterface 0
                *** Endpoint descriptor ***
                bLength 7
                bDescriptorType 5
                bEndpointAddress 0x83   EP 3 IN
                bmAttributes 0x3        INT
                wMaxPacketSize 10
                bInterval 9
                *** Endpoint descriptor ***
                bLength 7
                bDescriptorType 5
                bEndpointAddress 0x82   EP 2 IN
                bmAttributes 0x2        BULK
                wMaxPacketSize 512
                bInterval 0
                *** Endpoint descriptor ***
                bLength 7
                bDescriptorType 5
                bEndpointAddress 0x2    EP 2 OUT
                bmAttributes 0x2        BULK
                wMaxPacketSize 512
                bInterval 0
        *** Interface descriptor ***
        bLength 9
        bDescriptorType 4
        bInterfaceNumber 2
        bAlternateSetting 0
        bNumEndpoints 3
        bInterfaceClass 0xff
        bInterfaceSubClass 0x0
        bInterfaceProtocol 0x0
        iInterface 0
                *** Endpoint descriptor ***
                bLength 7
                bDescriptorType 5
                bEndpointAddress 0x85   EP 5 IN
                bmAttributes 0x3        INT
                wMaxPacketSize 10
                bInterval 9
                *** Endpoint descriptor ***
                bLength 7
                bDescriptorType 5
                bEndpointAddress 0x84   EP 4 IN
                bmAttributes 0x2        BULK
                wMaxPacketSize 512
                bInterval 0
                *** Endpoint descriptor ***
                bLength 7
                bDescriptorType 5
                bEndpointAddress 0x3    EP 3 OUT
                bmAttributes 0x2        BULK
                wMaxPacketSize 512
                bInterval 0
        *** Interface descriptor ***
        bLength 9
        bDescriptorType 4
        bInterfaceNumber 3
        bAlternateSetting 0
        bNumEndpoints 3
        bInterfaceClass 0xff
        bInterfaceSubClass 0x0
        bInterfaceProtocol 0x0
        iInterface 0
                *** Endpoint descriptor ***
                bLength 7
                bDescriptorType 5
                bEndpointAddress 0x87   EP 7 IN
                bmAttributes 0x3        INT
                wMaxPacketSize 10
                bInterval 9
                *** Endpoint descriptor ***
                bLength 7
                bDescriptorType 5
                bEndpointAddress 0x86   EP 6 IN
                bmAttributes 0x2        BULK
                wMaxPacketSize 512
                bInterval 0
                *** Endpoint descriptor ***
                bLength 7
                bDescriptorType 5
                bEndpointAddress 0x4    EP 4 OUT
                bmAttributes 0x2        BULK
                wMaxPacketSize 512
                bInterval 0
        *** Interface descriptor ***
        bLength 9
        bDescriptorType 4
        bInterfaceNumber 4
        bAlternateSetting 0
        bNumEndpoints 3
        bInterfaceClass 0xff
        bInterfaceSubClass 0xff
        bInterfaceProtocol 0xff
        iInterface 0
                *** Endpoint descriptor ***
                bLength 7
                bDescriptorType 5
                bEndpointAddress 0x89   EP 9 IN
                bmAttributes 0x3        INT
                wMaxPacketSize 8
                bInterval 9
                *** Endpoint descriptor ***
                bLength 7
                bDescriptorType 5
                bEndpointAddress 0x88   EP 8 IN
                bmAttributes 0x2        BULK
                wMaxPacketSize 512
                bInterval 0
                *** Endpoint descriptor ***
                bLength 7
                bDescriptorType 5
                bEndpointAddress 0x5    EP 5 OUT
                bmAttributes 0x2        BULK
                wMaxPacketSize 512
                bInterval 0
I (12672) modem_board: DTE reconnect, reconnecting ...

I (12677) main: Modem Board Event: USB connected
I (12681) USBH_CDC: Opened cdc device: 2
I (12685) USBH_CDC: New device connected, address: 2
I (13677) modem_board: reconnect after 5s...
I (14677) modem_board: reconnect after 4s...
I (15677) modem_board: reconnect after 3s...
I (16677) modem_board: reconnect after 2s...
I (17677) modem_board: reconnect after 1s...
I (17677) modem_board: Modem state STAGE_SYNC, Start
E (17694) esp-modem-dte: esp_dte_handle_line(79): handle line failed
I (17694) modem_board: Network Auto reconnecting ...
I (17694) modem_board: Modem state STAGE_SYNC, Success!
W (17694) main: Modem Board Event: Network disconnected
I (17799) modem_board: Modem state STAGE_CHECK_SIM, Start
I (17805) modem_board: SIM Card Ready
I (17805) modem_board: Modem state STAGE_CHECK_SIM, Success!
I (17805) main: Modem Board Event: SIM Card Connected
I (17905) modem_board: Modem state STAGE_CHECK_SIGNAL, Start
I (17908) modem_board: Signal quality: rssi=11, ber=99
I (17908) modem_board: Modem state STAGE_CHECK_SIGNAL, Success!
I (18009) modem_board: Modem state STAGE_CHECK_REGIST, Start
I (18014) modem_board: Network registered, Operator: "CHN-CT"
I (18014) modem_board: Modem state STAGE_CHECK_REGIST, Success!
I (18114) modem_board: Modem state STAGE_START_PPP, Start
I (18131) modem_board: Modem state STAGE_START_PPP, Success!
I (18231) modem_board: Modem state STAGE_WAIT_IP, Start
W (18231) modem_board: Modem event! 0
I (18460) esp-netif_lwip-ppp: ppp: Connected
I (18460) modem_board: IP event! 6
I (18460) modem_board: Modem Connected to PPP Server
I (18461) modem_board: ppp ip: 10.47.107.47, mask: 255.255.255.255, gw: 10.64.64.64
I (18468) modem_board: Main DNS: 202.96.128.86
I (18472) modem_board: Backup DNS: 202.96.134.133
I (18477) modem_board: Modem state STAGE_WAIT_IP, Success!
I (18477) esp-modem-netif: PPP state changed event 0: (NETIF_PPP_ERRORNONE)
I (18489) main: Modem Board Event: Network connected
I (18523) esp_eth.netif.netif_glue: 30:ed:a0:e0:ce:3a
I (18523) esp_eth.netif.netif_glue: ethernet attached to netif
I (20424) main: Ethernet Started
I (20424) LVGL: Starting LVGL task
I (20424) main: Ethernet Link Up
I (20424) main: Ethernet HW Addr 30:ed:a0:e0:ce:3a
W (20426) ledc: GPIO 26 is not usable, maybe conflict with others
I (20432) ESP32_P4_EV: MIPI DSI PHY Powered on
I (20437) ESP32_P4_EV: Install MIPI DSI LCD control panel
I (20441) ESP32_P4_EV: Install EK79007 LCD control panel
I (20446) ek79007: version: 1.0.2
I (20613) ESP32_P4_EV: Display initialized
I (20613) ESP32_P4_EV: Display resolution 1024x600
E (20614) lcd_panel: esp_lcd_panel_swap_xy(50): swap_xy is not supported by this panel
W (20618) i2c.master: Please check pull-up resistances whether be connected properly. Otherwise unexpected behavior would happen. For more detailed information, please read docs
I (20633) GT911: I2C address initialization procedure skipped - using default GT9xx setup
I (20641) GT911: TouchPad_ID:0x39,0x31,0x31
I (20645) GT911: TouchPad_Config_Version:89
I (20649) ESP32_P4_EV: Setting LCD backlight: 100%
[WARN] [esp_brookesia_core.cpp:46](getDisplaySize): Display is not set, use default display
[INFO] [esp_brookesia_core.cpp:204](beginCore): Library version: 0.4.1
[WARN] [esp_brookesia_phone_manager.cpp:72](begin): No touch device is set, try to use default touch device
[WARN] [esp_brookesia_phone_manager.cpp:76](begin): Using default touch device(@0x0x484b742c)
I (20707) ov5647: Detected Camera sensor PID=0x5647
I (20769) app_video: version: 0.8.0
I (20769) app_video: driver:  MIPI-CSI
I (20769) app_video: card:    MIPI-CSI
I (20769) app_video: bus:     esp32p4:MIPI-CSI
I (20772) app_video: width=1280 height=960
I (20788) app_camera_pipeline: new elements[0]:0x48e20d04, internal:1
I (20800) app_camera_pipeline: new elements[1]:0x48fe8d08, internal:1
I (20812) app_camera_pipeline: new elements[2]:0x491b0d0c, internal:1
I (20824) app_camera_pipeline: new elements[3]:0x49378d10, internal:1
I (20825) app_camera_pipeline: new pipeline 0x484b9388, elem_num:4
I (20826) app_camera_pipeline: new elements[0]:0x484b9498, internal:1
I (20832) app_camera_pipeline: new elements[1]:0x484b94ec, internal:1
I (20838) app_camera_pipeline: new elements[2]:0x484b9540, internal:1
I (20844) app_camera_pipeline: new elements[3]:0x484b9594, internal:1
I (20850) app_camera_pipeline: new pipeline 0x484b9410, elem_num:4

I (23552) modem_board: IP event! 4
I (23552) esp_netif_handlers: eth ip: 192.168.10.211, mask: 255.255.255.0, gw: 192.168.10.15
I (23552) main: Ethernet Got IP Address
I (23555) main: ~~~~~~~~~~~
I (23558) main: ETHIP:192.168.10.211
I (23561) main: ETHMASK:255.255.255.0
I (23565) main: ETHGW:192.168.10.15
I (23568) main: ~~~~~~~~~~~
I (30860) main: Ping addr 8.8.8.8 Restart..
I (30871) main: 64 bytes from 8.8.8.8 icmp_seq=1 ttl=114 time=11 ms

## MSC USB Log Example

When a USB storage device is plugged in, you will see log output similar to the following:

```bash
I (15234) MSC_USB: Waiting for USB flash drive to be connected
I (18456) MSC_USB: MSC device connected (usb_addr=1)
*** Device descriptor ***
bLength 18
bDescriptorType 1
bcdUSB 2.00
bDeviceClass 0x0
bDeviceSubClass 0x0
bDeviceProtocol 0x0
bMaxPacketSize0 64
idVendor 0x0951
idProduct 0x1666
bcdDevice 1.10
iManufacturer 1
iProduct 2
iSerialNumber 3
bNumConfigurations 1
Device info:
	 Capacity: 7628 MB
	 Sector size: 512
	 Sector count: 15625216
	 PID: 0x1666
	 VID: 0x0951
I (18567) MSC_USB: Listing contents of /usb0
/usb0/.
/usb0/..
I (18589) MSC_USB: Creating file
I (18592) MSC_USB: Reading file
I (18595) MSC_USB: Read from file '/usb0/esp/test.txt': 'Hello World!'
I (18602) MSC_USB: Writing to file /usb0/esp/dummy
I (19234) MSC_USB: Write speed 8.45 MiB/s
I (19456) MSC_USB: Reading from file /usb0/esp/dummy
I (19678) MSC_USB: Read speed 12.34 MiB/s
I (19689) MSC_USB: Example finished, you can disconnect the USB flash drive
```
```
