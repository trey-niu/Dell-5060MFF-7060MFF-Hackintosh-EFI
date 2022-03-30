# Dell-5060MFF-7060MFF-Hackintosh-EFI

成功运行 `Monterey 12.3`，三卡（ Intel网卡 ） 、HDMI、睡眠、AirDrop正常，暂时完美。

[2022-03-25] 经测试，5060MFF (IPCFL-BS/EK) 和 7060MFF标压版（IPCFL-BS 65W）均可正常使用。

## -1. TL; DL 太长不看版

[安装步骤 / Install Instructions](#TLDR)

## 0. 感谢

感谢 [OpenCore Bootloader](https://github.com/acidanthera/OpenCorePkg) 。

感谢参考过的其他作者：[minhtranbaolong](https://github.com/minhtranbaolong)、[chencaidy](https://github.com/chencaidy/Hackintosh-OC-Optiplex-5060MFF)、[uouuou](https://github.com/uouuou/OpenCore_DELL_5060MFF_EFI) 。

感谢 [黑果小兵](https://blog.daliansky.net) 提供的镜像，以及[驱动核心显卡的教程](https://blog.daliansky.net/Tutorial-Using-Hackintool-to-open-the-correct-pose-of-the-8th-generation-core-display-HDMI-or-DVI-output.html) 。

感谢 [机汤TV](https://space.bilibili.com/485711932?spm_id_from=333.788.b_765f7570696e666f.1) 提供的 [禁用 `CFG Lock` 和修改 `DVMT`的教程](https://www.bilibili.com/video/BV1WT4y1M79x?from=search&seid=419404128670007921&spm_id_from=333.337.0.0) 。

感谢 [悄然王者](https://space.bilibili.com/366117514?spm_id_from=333.788.b_765f7570696e666f.1) 提供的[使用Loopback调整HDMI音量的教程](https://www.bilibili.com/video/BV1Tt4y1673c?from=search&seid=11648939290829135198&spm_id_from=333.337.0.0) 。

[2021-12-15] 感谢[华米OV的教程](https://zhuanlan.zhihu.com/p/404324240)，顺利驱动 Wi-Fi 和蓝牙。

[2021-12-15] 感谢 [IntelBluetoothFirmware](https://github.com/OpenIntelWireless/IntelBluetoothFirmware) 项目的蓝牙驱动。

感谢使用最多的三板斧：[OpenCore Auxiliary Tools](https://github.com/ic005k/QtOpenCoreConfig)、OpenCore Configurator、[Hackintool](https://github.com/headkaze/Hackintool)。

## 1. 配置：

- CPU: i5 8600 (6C6T)
- GPU: Intel UHD 630 2048Mb
- RAM: SAMSUNG DDR4 3200MHz 16Gb*2
- Hard Drive: KIOXIA RD20 500Gib
- Network: Intel Dual Band Wireless-AC 7260

## 2. 已解决问题:
- &check; 睡眠唤醒正常
- &check; HDMI 输出正常
- &check; HDMI 输出音量 (白苹果原生问题，使用 [Loopback](http://www.pc6.com/mac/225861.html) 暂时解决 )
- &check; Wi-Fi (~~使用 [itlwm.kext](https://github.com/OpenIntelWireless/itlwm) 和 [Heliport](https://github.com/OpenIntelWireless/HeliPort)~~ [2021-12-15] 将 `itlwm` 替换为 [Airportitlwm](https://github.com/OpenIntelWireless/itlwm) ，这样就不必使用 `Heliport` 来管理了 )
- &check; 风扇转速监控 (可使用 [iStat Menus](http://www.pc6.com/mac/111587.html) 等软件监控 )
- &check; Bluetooth 与隔空投送

## 3. 已知问题:
- &cross; ~~BlueTooth (无法正常使用)~~ ( [2021-12-15] 参考 [华米OV的教程](https://zhuanlan.zhihu.com/p/404324240) 已解决 )
- &cross; （推测是机器平台问题）视频输出需使用 DP 转 HDMI 主动式转换器（可以随 HDMI 输出音频，使用 [Loopback](http://www.pc6.com/mac/225861.html) 管理音量），或 DP 转 VGA 转换器，或使用戴尔 HDMI 扩展卡。在 Windows 下无上述问题。
- &cross; 音频输出问题，使用单线 3.5mm 耳机输出音频会出现失真，见 [Issue 1](https://github.com/ShuyNiu/Dell-5060MFF-7060MFF-Hackintosh-EFI/issues/1)。

## 4. 关键设置(已集成到EFI中，个人备忘)：
- 驱动 HDMI 输出：参考 [黑果小兵的驱动8代核显教程](https://blog.daliansky.net/Tutorial-Using-Hackintool-to-open-the-correct-pose-of-the-8th-generation-core-display-HDMI-or-DVI-output.html) ，修改平台ID、总线ID和接口类型，生成补丁后只把显卡的部分 `<key>PciRoot(0x0)/Pci(0x2,0x0)</key>` 填入 `config.plist` 。
- ~~驱动无线网卡：安装 `itlwm.kext` 到 `EFI/OC/Kexts`，再安装 `Heliport` 管理。~~
- 修复睡眠问题：使用`Hackintool`，调整 `hibernatemode = 0` 和 `proximitywake = 0`。

## 5. EFI 概要

- OpenCore 0.7.8

### Kexts：
- Lilu ~~1.5.7~~ 1.6.0
- VirtualSMC ~~1.2.7~~ 1.2.8
- Airportitlwm 2.0.0
- IntelBluetoothFirmware 2.0.1
- BlueToolFixup 2.6.1
- IntelMausi 1.0.7
- NVMeFix 1.0.9
- USBPorts 1.0
- VerbStub 1.0.4
- WhateverGreen ~~1.5.5~~ 1.5.7
- AppleALC ~~1.6.6~~ 1.6.9
- SMCDellSensors 1.2.7
- SMCProcessor 1.2.7

建议使用 [OpenCore Auxiliary Tools](https://github.com/ic005k/QtOpenCoreConfig) 管理 OC 和 kexts 的版本，使用 OpenCore Configurator 调整 kext 加载顺序。

按照上述列表顺序加载即可正常使用。

仿冒：
- 平台ID：0x3E9B0007
- 声卡ID：42000000 (DATA类型)


## <span id="TLDR"> 6. 安装步骤：</span>
### 6.1 调整 BIOS 设置
- `General` - `Advanced Boot Options` 取消勾选 `Enable Legacy Option ROMs`
- `System Configuration` - `SATA Operation` 选择 `AHCI`
- （部分BIOS无此选项）`System Configuration` - `Serial Port` 选择 `Disabled` 
- `Video` - `Primary Display` 选择 `Intel HD Graphics`
- `Secure Boot` - `Secure Boot Enable` 取消勾选 `Secure Boot Enable`
- `Intel Software Guard Extensions` - `Intel SGX Enable` 选择 `Disabled`
- `Virtualization Support` - `Virtualization` 勾选 `Enable Intel Virtualization Technology`
- `Virtualization Support` - `VT for Direct I/O` 取消勾选 `Enable VT for Direct I/O`
### 6.2 使用 `Grub EFI` 启动，禁用 `CFG Lock` ，修改 `DVMT` 为 64MB。参考 [机汤TV](https://space.bilibili.com/485711932?spm_id_from=333.788.b_765f7570696e666f.1) 提供的 [禁用 `CFG Lock` 和修改 `DVMT`的教程](https://www.bilibili.com/video/BV1WT4y1M79x?from=search&seid=419404128670007921&spm_id_from=333.337.0.0) 时间轴 09:52处。
```
setup var 0x5BE 0x0
setup_var 0x8DC 0x2
```
### 6.3 下载 macOS 镜像，写盘，复制 `OpenCore EFI` 文件夹至U盘的 EFI 分区，将文件夹重命名为 "EFI"。
### 6.4 启动系统，选择 `Install macOS`。
### 6.5 复制 `OpenCore EFI` 到本机硬盘的 `ESP` 分区，将文件夹重命名为 "EFI"。
### 6.6 （可选）安装 [Loopback](http://www.pc6.com/mac/225861.html)，管理 HDMI 输出的音量 (默认是最大音量，请保护耳朵)。（[2022-03-25] 在 `Monterey 12.2` 以后 Loopback 需使用 2.8 版本或以上。）

## 7. 黑苹果不易，且行且珍惜。
或许哪天苹果不提供对 `Intel` CPU 的支持了，黑苹果就🈚️了。
