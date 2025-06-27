---
title: "Vivo X9 系列解锁 Root 并开启 VoLTE 教程"
date: 2025-06-27T16:32:58+08:00
draft: false
description: ""
---

# Vivo X9 系列解锁 Root 并开启 VoLTE 教程

本教程介绍的解锁与 Root 以及开启 VoLTE 的方法**理论上**适用于  **Vivo X9 系列** 

**(包括但不限于 vivo X9/X9i/X9l/X9s/X9Plus/X9sPlus/X9Plus L等变种)** 

> 由于 **X9/X9i** 采用了与 X9 Plus/ X9s/ X9s Plus **不同的启动验证方式**，故 X9/X9i **无法**直接刷入 magisk 修补后的 boot 以**实现永久 root**，只能使用 fastboot boot 指令进行临时启动，且现阶段 vivo X9/X9i 只能在 Android 6.0/7.1 上实现临时 Root。

**刷机有风险，请谨慎操作**

## 0. 准备工作

###  硬件需求：

- 一台正常运行 Windows 操作系统并能正确安装驱动的电脑
- 一根普通的 Micro USB 数据线
- 一个可以用于短接的金属镊子（根据酷友反馈：X9 系列无法使用“ 9008 工程线”进入 9008）
- 一台 Vivo X9 系列的手机
- 不要太笨的脑袋

###  软件需求：

1. **手机驱动**
   确保手机已正确安装完整的驱动，推荐使用以下驱动包（感谢 @cramfs28 提供）。
   **下载链接**： https://www.123pan.com/s/mHIrVv-1GrOA
   **提取码**：M4AA

2. **Viqoo 工具箱**
   需要使用 @某贼 大佬的 **viqoo 工具箱** 来解锁 Bootloader。

   **下载链接（文件编码为 UTF-8）**： https://shion.lanzn.com/iHuF42ola5ha

   如果你的设备使用 GBK 编码，下载下面这个版本：

   **下载链接（文件编码为 GBK）**：[https://shion.lanzouu.com/icfyP2zp40na](https://shion.lanzouu.com/icfyP2zp40na)
   
3. **QPST**（用于开启三网 VoLTE，特别是电信 VoLTE）
   **下载链接**： https://qpsttool.com/wp-content/uploads/QPST_2.7.496.zip

4. **Android Image Kitchen Vivo**（基于 Android Image Kitchen 的用于修补糟糕的 vivo 的 Boot 镜像的自动化工具）：[https://shion.lanzouu.com/i14Oz2zotlpc](https://shion.lanzouu.com/i14Oz2zotlpc)

5. **搞机助手**（可选）
   如果对 **adb** 和 **fastboot** 不熟悉，可以下载一个搞机助手，方便刷写操作。

   **下载链接**： https://lsdy.top/gjzs

------

## 1.  升级到 Funtouch OS 4.0（Android 8.1）

Vivo 为 X9 系列提供了三个系统版本（Android 6.0 / 7.1 / 8.1），其中 8.1 为测试版，但本教程需要使用 **Riru 激活 LSPosed**，而 **LSPosed 需要 Android 8.1 及以上**，所以建议先升级到 **Android 8.1**。

**官方 Android 8.1 下载链接：** https://bbs.vivo.com.cn/newbbs/thread/6221269

⚠️ **注意**：该机型只能使用 **特殊版本的Magisk 23.0**，**不支持 Zygisk**，其它版本的 Magisk 目前无法正常使用。

### **升级步骤：**

1. **下载** 对应机型的 **全量更新包**，并将其放入手机内部存储根目录。
2. 在手机 **开机状态下**，打开官方 **文件管理器**，找到全量包并点击升级。
3. 按提示完成升级。

------

##  2.解锁 Bootloader

1. **打开 viqoo 工具箱**

2. **执行解锁步骤**

   - 进入工具箱后，选择 **1. 解锁 BL 锁**
   - 选择 **解锁方案 4**，按照提示操作
   - 手机进入 **Fastboot 模式** 后，选择对应机型（目前已知 vivo X9s Plus 可以使用 X9 Plus 的文件进行解锁，X9s 或许也可以使用 X9 Plus 的文件进行解锁，请自行测试，型号后面的 L 仅用于区分设备是否为全网通设备）
   - 可能会提示按住按键进入 **9008 模式**，但 **Android 8.1 的 Vivo X9 系列** 无法使用此方法
   - **必须拆机短接** 进入 **9008 模式**

3. **保持数据线连接，解锁 BL**

   - 进入 9008 后，按照提示完成解锁
   - **解锁过程中不要拔数据线！**
   - 解锁后，手机 **数据会被清空**，但可以正常开机

4. **修复 Windows 11 解锁时崩溃问题**

   - **部分 Windows 11（特别是 24H2 版本）** 解锁时工具箱会闪退
   - 解决方法：
     1. **打开 Windows 设置** → **系统** → **可选功能**
     2. 选择 **添加可选功能**，搜索 **WMIC** 并安装

5. **验证解锁状态**

   - 开机后，打开 **USB 调试**

   - 电脑终端输入：

     ```
     adb reboot bootloader
     ```

   - 进入 Fastboot 模式后，使用搞机助手检查 Bootloader 状态

   - 若显示 **Bootloader 已解锁**，解锁成功！ 🎉

     

## 3. Root 设备

### **注意事项**

根据 [Magisk Issue #5148](https://github.com/topjohnwu/Magisk/issues/5148) 的讨论，**Vivo 在内核中做了限制**，导致名为 **su** 的二进制文件无法执行，因此需要使用 **suu 作为替代**。

此外，由于 **Magisk 23.0 以上版本无法正常运行**，所以 **无法使用 Zygisk 及其相关模块**。

⚠️ **Vivo X9 系列使用 FDE 分区加密**，直接刷入修补后的 Boot 镜像会导致无法启动，需要修改分区表去除加密。

### **Root 步骤**

1. 使用 **Android Image Kitchen** **修改分区表**，去除加密（⚠️ 但去除加密是不安全的）。
2. 使用 **Magiskboot** 工具去除**分区挂载限制**。
3. 使用 **Magisk Manager 或 MagiskPatcher** 修补已经去除分区加密和分区挂载限制的 Boot 镜像。
4. 通过 Fastboot **刷入修补后的 Boot 镜像**。

### **关于 Root 的一些误区**

之前有传言称 **Vivo X9 系列无法永久 Root，只能通过 `fastboot boot` 进行临时 Root**，但，这是 **错误的**。
事实上，除了 Vivo X9/X9i 之外，X9 Plus/X9s/X9s Plus **可以永久 Root**，但是 **如果不去除分区加密，临时启动也无法进入系统**。

初步推测原因是 X9/X9i 与 X9 Plus/X9s/X9s Plus 对于 `androidboot.securebootkeyver`的配置不同。

### **部分已经修补好的 Boot 镜像**

如果您实在是无法理解如何修补镜像，且恰好设备已经更新到最新版本的固件，可以跳过下面的修补教程，直接刷入下面提供的 Boot 镜像。

请在确保**版本号**和**设备型号完全匹配**的情况下刷入，**设备型号**后缀的 **L** 仅用于**区分**设备**是否为全网通**，Boot 镜像**并无差异**，如 X9 Plus L 可以刷入 X9 Plus 的固件。

- X9 Plus (PD1619_A_8.12.1)：[下载链接](https://shion.lanzouu.com/isIsz2psu1he)
- X9s (PD1616B_A_8.20.30)：[下载链接](https://shion.lanzouu.com/ijU9k316tbhi)
- X9s Plus (PD1635_A_8.20.15)：[下载链接](https://shion.lanzouu.com/igMvF316uiba)

------

### 3.1 获取 Boot 镜像

我们需要使用 **修改后的 Magisk 23.0** 并且最好通过某贼工具箱来修补 Boot 镜像，**Boot 镜像必须来自与你的系统版本匹配的官方固件**。

**下载链接**： https://syxz.lanzoub.com/iB5dH0671hah

#### **提取 Boot 镜像**

1. 以 **Funtouch OS 4.0（Android 8.1）** 为例，从下载的 **全量更新包** 中提取 `boot.img`。
2. 确保提取的 Boot 镜像版本与当前系统完全匹配，否则会出现无法启动的问题。

------

### **3.2 修改 Boot 镜像以去除 FDE 分区加密以及分区挂载限制**

Vivo X9 系列采用 **FDE 分区加密**，直接刷入修补后的 Boot 镜像会导致无法启动，所以需要修改 **fstab.qcom** 文件，去除 **强制加密**，以及 Vivo 在内核中**对 /system 分区的挂载**进行了**限制**，从而使得需要挂载到 /system 分区的模块（比如说 Riru、字体模块等等）失效，所以我们需要使用 Magiskboot 工具对其进行修补，这里使用了 wuxianlin 大佬的[方案](https://github.com/wuxianlin/build_magisk_vivo/blob/master/patches/Magisk/patch_vivo_do_mount_check.diff)

#### **3.2.1 简化的修改步骤（如不生效请参考下面的手动步骤）**

1. 将提取的 `boot.img` **拖入** **Android Image Kitchen Vivo** 进行解包。
2. **执行** `magiskboot.bat` 得到 `boot_repacked.img`。

#### **3.2.2 手动的修改步骤（如简化的修改步骤有效，请直接跳过）**

1. 下载 本文提供的 **Android Image Kitchen Vivo**

2. 将 boot.img 拖入 **Android Image Kitchen Vivo** 的文件夹，如下图所示：

   ![](/img/ScreenShot_2.png)

3. 点击  **unpackimg.bat** 从而解包 **boot,img**

4. 打开 **ramdisk** 文件夹，找到该文件夹下的 **fstab.qcom**，用记事本打开

5. 删掉 `,forceencrypt=footer` ，即下图中高亮的部分，然后保存：

   ![](/img/ScreenShot_3.png)

6. 返回 **Android Image Kitchen Vivo** 文件夹点击 **repackimg.bat**

7. 得到的 **unsigned-new.img**

   > 如果你得到的是 `image-new.img`，则说明你提前用 Magisk 修补了 boot.img

8. 在 **Android Image Kitchen Vivo** 所在的路径打开 终端 或者 Powershell 或者 Cmd，执行如下内容：

   ```bash
   magiskboot.exe unpack unsigned-new.img
   magiskboot.exe hexpatch kernel     0092CFC2C9CDDDDA00 0092CFC2C9CEC0DB00
   magiskboot.exe hexpatch kernel_dtb 0092CFC2C9CDDDDA00 0092CFC2C9CEC0DB00
   magiskboot.exe repack unsigned-new.img boot_repacked.img
   ```

   > 如果你在一步得到的是 image-new.img，请自行将上面指令的 unsigned-new.img 更改为 image-new.img

9. 指令执行结束后得到的 `boot_repacked.img` 即是 去除了 **FDE 加密** 并且绕过了**分区挂载限制**的 boot 镜像

------

### **3.3 使用 Magisk Patcher 修补镜像实现 Root**

> 当然您也可以使用本文提供的 Magisk Manager 进行修补，修补选项必须两个全勾选，否则无法开机
>
> 如果你不知道如何选项修补选项，请参考下面的教程。

可参考 **酷友的方法** 进行操作：

1. **下载** @某贼 的 **viqoo 工具箱**。

2. 替换 Magisk 安装包

   （如使用本文提供的工具箱，则此步骤可跳过）：

   - 将上文提供的 **Magisk 23.0** 替换工具箱内的 `bin\MagiskPatcher\prebuilt\MagiskAlpha-25101.apk`。

3. 修补 Boot 镜像：

   - 进入 viqoo 工具箱，选择默认的 **Alpha 25.2 方式修补 boot.img**，按提示完成操作。
   - 修补后的镜像位于 viqoo 工具箱所在路径的  bin\MagiskPatcher 下，名为  `boot_repacked-MagiskAlpha25101Patched.img`

### **3.4 刷入修补后的 Boot 镜像**

使用 Fastboot 刷入 `boot_repacked-MagiskAlpha25101Patched.img`，执行以下指令：

```
fastboot flash boot boot_repacked-MagiskAlpha25101Patched.img
```

**格式化 Data 分区**（必须，否则系统可能无法正常启动）：

```
fastboot format userdata
fastboot format cache
```

⚠️ **为了防止 Vivo 在恢复模式下进行额外的限制，建议手动进入 Recovery 双清**：

```
fastboot reboot recovery
```

然后在 Recovery 模式下手动 **清除数据和缓存**，再重启系统。

------

### **3.5 验证 Root 状态**

设备重启后，Root 权限已生效，安装提供的 Magisk 可以正常获得权限，但目前 **仅限 `suu`**，不能直接使用 `su` 命令。



## 4. **Riru、Sui、LSPosed 及 核心破解问题**

Riru 请直接使用最终版：[下载链接](https://github.com/RikkaApps/Riru/releases/download/v26.1.7/riru-v26.1.7.r530.ab3086ec9f-release.zip)

在 Riru 正常工作后，直接 **安装支持 Riru 的 LSPosed 6712 版本**（[下载链接](https://github.com/LSPosed/LSPosed/releases/download/v1.8.6/LSPosed-v1.8.6-6712-riru-release.zip)） 即可，之后即可使用 LSPosed 模块。（由于我们的设备只能使用特殊版本的 Magisk 23.0，故无法使用高于 6712 版本的 LSPosed）

Sui 目前只能使用 12.0.0 版本（[下载链接](https://shion.lanzouu.com/iSL4g316osud)），详细参见该 [issue](https://github.com/RikkaApps/Sui/issues/24)，与 Shizuku App不同，无需进行修改即可正常实现 Shizuku API 的授权。

核心破解请直接使用**幸运破解器**的 **Xposed 模式**的**核心破解**吧，旧版本的 "核心破解" 模块，在我们的设备上效果不佳，使用**幸运破解器**的**核心破解**即可

------

## **5. 解决 Root 软件授权问题（黑域、冰箱等）**

 由于 **Vivo 只能使用 `suu` 而非 `su`**，导致大部分 Root 相关软件无法正常授权。
 目前，只有 **MT 管理器、爱玩机工具箱、Scene** 等专门适配的软件可以正常获取 Root 权限。

### **解决方案**

- **使用 ReplaceSu 模块**
  由 @PenicillinA 大佬开发的 **ReplaceSu（LSPosed 模块）** 可解决部分软件的授权问题（[下载链接](https://www.yhcres.top/00-%E5%B7%A5%E5%85%B7%E9%A9%B1%E5%8A%A8%E6%A8%A1%E5%9D%97/ReplaceSU) 请下载 suu.apk 并安装激活）。
- **针对黑域、Shizuku 等 ReplaceSu 可能失效的软件**
  由于 ReplaceSu 并不能兼容所有软件，比如 Shizuku，我们可以使用 MT 管理器修改 apk 内调用 su 的指令以强行兼容 suu:
  1. 使用 **MT 管理器**打开 APK 文件，点击 DEX编辑器++。
  2. 点击**搜索** 然后点击 **发起新搜索**。
  3. **查找内容**填写 **su**，**搜索类型**选择**字符串**，并确保"搜索子目录"和"完全匹配"两个选项已经勾选，然后点击**确定**
  4. 点击**在当前结果中替换**。替换内容填写为 **suu**，点击确定
  5. 按返回键，弹出提示 ”文件 classes.dex 已被修改"，选择自动签名，点击确定，安装修改后的 APK 即可。

### **特别提醒**

由于 `suu` 的特殊性，建议尽量使用 **原生支持 `suu` 的软件**（如爱玩机工具箱等），本文提到的使用 MT 管理器修改的方法仅适用于未加固的软件，因此对于存储空间隔离、黑域等需要 Root 但是又加固的软件无效。故对于存储空间隔离，请使用 Sui 进行授权，对于黑域请使用 Shizuku 进行激活。

------

## **6. 启用三网 VoLTE**

此部分教程用于解决 Vivo X9 系列**不支持电信/联通 VoLTE** 的问题，但是只使用电信卡在 X9 Plus 上测试过，请谨慎跟随教程进行操作，且该设备的基带似乎仅支持 7+5 Mode 而不支持 7+7 Mode 故**似乎无法实现跨运营商双 4G待机**。因此，**仅建议单卡使用**

操作的前提是，设备已经更新到 **Android 8.1** 并且已经**解锁 Bootloader** 获取了 Root。

需要的软件：

1. 创建快捷方式（已经修改支持 suu，直接安装即可）：[下载链接](https://shion.lanzouu.com/iZAZW2zp3qsf)
2. MT 管理器
3. Windows 上的 QPST 套件，[下载并安装](https://shion.lanzouu.com/icjk52zoyo1g)

 需要的 Magisk 模块：

- VoEnabler [下载链接](https://shion.lanzouu.com/idlxK2zp3kxe)

### **6.1 提取 NON-HLOS.bin 中的配置文件**，用作备份

1. 从下载的 **全量更新包** 中提取 `NON-HLOS.bin`。
2. 确保提取的 NON-HLOS.bin 版本与当前系统完全匹配，否则会出现无法生效的问题。
3. 用 7-Zip 等压缩软件将 NON-HLOS.bin 作为压缩包打开，提取其中的 `image\modem_pr\mcfg\configs\mcfg_sw\generic\mbn_ota\`文件夹

### **6.2 刷入 VoEnabler 并修改**

**Vivo** 在包括但不限于 **framework.jar、services.jar、TeleServices.apk** 中加入了大量的**机型判断**，其中的一些**机型判断**使得系统设置对于 Vivo X9 等一系列设备**不显示电信/联通的 VoLTE** 选项，哪怕设备在硬件上是支持的，但是我们又**不能直接修改设备型号信息**，这会导致设备**没有声音、无法打开相机、甚至无法正常启动**。

值得庆幸的是，我们可以通过**修改设备版本号**来解决这个问题。

1. 首先，我们刷入 VoEnabler 模块并重启
2. 然后打开 MT 管理器，将 /oem/oem.prop 中的内容全部复制到 /data/adb/modules/voenabler/system.prop 中 (注意，不要覆盖其中原本的内容，而是添加到原本的内容之后)
3. 将 `persist.vivo.radio.tyoe.abbr=A` 置空【即改为 `persist.vivo.radio.tyoe.abbr=` ，也就是删除等号后的 A，如果是其它内容，也请删除】
4. 同理，将 `ro.vivo.op.entry` 和 `persist.vivo.network`  置空
5. 然后将 `ro.vivo.oem.name` 改为其它机型，比如说 `PD1709`
6. 然后将 `ro.vivo.product.version` 和 `ro.build.version.bbk` 以及 `ro.build.networkaccess.version` 均改为 `PD1709_A_8.12.1`
7. 保存修改后的 system.prop，重启
8. 重启后应该可以在设置内看见 VoLTE 选项。

### **6.3 锁频段并且禁用 MBN 自动更新**

1. 安装创建快捷方式，打开
2. 点击右上角的三个点，选择 **也搜索系统应用**
3. 搜索 network，选择 NetworkState 的活动列表
4. 点击第一条 MainActivity 的详情，然后打开，便进入了 Vivo 隐藏的网络设置菜单
5. 选择锁 Rat，改为 LTE Only，返回
6. 选择锁 Band，建议只选择你的运营商支持的 4G 频段，返回
7. 选择  MBN Test，点击 MBN 自动激活开关，选择关闭，手机会重启。
8. 重启后打开 MT 管理器，打开 `/firmware/image/modem_pr/mbn_ota.txt` 删除 `mcfg_sw/generic/mbn_ota/ct/mcfg_sw.mbn` 和 `mcfg_sw/generic/mbn_ota/cu/mcfg_sw.mbn` 这两行，保存
9. 用 MT 管理器 将`/firmware/image/modem_pr/mcfg_sw/generic/mbn_ota/ct/mcfg_sw.mbn` 和 `/firmware/image/modem_pr/mcfg_sw/generic/mbn_ota/cu/mcfg_sw.mbn`替换为 `/firmware/image/modem_pr/mcfg_sw/generic/mbn_ota/cmcc/mcfg_sw.mbn`，然后重启。

### **6.4 使用 PDC 导入相应的配置**

1. 相关驱动和软件的安装请参考此[教程](http://www.xperiarom.work/2023/11/10/sony-jpver-force-enable-dsds-and-5g/)

2. 打开 Powershell 或者 cmd，依次输入如下内容并回车：

   ```cmd
   adb shell 
   suu
   setprop sys.usb.config diag,serial_smd,rmnet_ipa,adb
   ```

3. 输入完成后打开 PDC，应该可以识别到设备，如下图所示

   ![](/img/ScreenShot_1.png)

4. 先选中 OpenMkt-Commercial-CU 和 OpenMkt-Commercial-CT，鼠标右键点击 Deactive 然后分别选择 Sub0 和 Sub1，然后再次选中后点击 Remove 删除对应的配置。

5. 再选中 Volte 开头的那一行或者 ROW 开头的那一行，右键点击 Active，选中电信和联通卡所在的卡槽（卡一为 Sub0 卡二为 Sub1）。然后点击下面的 Active。使得最后效果如上图所示即可。

6. 此时应该可以正常获取到 VoLTE，你可以在拨号界面输入\*#\*#4838#\*#* 以检查是否生效。

------

## **7. 终章 & 感想**

不得不说，**Vivo 的魔改确实够多**，折腾这台手机的过程也令人头疼，但最终还是实现了 Root + VoLTE 的目标。

其实早在高中时，我就尝试过折腾这台手机，但当时 X9 Plus 仍在我父亲手里，**没有机会深入研究**。
 后来，这台手机因 **缺少电信 VoLTE 而退役**，被我接手进行研究。

所幸，**经过大学四年的积累**，对相关技术有了一定了解，加上 **各路大佬的帮助**，终于让这个“老古董”成功 Root 了。

在此，特别感谢本文提到的各位大佬，
 以及 @秋水 105 **一直以来对 Vivo 设备的深入研究**！

另外，Vivo X9 Plus 相关的修改已经上传至 GitHub：[https://github.com/ShionKanagawa/pd1619_archive](https://github.com/ShionKanagawa/pd1619_archive)



