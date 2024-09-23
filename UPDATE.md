# Hackintosh i5-10400 B460M-AORUS-ELITE RX580

## 20240923 更新

- 升级 `Lilu.kext` 到 v1.6.8，移除`-lilubetaall` 参数。
- `UIScale = -1`

## 20240921 更新

支持从 macOS Sonoma 升级到 macOS Sequoia

- 升级 WhateverGreen.kext 到 1.6.7

- 添加 `-lilubetaall` 参数，解决 DP 口启动黑屏

## 20231201 更新

2023-12-01 更新，支持 macOS Sonoma 升级，去除 BCM943224 无线网卡驱动与补丁，Apple 已从 Sonoma 系统中剔除博通无线网卡驱动。