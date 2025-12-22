# slc-cli (Silicon Labs Configurator CLI)

`slc-cli` is the command-line interface for the Silicon Labs Configurator. It allows you to generate, configure, and build Gecko SDK projects without a GUI.

> **This is the primary approach used by this project.**

______________________________________________________________________

## Installation

slc-cli is installed automatically as part of the build environment:

| Method | Command |
|--------|---------|
| **Native** | `cd 1-Build-Environment && sudo ./install_deps.sh` |
| **Docker** | `docker build -t lidl-gateway-builder 1-Build-Environment` |

Tools are installed in `<project>/silabs-tools/`:
```
silabs-tools/
├── slc_cli/          # slc-cli itself
├── gecko_sdk/        # Gecko SDK 4.5.0
├── arm-gnu-toolchain/# ARM GCC compiler
└── commander/        # Simplicity Commander
```

______________________________________________________________________

## Basic Usage

### Check Version

```bash
slc --version
```

### Generate a Project

```bash
slc generate project.slcp \
    --sdk /path/to/gecko_sdk \
    --with DEVICE_PART_NUMBER \
    --force
```

### Build the Project

After generation, use `make`:

```bash
make -f project.Makefile release
```

______________________________________________________________________

## Key Commands

| Command | Description |
|---------|-------------|
| `slc generate` | Generate build files from .slcp project |
| `slc signature trust` | Trust an SDK signature (required for SDK 4.x) |
| `slc configuration` | View/modify project configuration |
| `slc --help` | Show all available commands |

______________________________________________________________________

## How This Project Uses slc-cli

### NCP Firmware ([24-NCP-UART-HW](../../24-NCP-UART-HW/))

```bash
cd 2-Zigbee-Radio-Silabs-EFR32/24-NCP-UART-HW
./build_ncp.sh
```

The script runs:
```bash
slc generate ncp-uart-hw.slcp \
    --sdk "${GECKO_SDK}" \
    --with EFR32MG1B232F256GM48 \
    --force

make -f ncp-uart-hw.Makefile release
```

### Bootloader ([23-Bootloader-UART-Xmodem](../../23-Bootloader-UART-Xmodem/))

```bash
cd 2-Zigbee-Radio-Silabs-EFR32/23-Bootloader-UART-Xmodem
./build_bootloader.sh
```

______________________________________________________________________

## Environment Variables

The build scripts auto-detect tools in `silabs-tools/`, but you can also set:

| Variable | Description |
|----------|-------------|
| `GECKO_SDK` | Path to Gecko SDK |
| `ARM_GCC_DIR` | Path to ARM GCC toolchain |
| `PATH` | Must include `slc_cli/`, `arm-gnu-toolchain/bin/`, `commander/` |

To manually set up the environment:
```bash
source silabs-tools/env.sh
```

______________________________________________________________________

## Troubleshooting

### "SDK signature not trusted"

```bash
slc signature trust --sdk /path/to/gecko_sdk
```

### "Command not found: slc"

Ensure slc-cli is in your PATH:
```bash
export PATH="$PWD/silabs-tools/slc_cli:$PATH"
```

### Java errors

slc-cli 5.11+ requires Java 17 or later:
```bash
sudo apt install openjdk-21-jre-headless
```

______________________________________________________________________

## References

- [Silicon Labs slc-cli Documentation](https://docs.silabs.com/simplicity-studio-5-users-guide/latest/ss-5-users-guide-tools-slc-cli/)
- [Gecko SDK on GitHub](https://github.com/SiliconLabs/gecko_sdk)
