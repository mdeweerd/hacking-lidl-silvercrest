# Building Zigbee Firmware

This section covers how to build firmware for the Silabs EFR32MG1B Zigbee radio chip.

## Two Approaches

| Approach | Type | Best For |
|----------|------|----------|
| [**slc-cli**](./slc-cli/) | Command-line | Automated builds, CI/CD, this project's scripts |
| [**Simplicity Studio**](./simplicity-studio/) | GUI (IDE) | Interactive development, visual debugging |

______________________________________________________________________

## Quick Comparison

### slc-cli (Recommended for this project)

- **Lightweight**: No GUI, runs in terminal or Docker
- **Reproducible**: Same commands produce same output
- **Scriptable**: Used by `build_ncp.sh` and `build_bootloader.sh`
- **Fast setup**: Installed automatically by `install_deps.sh`

```bash
# Example: generate and build NCP firmware
slc generate project.slcp --with EFR32MG1B232F256GM48 --sdk $GECKO_SDK
make -f project.Makefile
```

### Simplicity Studio

- **Full IDE**: Project wizard, code completion, visual configuration
- **Debugging**: Integrated JTAG/SWD debugging with breakpoints
- **Component browser**: Visual selection of SDK components
- **Learning**: Good for understanding the Gecko SDK structure

______________________________________________________________________

## Which One Should I Use?

| Scenario | Recommended |
|----------|-------------|
| Just want to build the firmware for this project | slc-cli (via `build_ncp.sh`) |
| Modifying firmware and need debugging | Simplicity Studio |
| Running in Docker or CI/CD | slc-cli |
| New to Silabs development | Simplicity Studio (for learning) |
| Creating a new project from scratch | Either works |

______________________________________________________________________

## Related Sections

- [23-Bootloader-UART-Xmodem](../23-Bootloader-UART-Xmodem/) — Build the UART bootloader
- [24-NCP-UART-HW](../24-NCP-UART-HW/) — Build the NCP firmware
