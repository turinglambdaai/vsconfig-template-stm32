# VS Code Config Template for STM32

> [!WARNING]
> **已弃用** — 本模板已被 [led-gcc-stm32](https://github.com/turinglambdaai/led-gcc-stm32) 取代。该仓库是一个开箱即用的完整 STM32 工程，内置同样的 VS Code 构建 / 烧录 / 调试工作流，且配置保持最新。请前往该仓库使用。

一套即拿即用的 VS Code 配置模板，专为 STM32 嵌入式开发设计。将 `.vscode/` 文件夹放入任何基于 GCC + Make + OpenOCD 的 STM32 工程中，即可立即开始构建、烧录和调试。

![VS Code](https://img.shields.io/badge/VS_Code-007ACC?logo=visualstudiocode&logoColor=white) [![License](https://img.shields.io/badge/license-MIT-blue)](LICENSE)

[English](README.md) · **中文**

## 功能特性

`.vscode/` 文件夹包含四个配置文件：

| 文件 | 用途 |
|------|------|
| `launch.json` | Cortex-Debug 配置 — 通过 OpenOCD 启动 GDB 调试 |
| `tasks.json` | 构建任务（`make`）和烧录任务（OpenOCD 直烧） |
| `c_cpp_properties.json` | ARM GCC IntelliSense 配置，含 HAL 宏定义 |
| `settings.json` | 编辑器设置 — Tab 补全、Git-Bash 终端 |

## 环境要求

| 工具 | 用途 |
|------|------|
| [ARM GCC 工具链](https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-rm)（`arm-none-eabi-gcc`） | 交叉编译 |
| [GNU Make](http://gnuwin32.sourceforge.net/packages/make.htm) | 构建系统 |
| [OpenOCD](http://openocd.org/) | 片上烧录与调试桥接 |
| VS Code + [Cortex-Debug](https://marketplace.visualstudio.com/items?itemName=marus25.cortex-debug) 扩展 | IDE 和调试前端 |
| CMSIS-DAP（或类似）调试器 | SWD/JTAG 硬件接口 |

## 快速开始

### 1. 克隆仓库

```bash
git clone https://github.com/turinglambdaai/vsconfig-template-stm32.git
```

### 2. 复制配置

将 `.vscode/` 文件夹复制到你的 STM32 工程根目录：

```
your-stm32-project/
├── .vscode/            <-- 将此文件夹粘贴到这里
│   ├── launch.json
│   ├── tasks.json
│   ├── c_cpp_properties.json
│   └── settings.json
├── Core/
├── Drivers/
├── Makefile
└── ...
```

### 3. 调整路径和设置

每个文件中可能需要根据你的环境修改部分路径：

#### `launch.json`

- `executable` — `.elf` 文件路径（默认：`build/LED.elf`）
- `configFiles` — OpenOCD 板级配置文件路径（默认：DAPLink 配置）

#### `tasks.json`

- `multi_build` 任务 — 运行 `make`（无参数）
- `flash` 任务 — 运行 OpenOCD 烧录 `.elf`（需更新 `.elf` 路径和 OpenOCD 脚本路径）

#### `c_cpp_properties.json`

- `compilerPath` — GCC 编译器路径
- `defines` — HAL 和设备宏定义（默认：`USE_HAL_DRIVER`、`STM32F103xE`）
- `includePath` — 头文件搜索路径（默认：所有子目录）

#### `settings.json`

- `terminal.integrated.shell.windows` — Git-Bash 路径（如安装在其他位置请修改）

### 4. 构建与调试

- **构建**：`Ctrl+Shift+B` 执行 `make`
- **烧录**：从任务运行器中选择 "flash" 任务
- **调试**：按 `F5` 通过 OpenOCD 启动 Cortex-Debug 调试会话

## 项目结构

```
vsconfig-template-stm32/
├── .vscode/
│   ├── launch.json             # Cortex-Debug 配置（GDB via OpenOCD）
│   ├── tasks.json              # 构建（make）+ 烧录（OpenOCD）任务
│   ├── c_cpp_properties.json   # ARM GCC IntelliSense 配置，含 HAL 宏
│   └── settings.json           # 编辑器设置（Tab 补全、Git-Bash）
├── LICENSE
└── README.md
```

## 配置详解

### launch.json — 调试器

使用 [Cortex-Debug](https://marketplace.visualstudio.com/items?itemName=marus25.cortex-debug) 扩展，以 OpenOCD 作为调试服务器。关键字段：

```jsonc
{
    "executable": "build/LED.elf",     // 你的 ELF 二进制文件
    "type": "cortex-debug",
    "servertype": "openocd",
    "configFiles": [                    // OpenOCD 板级配置
        ".../daplink.cfg"
    ]
}
```

如果有 SVD 文件，添加 `"svdFile"` 字段即可启用外设寄存器视图。

### tasks.json — 构建与烧录

- **`multi_build`**：调用 `make` — 适用于任何基于 GCC Makefile 的 STM32 工程
- **`flash`**：直接调用 OpenOCD 进行编程、校验和复位

### c_cpp_properties.json — IntelliSense

为 `arm-none-eabi-gcc` 配置，包含标准 STM32 HAL 宏定义。修改 `defines` 以匹配你的设备（例如将 `STM32F103xE` 改为 `STM32F407xx`）。

### settings.json — 编辑器

在 Windows 上设置 Git-Bash 为集成终端，并启用 Tab 补全。

## 适配其他 STM32 系列

1. 在 `c_cpp_properties.json` 中修改 `defines`（如将 `STM32F103xE` 改为 `STM32F407xx`）
2. 在 `launch.json` 中更新 OpenOCD 配置文件以匹配目标芯片（如 `stm32f4x.cfg`）
3. 在 `tasks.json` 中更新 OpenOCD 脚本路径以匹配你的板级配置

## 许可证

基于 [MIT 许可证](LICENSE) 授权。
