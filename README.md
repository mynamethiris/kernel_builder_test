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
| `clang-repo` | Clang compiler source (tar.gz or git repo + branch) | No | `https://github.com/example/prebuilts-clang.git` |
| `gcc64-repo` | 64-bit GCC toolchain | No | `https://github.com/example/aarch64-linux-android.git` |
| `gcc32-repo` | 32-bit GCC toolchain | No | `https://github.com/example/arm-linux-androideabi.git` |
| `kernel-repo` | Kernel source URL for compiling kernel | Yes | `https://github.com/example/kernel.git` |
| `anykernel3-repo` | AnyKernel3 URL for zipping kernel | Yes | `https://github.com/example/AnyKernel3.git` |
| `toolchains-path` | PATH variable for executables (colon-separated, no trailing colon) | Yes | `$GITHUB_WORKSPACE/clang/bin:$GITHUB_WORKSPACE/gcc64/bin:$GITHUB_WORKSPACE/gcc32/bin` |
| `more-flags` | Build arguments/compiler flags | Yes | `ARCH=arm64 CC=clang` |
| `defconfig` | Device-specific configuration file | Yes | `DEVICE_defconfig` |
| `ksu-setup-script` | KernelSU installation script (alternative to submodule) | No | `curl -LSs "https://https://raw.githubusercontent.com/example/setup.sh" \| bash -` |

### Setup Steps

1. **Fork this repository**
2. **Configure workflow**:
   - Navigate to *Actions → Build Kernel → Run workflow*
   - Fill required parameters:

   ```bash
   kernel-repo: https://github.com/yourname/kernel.git -b lineage-21
   defconfig: your_device_defconfig
   more-flags: ARCH=arm64 CC=clang LLVM=1
   ```

3. **Trigger build** and monitor progress
4. **Download artifact** from workflow summary post-build

## 🧠 Advanced Configuration

### Branch Management

Append `-b <branch>` to repository URLs:

```bash
clang-repo: https://github.com/example/prebuilts-clang.git -b clang-18
kernel-repo: https://github.com/example/kernel.git -b android13-qpr2
```

### Toolchain Strategies

**Option 1 - Prebuilt Bundle**:

```bash
toolchains-repo: https://github.com/example/toolchains.git
toolchains-path: $GITHUB_WORKSPACE/toolchains/bin
```

**Option 2 - Component-Based**:

```bash
clang-repo: https://github.com/example/prebuilts-clang.git
gcc64-repo: https://github.com/example/aarch64-linux-android.git
gcc32-repo: https://github.com/example/arm-linux-androideabi.git
```

## 🔒 KernelSU Integration

**Automatic Setup**:
The workflows will detects a `KernelSU` submodule.

**Manual Configuration**:

```bash
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
