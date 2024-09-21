## 修正 OC 导引界面缩放

#### 修改

`UEFI -> Output -> UIScale = -2`

改为：

`UEFI -> Output -> UIScale = 2`

TIP: 修改后可能需要 重置 NVRAM 也能生效。

#### UIScale 说明

UIScale 开机用户界面缩放参数，对应4D1EDE05-38C7-4A6A-9CC6-4BCCA8B38C14:UIScale变量

`-1` : 保持当前变量不变。

`0` : 自动选择基于当前分辨率的缩放。

`1` : 1x缩放，对应正常显示。

`2` : 2x缩放，对应HiDPI显示。

注1:自动比例因子检测工作在总像素面积的基础上，可能在小的HiDPI显示上失败，在这种情况下，可以使用NVRAM段手动管理值。注2:当从手动指定的NVRAM变量切换到这个首选项时，可能需要NVRAM复位

4k显示器（默认开启hidpi），UIScale = 1 或空，表现为logo由小变大。 修改UIScale = 2，保存重启，即可大小一致。  
1080p显示器（默认未开启hidpi），UIScale = 2，表现为logo由大变小。 修改UIScale = 1，保存重启，即可大小一致

## 移除跑码信息显示

移除 NVRAM - 7C436110-AB2A-4BBB-A880-FE41995C9F82 - boot-args 参数:

原：`-v alcid=3 keepsyms=1`

新：`alcid=3`

`-v` 启动系统时跑码

`keepsyms=1` 跑码时显示更详细的信息

## 移除 XHCI-unsupported.kext

可能并不需要它，需要测试 usb 是否工作正常。

---

## 添加主题

**启用主题：**

1. 添加 `EFI/OC/Drivers/OpenCanopy.efi` 版本 v0.9.9 主题驱动文件

2. 设置选择启动管理器的界面`PickerMode = External`
   
   - `Builtin` — 使用由 OpenCore 处理的启动管理器，简单的文本用户界面。
   
   - `External` — 如果可用，则使用外部启动管理器协议，否则使用 `Builtin` 模式。
   
   - `Apple` — 如果可用，则使用 Apple 启动管理器，否则使用 `Builtin` 模式。

3. 选择启动管理器所使用的图标集 `PickerVariant` = `Acidanthera\GoldenGate`
   图标集是一个相对于 `Resources/Image` 的目录路径，其中有图标和一个可选的清单。我们建议使用 `Vendor/Set` 格式的图标集，例如 `Acidanthera\GoldenGate`。
   
   作为 [OcBinaryData](https://github.com/acidanthera/OcBinaryData) 资源库的一部分提供的样本资源提供了以下图标集：
   
   - Acidanthera\GoldenGate - macOS 11风格的图标集。
   - Acidanthera\Syrah - macOS 10.10风格的图标集。
   - Acidanthera\Chardonnay - macOS 10.4风格的图标集。 

4. 如果使用自定义主题，需要将自定义主题图片集放到 Resources/Image 目录下，再设置 PickerVariant 为该主题的文件目录名称。例如：
   
   - 在 Resources/Image 目录下添加 FairyPlus/NeonlampBusinessTheme 目录
   
   - 设置 PickerVariant = FairyPlus/NeonlampBusinessTheme
   
   对于目录路径符`/` `\`正反斜扛都可以。

**修改了以下项：**

- 替换 `EFI/OC/OpenCore.efi`  版本 v0.9.9

- 添加`仙女plus- OC霓虹简约商务主题4k稳定版-v2.0`
  添加了目录 `EFI/OC/Resources/Image/FairyPlus`

- 隐藏开机引导项菜单中的辅助条目：`Misc - Boot - HideAuxiliary = true`
  如果需要用到辅助条目，可以在引导界面按 `空格键` 显示被隐藏的选项。
  对于在 Misc - Tools 中添加的项，只有在将该项的 Auxiliary 设置为 true ，并且 HideAuxiliary = true 时才会真正隐藏。
