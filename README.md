# Minimil - Custom Zephyr Board Support Package

A custom board support package built on top of the [Alif Semiconductor Zephyr SDK](https://github.com/alifsemi/sdk-alif), providing board definitions for custom hardware variants based on Alif Balletto B1 SoCs.

## Overview

This repository contains custom board definitions and samples for:
- **Legacy Board**: `minimil_b1_dk` - Original minimil development kit
- **Current Board**: `term` (Tera vendor) - Updated board definition with cleaner naming

Both boards are based on Alif Balletto B1 SoCs with support for multiple package variants (CSP and BGA).

## Prerequisites

- **Windows 10/11**
- **Python 3.12+**
- **Git**
- **Administrative privileges** (for initial setup only)

## One-Time System Setup

### 1. Install Required Tools

Open **Command Prompt as Administrator** and run:

```cmd
winget install Kitware.CMake Ninja-build.Ninja oss-winget.gperf Python.Python.3.12 Git.Git oss-winget.dtc wget 7zip.7zip
```

### 2. Download and Install Zephyr SDK 0.17.0

```powershell
# Navigate to home directory
cd $Env:HOMEPATH

# Download Zephyr SDK
wget https://github.com/zephyrproject-rtos/sdk-ng/releases/download/v0.17.0/zephyr-sdk-0.17.0_windows-x86_64.7z

# Extract SDK
7z x zephyr-sdk-0.17.0_windows-x86_64.7z

# Run setup
cd zephyr-sdk-0.17.0
setup.cmd
```

### 3. Create Python Virtual Environment

```powershell
# Create virtual environment
cd $Env:HOMEPATH
python -m venv zephyrproject\.venv

# Set execution policy (if needed)
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser

# Activate virtual environment
C:\Users\<username>\zephyrproject\.venv\Scripts\Activate.ps1

# Install West and dependencies
pip install west pyelftools
```

## Project Setup

### 1. Activate Environment

**Always start with this step for each session:**

```powershell
# Activate Python virtual environment
C:\Users\<username>\zephyrproject\.venv\Scripts\Activate.ps1
```

### 2. Initialize Minimil Workspace

**For new workspace setup:**

```bash
# Create workspace directory (at your preferred location)
mkdir minimil-workspace
cd minimil-workspace

# Initialize with Minimil repository
west init -m https://github.com/Ankit792/minimil.git --mr main
west update

# Install Python dependencies
python -m pip install @((west packages pip) -split ' ')
```

**Alternative: Clone specific revision:**
```bash
west init -m https://github.com/Ankit792/minimil.git --mr ${revision}
```

### 3. Verify Setup

**Test with Alif reference board:**
```bash
west build -p always -b alif_b1_dk/ab1c1f4m51820hh0/rtss_he alif/samples/hello_world
```


## Quick Start Guide

### Daily Development Workflow

**1. Activate Environment:**
```powershell
C:\Users\<username>\zephyrproject\.venv\Scripts\Activate.ps1
```

**2. Navigate to Workspace:**
```bash
cd /path/to/minimil-workspace
```

**3. Build Applications:**

**For Term board (recommended):**
```bash
# CSP package variant
west build -p always -b term/ab1c1f4m51820hh0/rtss_he minimil/samples/lc3/lc3_codec

# BGA package variant  
west build -p always -b term/ab1c1f4m51820ph0/rtss_he minimil/samples/lc3/lc3_codec
```

**For Legacy minimil_b1_dk board:**
```bash
# CSP package variant
west build -p always -b minimil_b1_dk/ab1c1f4m51820hh0/rtss_he minimil/samples/lc3/lc3_codec

# BGA package variant
west build -p always -b minimil_b1_dk/ab1c1f4m51820ph0/rtss_he minimil/samples/lc3/lc3_codec
```

## Supported Boards

### Current: Tera Term Board (`term`)

**Vendor:** `tera`  
**Board:** `term`  
**Location:** `minimil/boards/tera/term/`

**Supported Variants:**
- `term/ab1c1f4m51820hh0/rtss_he` - CSP package, RTSS-HE core
- `term/ab1c1f4m51820ph0/rtss_he` - BGA package, RTSS-HE core (with MPU)

**Features:**
- ARM Cortex-M55 @ 160MHz
- 1.5MB RAM, 1.8MB Flash
- IEEE 802.15.4 wireless
- Bluetooth LE support
- GPIO, UART, I2C, SPI interfaces
- Hardware FPU and MPU (BGA variant)

### Legacy: Minimil B1 DK Board (`minimil_b1_dk`)

**Vendor:** `minimil`  
**Board:** `minimil_b1_dk`  
**Location:** `minimil/boards/minimil/minimil_b1_dk/`

**Supported Variants:**
- `minimil_b1_dk/ab1c1f4m51820hh0/rtss_he` - CSP package, RTSS-HE core
- `minimil_b1_dk/ab1c1f4m51820ph0/rtss_he` - BGA package, RTSS-HE core

## Available Samples

### LC3 Audio Codec
**Location:** `minimil/samples/lc3/lc3_codec/`

Low Complexity Communication Codec implementation for audio processing applications.

**Build Examples:**
```bash
# Term board
west build -b term/ab1c1f4m51820hh0/rtss_he minimil/samples/lc3/lc3_codec

# Legacy board
west build -b minimil_b1_dk/ab1c1f4m51820hh0/rtss_he minimil/samples/lc3/lc3_codec
```

## Useful Commands

### Board Discovery
```bash
# List all available boards
west boards

# Find minimil boards
west boards | findstr minimil

# Find term boards  
west boards | findstr term

# Find all Alif boards
west boards | findstr alif

# Get specific board info
west boards --board minimil_b1_dk
west boards --board term
```

### Project Management
```bash
# List all repositories in workspace
west list

# Update all repositories
west update

# Check repository status
west status
```

### Build Management
```bash
# Clean build (recommended for board changes)
west build -p always -b <board_identifier> <application_path>

# Incremental build
west build -b <board_identifier> <application_path>

# Build with specific configuration
west build -b <board_identifier> <application_path> -- -DCONFIG_OPTION=y
```

## Hardware Specifications

### SoC Variants

| Part Number | Package | Features |
|-------------|---------|----------|
| AB1C1F4M51820HH0 | CSP | Standard feature set |
| AB1C1F4M51820PH0 | BGA | Enhanced with ARM MPU |

### Core Specifications
- **CPU:** ARM Cortex-M55 @ 160MHz
- **FPU:** Hardware floating-point unit
- **Memory:** 1.5MB RAM, 1.8MB Flash
- **Connectivity:** IEEE 802.15.4, Bluetooth LE
- **Peripherals:** UART, I2C, SPI, GPIO, ADC, DAC

## Development Workflow

### 1. Making Changes
```bash
# Activate environment
C:\Users\<username>\zephyrproject\.venv\Scripts\Activate.ps1

# Navigate to workspace
cd /path/to/minimil-workspace

# Make your changes to board definitions or samples
# ...

# Test build
west build -p always -b term/ab1c1f4m51820hh0/rtss_he minimil/samples/lc3/lc3_codec
```

### 2. Committing Changes
```bash
# Navigate to minimil directory (manifest repository)
cd minimil

# Check status
git status

# Add changes
git add .

# Commit with descriptive message
git commit -m "Description of changes"

# Push to repository
git push
```

### 3. Updating Dependencies
```bash
# Update all repositories to latest versions
west update

# Check status of all repositories
west status

# List all repositories
west list
```

### 4. Board Definition Structure
```
minimil/boards/<vendor>/<board>/
├── board.cmake                    # Flash/debug configuration
├── board.yml                      # Board metadata and SoC variants
├── Kconfig.<board>                 # Board-specific Kconfig options
├── Kconfig.defconfig               # Default configurations
├── <board>_<soc>_<core>.yaml      # Board variant metadata
├── <board>_<soc>_<core>.dts       # Device tree source
└── <board>_<soc>_<core>_defconfig # Default configuration
```

**Example for Term board:**
```
minimil/boards/tera/term/
├── board.cmake
├── board.yml                      # Defines both HH0 and PH0 SoCs
├── Kconfig.term
├── Kconfig.defconfig
├── term_ab1c1f4m51820hh0_rtss_he.yaml
├── term_ab1c1f4m51820hh0_rtss_he.dts
├── term_ab1c1f4m51820hh0_rtss_he_defconfig
├── term_ab1c1f4m51820ph0_rtss_he.yaml
├── term_ab1c1f4m51820ph0_rtss_he.dts
└── term_ab1c1f4m51820ph0_rtss_he_defconfig
```

### 5. Adding New Board Variants

**To add a new SoC variant:**

1. **Update board.yml:**
```yaml
board:
  name: term
  vendor: tera
  socs:
  - name: ab1c1f4m51820hh0
  - name: ab1c1f4m51820ph0
  - name: your_new_soc        # Add here
```

2. **Create variant files:**
```bash
# Create the three required files for new variant
touch term_your_new_soc_rtss_he.yaml
touch term_your_new_soc_rtss_he.dts
touch term_your_new_soc_rtss_he_defconfig
```

3. **Test the new variant:**
```bash
west build -p always -b term/your_new_soc/rtss_he minimil/samples/lc3/lc3_codec
```

## Troubleshooting

### Common Issues

**Board not found:**
```bash
# Ensure you're in the correct workspace directory
cd /path/to/minimil-workspace

# Check if board is listed
west boards | findstr <board_name>

# Update repositories if needed
west update
```

**Build failures:**
```bash
# Clean build (recommended)
west build -p always -b <board> <app>

# Check for missing dependencies
west update

# Verify environment is activated
C:\Users\<username>\zephyrproject\.venv\Scripts\Activate.ps1
```

**Environment issues:**
```bash
# Recreate virtual environment if corrupted
cd $Env:HOMEPATH
Remove-Item -Recurse -Force zephyrproject\.venv
python -m venv zephyrproject\.venv
C:\Users\<username>\zephyrproject\.venv\Scripts\Activate.ps1
pip install west pyelftools
```

**West initialization issues:**
```bash
# Clean workspace and reinitialize
Remove-Item -Recurse -Force .west
west init -m https://github.com/Ankit792/minimil.git --mr main
west update
```

### Setup Verification

**Check Zephyr SDK:**
```bash
# Should show SDK path
echo $Env:ZEPHYR_SDK_INSTALL_DIR
```

**Check West installation:**
```bash
west --version
```

**Check board availability:**
```bash
west boards | findstr -E "(term|minimil)"
```

## Related Projects

- [Alif SDK](https://github.com/alifsemi/sdk-alif) - Base Alif Semiconductor Zephyr SDK
- [Zephyr RTOS](https://github.com/zephyrproject-rtos/zephyr) - Real-time operating system
- [West](https://docs.zephyrproject.org/latest/develop/west/index.html) - Zephyr's meta-tool

