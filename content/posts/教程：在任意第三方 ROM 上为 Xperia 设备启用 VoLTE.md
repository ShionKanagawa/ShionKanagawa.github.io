---
title: "教程：在任意第三方 ROM 上为 Xperia 设备启用 VoLTE"
date: 2025-02-23T08:55:00+08:00
draft: false
description: ""
---

### 前言

在 XDA 上查找 Xperia 的第三方 ROM 时，经常会看到类似的警告：

> **注意:** 请确保你能够发送和接收短信，并能够正常拨打和接听电话（包括通过 WiFi 和 LTE，如果可用）。否则，即使在 LineageOS 上也无法正常工作！此外，某些设备需要在官方系统上使用过一次 VoLTE/VoWiFi 以配置 IMS。

ROM 的开发者通常会提醒用户，在官方系统中确保 VoLTE 功能正常后再刷入第三方 ROM，否则 VoLTE 功能可能无法正常使用。

实际上，这并不是 ROM 自身的 IMS 实现存在问题，更可能是索尼 Xperia 设备的问题。在一些 Xperia 设备上，VoLTE 配置是“即插即用”的：当用户插入 SIM 卡时，系统才会下发 VoLTE 配置文件。而在某些设备上，拔出 SIM 卡会删除这些配置文件（例如，Xperia 1&5）。细心的用户可能注意到，在官方系统上更换 SIM 卡时，系统可能会提示“正在进行运营商网络优化，请重启”，这通常是因为当前 VoLTE 配置文件与插入的 SIM 卡不匹配，系统需要重新获取配置文件。

第三方 ROM 似乎不具备这种自动下发的能力。虽然 Xperia 的官方系统如何下发这些配置文件仍不明确，但我们可以通过 QPST 中的 PDC 工具手动下发，从而在第三方 ROM 上也能正常使用 VoLTE/VoWiFi。

### 步骤一：下载必要工具

1. **QPST 工具**: [下载链接](https://qpsttool.com/wp-content/uploads/QPST_2.7.496.zip)
2. **Android Platform Tools**: [下载链接](https://developer.android.com/tools/releases/platform-tools)
3. **驱动包**（主要是 90E5 驱动和 adb 驱动）: [下载链接](https://www.123pan.com/s/mHIrVv-1GrOA)  提取码：M4AA

### 步骤二：通过 root 或 adb root 开启必要端口

1. **开启 USB 调试**：请确保你的设备已开启 USB 调试。

2. **Root 用户**: 在电脑终端或 PowerShell 中输入以下命令：

   ```bash
   adb shell
   su
   setprop sys.usb.config diag,diag_mdm,qdss,qdss_mdm,serial_cdev,dpl,rmnet,adb
   ```

3. **非 Root，但使用了 Userdebug 第三方 ROM 的用户**：在开发者选项中启用“以 Root 身份调试”，然后执行以下命令：

   ```bash
   adb root
   adb shell
   setprop sys.usb.config diag,diag_mdm,qdss,qdss_mdm,serial_cdev,dpl,rmnet,adb
   ```

此时 `adb shell` 会自动退出，这是正常现象，请继续按照教程操作。

### 步骤三：安装必要驱动

参考 [这篇教程中的驱动安装部分](http://www.xperiarom.work/2023/11/10/sony-jpver-force-enable-dsds-and-5g)，为所有标有问号的设备安装合适的驱动程序。

### 步骤四：提取所需的 mbn 配置文件

在大多数 Xperia 设备上，mbn 配置文件通常位于以下路径：

```
/vendor/firmware_mnt/image/modem_pr/mcfg/configs/mcfg_sw/generic/
```

1. 使用文件管理器（例如 MT 文件管理器）将该文件夹复制出来。

2. 该目录下按地区代码组织文件夹（例如 EU 代表欧洲，SEA 代表东南亚），而地区代码目录下按运营商组织文件夹（例如 CMCC 代表中国移动）。

3. 以中国移动 (China Mobile) 为例，其配置文件路径如下：

   ```
   /vendor/firmware_mnt/image/modem_pr/mcfg/configs/mcfg_sw/generic/China/CMCC/Commercial/Volte_OpenMkt/mcfg_sw.bin
   ```

   - CT 表示中国电信（China Telecom）。
   - CU 表示中国联通（China Unicom）。

接下来，使用 PDC 工具导入所需的配置文件。如果你的设备可以在 SIM 卡插拔时正常切换配置文件，只需导入所需运营商的配置文件。例如，需要在设备上使用中国移动、NTT docomo、SK Telecom 的 SIM 卡，则只需导入这些运营商的配置文件，设备会自动切换配置文件，无需 PDC 手动干预。

若设备无法自动切换配置文件，则建议使用以下通用配置文件：

```
/vendor/firmware_mnt/image/modem_pr/mcfg/configs/mcfg_sw/generic/GLOBAL/Default_Global/VL/Global/mcfg_sw.mbn
```

### 步骤五：写入、激活并切换配置文件

1. 参考 [教程链接](http://www.xperiarom.work/2023/11/10/sony-jpver-force-enable-dsds-and-5g) 。
2. 在 PDC 工具中选择 “Load”，加载提取的 `mcfg_sw.bin` 文件。
3. 加载完成后，PDC 工具会显示一个新的配置文件选项。
4. 激活此配置文件：选中加载得到的配置文件，点击 "Active"
   - 单卡设备：激活到 sub0。
   - 双卡设备：激活到 sub0 和 sub1。
5. 重启设备

完成后，你的设备应该可以在第三方 ROM 上正常使用 VoLTE 和 VoWiFi 功能。
