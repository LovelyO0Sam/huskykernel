# Pixel 8 Pro (husky) Kernel Builder

基于 KernelSU Official 的 Pixel 8 Pro (husky) GKI 内核自动构建模板。在 GitHub Actions 中手动触发，无需 ABK App。

本模板基于 [ABK (AnyBase Kernel)](https://github.com/xingguangcuican6666/ABK) 项目。

## 内核信息

- **设备**: Pixel 8 Pro (husky)
- **内核分支**: `android-gs-shusky-6.1-android16`
- **GKI 版本**: android14-6.1 (Tensor G3)
- **KernelSU 变体**: Official

## 使用方法

1. Fork 本仓库
2. 进入 Actions 页面，选择 **Pixel 8 Pro (husky) Kernel Builder**
3. 点击 "Run workflow" 手动触发
4. 配置构建参数：
   - **Sub Level**: 子版本号，X 代表 LTS 最新
   - **Security Patch Level**: 安全补丁级别，lts 代表 LTS 最新
   - **功能开关**: SUSFS / ZRAM / BBG / DDK / NTsync / Networking / KPM 等按需启用
5. 等待构建完成，下载构建产物

## 构建产物

构建完成后，可在 GitHub Actions Artifacts 中下载：

| 产物 | 说明 |
|------|------|
| **boot.img** | 可直接通过 fastboot 刷入的 boot 镜像 |
| **AnyKernel3.zip** | 兼容 AnyKernel3 的刷机包，可通过 KernelSU Manager 或第三方 Recovery 刷入 |
| **KernelSU APK** | KernelSU Manager 安装包（Official 版） |
| **SUSFS 模块** | SUSFS Magisk 模块（如启用 SUSFS） |
| **ABK Extension** | 扩展功能模块（如启用 NTsync / Networking） |

## 刷机步骤

⚠️ **只需替换 boot.img，其它分区保持原厂固件不变。**

1. 下载构建产物中的 `boot.img`
2. 将手机重启至 fastboot 模式：
   ```bash
   adb reboot bootloader
   ```
3. 刷入 boot 分区：
   ```bash
   fastboot flash boot boot.img
   ```
4. 重启手机：
   ```bash
   fastboot reboot
   ```
5. 安装 KernelSU Manager APK 完成 Root 管理

> **注意**: 首次刷入后如遇到启动问题，可刷回原厂 boot.img 恢复。本内核仅修改 boot 分区，不影响系统其它分区。

## 功能特性

- **KernelSU Official** — 内核级 Root 方案
- **SUSFS** — 内核态文件系统伪装
- **ZRAM 增强** — 支持 lzo/lz4/lz4hc/lz4k/lz4kd/deflate/842/zstd 等压缩算法
- **BBG 防砖** — 防止错误刷机导致设备变砖
- **DDK LSM** — 设备驱动套件安全模块
- **NTsync** — NT 同步驱动（提升 Wine/Proton 兼容性）
- **网络增强** — IPSet + BBR 拥塞控制
- **KPM** — 内核包管理器（需非 Official KernelSU）
- **虚拟化支持** — slot patches (off/678/123/345)
- **自定义外部模块注入** — 支持 repo / module / module_set 多种注入模式

## 项目结构

```
.
├── .github/workflows/
│   ├── pixel8pro.yml      # Pixel 8 Pro 专用构建入口（本文件）
│   ├── build.yml          # 通用内核构建流程
│   └── get-manager.yml    # KernelSU Manager APK 获取流程
├── config/                # 自定义配置（可选）
└── README.md
```

## 致谢

- [KernelSU](https://github.com/tiann/KernelSU)
- [ABK (AnyBase Kernel)](https://github.com/xingguangcuican6666/ABK)
- [SUSFS4KSU](https://gitlab.com/simonpunk/susfs4ksu)
