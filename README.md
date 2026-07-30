# VS Code Config Template for STM32

A ready-to-use VS Code configuration template for STM32 embedded development. Drop the `.vscode/` folder into any STM32 project built with GCC + Make + OpenOCD and start building, flashing, and debugging immediately.

![VS Code](https://img.shields.io/badge/VS_Code-007ACC?logo=visualstudiocode&logoColor=white) [![License](https://img.shields.io/badge/license-MIT-blue)](LICENSE)

**English** · [中文](README.zh-CN.md)

## Features

Four configuration files are included in the `.vscode/` folder:

| File | Purpose |
|------|---------|
| `launch.json` | Cortex-Debug configuration — launches GDB via OpenOCD |
| `tasks.json` | Build task (`make`) and flash task (OpenOCD direct) |
| `c_cpp_properties.json` | IntelliSense setup for ARM GCC with HAL defines |
| `settings.json` | Editor settings — tab completion, Git-Bash terminal |

## Requirements

| Tool | Purpose |
|------|---------|
| [ARM GCC Toolchain](https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-rm) (`arm-none-eabi-gcc`) | Cross-compilation |
| [GNU Make](http://gnuwin32.sourceforge.net/packages/make.htm) | Build system |
| [OpenOCD](http://openocd.org/) | On-chip flashing and debug bridge |
| VS Code + [Cortex-Debug](https://marketplace.visualstudio.com/items?itemName=marus25.cortex-debug) extension | IDE and debugger frontend |
| CMSIS-DAP (or similar) debug probe | SWD/JTAG hardware interface |

## Quick Start

### 1. Clone

```bash
git clone https://github.com/turinglambdaai/vsconfig-template-stm32.git
```

### 2. Copy the configuration

Copy the `.vscode/` folder into the root of your STM32 project:

```
your-stm32-project/
├── .vscode/            <-- paste this folder here
│   ├── launch.json
│   ├── tasks.json
│   ├── c_cpp_properties.json
│   └── settings.json
├── Core/
├── Drivers/
├── Makefile
└── ...
```

### 3. Adjust paths and settings

Each file has a few paths you may need to customize for your environment:

#### `launch.json`

- `executable` — Path to your `.elf` file (default: `build/LED.elf`)
- `configFiles` — OpenOCD board config file path (default: DAPLink config)

#### `tasks.json`

- `multi_build` task — Runs `make` with no arguments
- `flash` task — Runs OpenOCD to program the `.elf` (update the `.elf` path and OpenOCD scripts path)

#### `c_cpp_properties.json`

- `compilerPath` — Path to your GCC compiler
- `defines` — HAL and device defines (default: `USE_HAL_DRIVER`, `STM32F103xE`)
- `includePath` — Header search paths (default: all subdirectories)

#### `settings.json`

- `terminal.integrated.shell.windows` — Path to Git-Bash (adjust if installed elsewhere)

### 4. Build and debug

- **Build**: `Ctrl+Shift+B` runs `make`
- **Flash**: Select the "flash" task from the task runner
- **Debug**: Press `F5` to start a Cortex-Debug session via OpenOCD

## Project Structure

```
vsconfig-template-stm32/
├── .vscode/
│   ├── launch.json             # Cortex-Debug config (GDB via OpenOCD)
│   ├── tasks.json              # Build (make) + flash (OpenOCD) tasks
│   ├── c_cpp_properties.json   # IntelliSense for ARM GCC + HAL defines
│   └── settings.json           # Editor settings (Tab completion, Git-Bash)
├── LICENSE
└── README.md
```

## Configuration Details

### launch.json — Debugger

Uses the [Cortex-Debug](https://marketplace.visualstudio.com/items?itemName=marus25.cortex-debug) extension with OpenOCD as the debug server. Key fields:

```jsonc
{
    "executable": "build/LED.elf",     // your ELF binary
    "type": "cortex-debug",
    "servertype": "openocd",
    "configFiles": [                    // OpenOCD board config
        ".../daplink.cfg"
    ]
}
```

Add `"svdFile"` to enable the peripheral register view if you have an SVD file.

### tasks.json — Build & Flash

- **`multi_build`**: Invokes `make` — works with any GCC Makefile-based STM32 project
- **`flash`**: Invokes OpenOCD directly to program, verify, and reset the target

### c_cpp_properties.json — IntelliSense

Configured for `arm-none-eabi-gcc` with standard STM32 HAL defines. Adjust `defines` to match your device (e.g. change `STM32F103xE` to `STM32F407xx`).

### settings.json — Editor

Sets Git-Bash as the integrated terminal on Windows, and enables tab completion.

## Adapting for Other STM32 Families

1. In `c_cpp_properties.json`, change the `defines` (e.g. `STM32F407xx` instead of `STM32F103xE`)
2. In `launch.json`, update the OpenOCD config file for your target (e.g. `stm32f4x.cfg`)
3. In `tasks.json`, update the OpenOCD scripts path for your board config

## License

Licensed under the [MIT License](LICENSE).
