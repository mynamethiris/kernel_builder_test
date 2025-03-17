# 🛠️ Kernel Builder with GitHub Actions

[![Build Status](https://github.com/mynamethiris/kernel_builder_test/actions/workflows/build.yaml/badge.svg)](https://github.com/mynamethiris/kernel_builder_test/actions/workflows/build.yaml)

Automated Android ARM64 kernel build pipeline featuring KernelSU integration and flexible toolchain management. Inspired by [kernel_build](https://github.com/zclkkk/kernel_build).

## ✨ Key Features

- **One-Click Automation**: Full compilation through GitHub Actions
- **Toolchain Flexibility**: Choose between prebuilt bundles or component-based setup
- **Branch Support**: Specify branches for any repository dependency
- **AnyKernel3 Ready**: Automatic packaging for recovery flashing
- **KernelSU Integration**: Automatic detection & configuration
- **Smart Versioning**: Auto-generated artifact names with kernel version

## 🚀 Quick Start

### Prerequisites

- Android ARM64 kernel source repo
- GitHub account
- Basic understanding of:
  - Kernel configuration files
  - Git repository management
  - GitHub Actions workflows

### Configuration Guide

| Parameter | Description | Required | Example |
|-|-|-|-|
| `toolchains-repo` | Prebuilt toolchain bundle (overrides individual components) | No | `https://github.com/example/toolchains.git` |
| `clang-repo` | Clang compiler source (support tar.gz or git repo) | No | `https://github.com/example/prebuilts-clang.git` |
| `gcc64-repo` | 64-bit GCC toolchain (support tar.gz or git repo) | No | `https://github.com/example/aarch64-linux-android.git` |
| `gcc32-repo` | 32-bit GCC toolchain (support tar.gz or git repo) | No | `https://github.com/example/arm-linux-androideabi.git` |
| `kernel-repo` | Kernel source URL for compiling kernel (only git repo) | Yes | `https://github.com/example/kernel.git` |
| `anykernel3-repo` | AnyKernel3 URL for zipping kernel (only git repo) | Yes | `https://github.com/example/AnyKernel3.git` |
| `toolchains-path` | PATH variable for executables (colon-separated, no trailing colon) | Yes | `$GITHUB_WORKSPACE/clang/bin:$GITHUB_WORKSPACE/gcc64/bin:$GITHUB_WORKSPACE/gcc32/bin` |
| `more-flags` | Build arguments/compiler flags | Yes | `ARCH=arm64 CC=clang CROSS_COMPILE=aarch64-linux-gnu- CROSS_COMPILE_ARM32=arm-linux-gnueabi-` |
| `defconfig` | Device-specific configuration file | Yes | `DEVICE_defconfig` |
| `ksu-setup-script` | KernelSU installation script (alternative to submodule) | No | `curl -LSs "https://https://raw.githubusercontent.com/example/setup.sh" \| bash -` |

### Setup Steps

1. **Fork this repository**
2. **Configure workflow**:
   - Navigate to *Actions → Build Kernel → Run workflow*
   - Fill in the required parameters or fill in others as needed
3. **Trigger build** and monitor progress
4. **Download artifact** from workflow summary post-build

## 🧠 Advanced Configuration

### Support for tar.gz files

```bash
# Example for Clang tar.gz:
clang-repo: https://android.googlesource.com/platform/prebuilts/clang/host/linux-x86/+archive/refs/heads/android12-release/clang-r416183b1.tar.gz

# Example for GCC64 tar.gz:
gcc64-repo: https://android.googlesource.com/platform/prebuilts/gcc/linux-x86/aarch64/aarch64-linux-android-4.9/+archive/84fb09fafc92a3d9b4d160f049d46c3c784cc941.tar.gz

# Example for GCC32 tar.gz:
gcc32-repo: https://android.googlesource.com/platform/prebuilts/gcc/linux-x86/arm/arm-linux-androideabi-4.9/+archive/5a8beef7b1aa2c8ca0dfe4a00358559d12dfa3b6.tar.gz
```

### Branch Management

```bash
# Use this format for any other repositories:
repo-name: <repository-url> -b <branch-name>

# Example for Clang repo:
clang-repo: https://gitlab.com/example/prebuilts-clang.git -b clang-18

# Example for Kernel repo:
kernel-repo: https://github.com/example/kernel.git -b lineage-21
```

### Toolchain Strategies

**Option 1 - Prebuilt Bundle**:

```bash
# Example for Toolchains repo:
toolchains-repo: https://github.com/example/toolchains.git

# Example for Toolchains path:
toolchains-path: $GITHUB_WORKSPACE/tools/clang/bin:$GITHUB_WORKSPACE/tools/gcc64/bin:$GITHUB_WORKSPACE/tools/gcc32/bin
```

**Option 2 - Component-Based**:

```bash
# Example for Clang repo:
clang-repo: https://github.com/example/prebuilts-clang.git

# Example for GCC repos:
gcc64-repo: https://github.com/example/aarch64-linux-android.git
gcc32-repo: https://github.com/example/arm-linux-androideabi.git

# Example for toolchains path with Clang only:
toolchains-path: $GITHUB_WORKSPACE/clang/bin

# Example for toolchains path with Clang and GCC:
toolchains-path: $GITHUB_WORKSPACE/clang/bin:$GITHUB_WORKSPACE/gcc64/bin:$GITHUB_WORKSPACE/gcc32/bin
```

## 🔒 KernelSU Integration

**Automatic Setup**:
The workflow will detects a `KernelSU` submodule.

**Manual Configuration**:

```bash
# Example for ksu setup script:
ksu-setup-script: curl -LSs "https://raw.githubusercontent.com/example/setup.sh" | bash -s next-susfs
```

## 🚨 Troubleshooting

- **Build Failures**: Verify toolchain/kernel version compatibility
- **Missing Branches**: Confirm repository supports specified branch
- **Path Errors**: Ensure `toolchains-path` has no trailing colon
- **KSU Issues**: Check for `CONFIG_KSU=y` in defconfig

## 📜 Important Notes

- 🔄 **Legacy Kernels**: Use older toolchain versions (pre-5.4 needs backports)
- 📦 **Artifacts**: Contains `Image.*` + `dtbo.img` (if generated)

## 🙏 Credits

- Original concept: [kernel_build](https://github.com/zclkkk/kernel_build)
- [GitHub Actions](https://docs.github.com/en/actions) infrastructure
- [tiann](https://github.com/tiann) for [KernelSU](https://kernelsu.org)
- [osm0sis](https://github.com/osm0sis) for [AnyKernel3](https://github.com/osm0sis/AnyKernel3)
