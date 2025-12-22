# Hacking the Lidl Silvercrest Gateway

> **If you find this project useful, please consider giving it a star!** It helps others discover it and motivates continued development.
>
> Questions? Use [Discussions](https://github.com/jnilo1/hacking-lidl-silvercrest-gateway/discussions). Found a bug? Open an [Issue](https://github.com/jnilo1/hacking-lidl-silvercrest-gateway/issues).

## What Is This?

The **Lidl Silvercrest Zigbee Gateway** is an inexpensive device sold as part of a closed ecosystem. Out of the box, it only works with the Tuya cloud and the Silvercrest app.

This project lets you **take full control** of this gateway:

- **Use it with Home Assistant or Zigbee2MQTT** — No cloud, no app, fully local
- **Modern system** — Linux 5.10 kernel with up-to-date tools
- **Secure** — No Tuya cloud connection, SSH access only

______________________________________________________________________

## Quick Start

### What You Need

- A Lidl Silvercrest Zigbee Gateway
- USB-to-serial adapter (3.3V UART)
- Ethernet connection

### Two Chips to Update

The gateway contains **two independent processors**:

| Chip | Role | What to flash |
|------|------|---------------|
| **Realtek RTL8196E** | Main Linux system | Kernel, rootfs, userdata |
| **Silabs EFR32MG1B** | Zigbee radio | NCP firmware |

### Choose Your Path

| | Option 1: Flash Pre-built | Option 2: Build from Source |
|---|---|---|
| **For** | Most users | Developers / Hackers |
| **Time** | ~30 minutes | ~2 hours |
| **Requires** | Python 3, USB-serial adapter | Docker or Ubuntu 22.04 |
| **Use case** | Just want a working local gateway | Customize kernel, rootfs, firmware |

______________________________________________________________________

## Option 1: Flash Pre-built Linux image & Zigbee Firmware

**No build environment needed.** Just flash the pre-built images.

### Step 1: Clone the Repository

```bash
git clone https://github.com/jnilo1/hacking-lidl-silvercrest-gateway.git
cd hacking-lidl-silvercrest-gateway
```

### Step 2: Flash the Linux System (RTL8196E)

Follow the migration guide: [35-Migration](./3-Main-SoC-Realtek-RTL8196E/35-Migration/)

This flashes the pre-built kernel, rootfs, and userdata via TFTP.

### Step 3: Update the Zigbee Firmware (EFR32MG1B)

Use `universal-silabs-flasher` to update the Zigbee radio over the network:

```bash
pip install universal-silabs-flasher
universal-silabs-flasher --device socket://<GATEWAY_IP>:8888 \
    flash --firmware 2-Zigbee-Radio-Silabs-EFR32/24-NCP-UART-HW/firmware/ncp-uart-hw-7.5.1.gbl
```

See [22-Flash-Firmware](./2-Zigbee-Radio-Silabs-EFR32/22-Flash-Firmware/) for detailed instructions.

### Step 4: Connect to Zigbee2MQTT

```yaml
serial:
  port: tcp://<GATEWAY_IP>:8888
```

______________________________________________________________________

## Option 2: Build from Source

**For developers who want to customize the system.**

### Step 1: Set Up the Build Environment

Clone the repository:
```bash
git clone https://github.com/jnilo1/hacking-lidl-silvercrest-gateway.git
cd hacking-lidl-silvercrest-gateway
```

Install the complete toolchain (see [1-Build-Environment](./1-Build-Environment/) for details):

| Approach | Command | Time |
|----------|---------|------|
| **Ubuntu/WSL2** | `cd 1-Build-Environment && sudo ./install_deps.sh` | ~45 min |
| **Docker** | `cd 1-Build-Environment && docker build -t lidl-gateway-builder .` | ~45 min |

### Step 2: Build and Flash the Linux System

**Native:**
```bash
cd 3-Main-SoC-Realtek-RTL8196E
./build_rtl8196e.sh    # Build kernel, rootfs, userdata
./flash_rtl8196e.sh    # Flash via TFTP
```

**Docker** (from project root):
```bash
docker run --rm -v $(pwd):/workspace lidl-gateway-builder \
    /workspace/3-Main-SoC-Realtek-RTL8196E/build_rtl8196e.sh
# Then flash from host (requires TFTP server)
```

### Step 3: Build and Flash the Zigbee Firmware

**Native:**
```bash
cd 2-Zigbee-Radio-Silabs-EFR32/24-NCP-UART-HW
./build_ncp.sh
```

**Docker** (from project root):
```bash
docker run --rm -v $(pwd):/workspace lidl-gateway-builder \
    /workspace/2-Zigbee-Radio-Silabs-EFR32/24-NCP-UART-HW/build_ncp.sh
```

Then flash using `universal-silabs-flasher` (see Option 1, Step 3).

### Step 4: Connect to Zigbee2MQTT

```yaml
serial:
  port: tcp://<GATEWAY_IP>:8888
```

______________________________________________________________________

## Repository Structure

### [0-Hardware](./0-Hardware/)

Hardware documentation: pinouts, debug interfaces, chip specifications.

### [1-Build-Environment](./1-Build-Environment/)

Build environment with Docker or native Ubuntu 22.04/WSL2. Includes all required toolchains:
- **Lexra MIPS** — for Main SoC (RTL8196E)
- **ARM GCC + Silabs slc-cli** — for Zigbee Radio (EFR32)

### [2-Zigbee-Radio-Silabs-EFR32](./2-Zigbee-Radio-Silabs-EFR32/)

Zigbee coprocessor (Silabs EFR32MG1B):

- Backup and restore the original bootloader
- Flash NCP firmware for Zigbee2MQTT/ZHA

### [3-Main-SoC-Realtek-RTL8196E](./3-Main-SoC-Realtek-RTL8196E/)

Main Linux system (Realtek RTL8196E):

- Linux 5.10 kernel
- Root filesystem and user partition
- Flash scripts and migration guide

______________________________________________________________________

## Credits

This project builds upon the initial research by [Paul Banks](https://paulbanks.org/projects/lidl-zigbee/).

## License

MIT License — See [LICENSE](./LICENSE) for details.
