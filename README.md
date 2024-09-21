# Hackintosh EFI B460M AORUS ELITE macOS Sequoia

| 名称  | 型号版本                                     |
| --- | ---------------------------------------- |
| 主板  | Gigabyte B460M AORUS ELITE               |
| CPU | Intel i5 10400                           |
| 核显  | Intel UHD Graphics 630                   |
| 独显  | RX570 4G OC 、RX560 4G OC、RX580(2304 满血版) |
| 声卡  | ALC1200                                  |
| 网卡  | i219-v、~~BCM943224~~                     |
| SSD | WD Blue SN550 500G M.2接口                 |
| 系统  | macOS Ventura、Sequoia                    |
| 引导  | OpenCore 1.0.1                           |
| 机型  | iMac20,1                                 |

## 驱动情况

- UHD630 核显使用 `WhateverGreen.kext` 驱动，用于视频解码硬件加速。

- 声卡使用 `AppleALC.kext` 输出、输入正常，注入 `alcid=3` (其他尝试可用：1, 2, 3, 4, 5, 7, 11, 27, 28, 29)

- 网卡使用 `IntelMausi.kext` 正常

- `USB` 使用 `USBToolBox.kext`、`UTBMap.kext` 定制生成的 `USBPorts.kext` 正常识别 `USB3.0`,`USB2.0`

- 休眠正常

- RX560、RX570、RX580 独立显卡免驱。使用独立显卡接口，已测试 DP、HDMI、DVI 接口正常。

----

## 安装前准备事项

## 主板 BIOS 设置

### `Gigabyte B460M AORUS ELITE`

加粗项为必须设置的。

- `Tweaker`
  
  - **VT-d = Disabled**

- `Settings`
  
  - Platform Power
  
  - IO Ports
    
    - **Initial Display Output = PCIe 1 Slot** # 对应安装了显卡的插槽设置
    - Internal Graphics = Enabled # 使用集显、或硬件加速时开启(开启本项，保存退出 BIOS，重启再进入 BIOS 后才会显示下面两项)
    - DVMT Pre-Allocated = 128M
    - DVMT Total Gfx Mem = 128M
    - Aperture Size = 256M
    - **Above 4G Decoding = Enabled**
  
  - USB Configuration
    
    - **Legacy USB Support = Enabled**
    - **XHCI Hand-off = Enabled**
    - USB Mass Storage Driver Support = Enabled
  
  - SATA And RST Configuration
    
    - **SATA Controller(s) = Enabled**
    
    - **SATA Mode Selection = AHCI**
    
    - Aggressive LPM Support = Disabled

- `Boot`
  
  - **CFG Lock = Disabled**
  - **`Fast Boot` = `Disable Link`**
  - `Windows 10 Features` = `Windows 10`/`Windows 10 WHQL`(主板启动支持 4K)
  - **`CSM Support` = `Disabled`** # 某些 AMD 显卡会在开机时几秒花屏，这时你需要开启 CSM
  - Storage Boot Option Control = Legacy # 当开启 CSM 时，需要设置
  - Other PCI devices = UEFI # 当开启 CSM 时，需要设置

### 通用 BIOS 设置

- 禁用：
  
  - `Fast Boot` (快速启动)
  - `CFG Lock` (CFG 锁)
  - `VT-d`
  - `CSM`
  - `Intel SGX`

- 启用：
  
  - `VT-x`
  - `Above 4G decoding` (大于 4G 地址空间解码)
  - `Hyper Threading` (超线程)
  - `Execute Disable Bit` (执行禁止位)
  - `EHCI/XHCI Hand-off` (EHCI/XHCI 控制)
  - `OS type : Windows 8.1/10` (操作系统类型：Windows 8.1/10)
  - `Legacy RTC Device` (传统 RTC 设备)

## 重新生成一次序列 ID

序列 ID 应该重新生成一次，避免使用这里使用过的。

使用 `OpenCore Configurator 2.76.2.0` 工具，`PlatformInfo-机型平台设置` 选择一次机型 `iMac20,1`，会自动生成一系列与当前机型匹配的序列 ID。这些 ID 参数决定是否能正常使用 Apple 的服务。

----

## 安装后操作事项

## 开启硬盘的 TRIM 功能

开启 TRIM 能让 SSD 在长期使用中有更长的使用寿命及更快的速度。
使用终端命令:

**开启 TRIM**

```shell
sudo trimforce enable
```

**关闭 TRIM**

```shell
sudo trimforce disable
```

**查询 TRIM 是否开启**

```shell
system_profiler SPSerialATADataType | **grep** "TRIM Support"
```

## 深度睡眠问题

无法睡眠，睡眠后马上自动唤醒，临时解决方法使用以下命令防止进入睡眠模式：

```bash
sudo pmset -a disablesleep 1
```

当此值设为 1 时，将停用所有睡眠功能。Apple 菜单中的“睡眠”项目还会变暗（“呈灰显状态”）。设为 0 时，可恢复停用的睡眠功能。
