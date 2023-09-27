# Hakintoshi-ROG B460i-10400-RX6600XT
這是一個使用華碩 ROG STRIX B460i 主機板，十代酷睿帶核顯CPU，以及訊景 RX 6600XT 海外版顯示卡的黑蘋果配置文件。如果您也使用 ROG B460i 主板和 AMD 免驅動顯卡，那麼該方案可直接套用（非核顯 CPU 小做修改即可）。
### ***Opencore版本为0.9.6，MacOS版本為13.6***

## ⌛️ 更新
* 2022/07/21 MacOS 版本從12.4更新至12.5
* 2022/08/18 MacOS 版本從12.5更新至12.5.1
* 2022/09/13 MacOS 版本從12.5.1更新至12.6
* 2022/10/08 OpenCore 版本從0.8.1更新至0.8.5，同時上載新的 Config 及 Drivers 文件
* 2022/10/10 設置 Quirks>DisableRtcChecksum 為 Ture，解決開機出現 Safe mode 的錯誤
* 2022/10/26 MacOS 版本從12.6更新至13, 更新 kext 文件和 Config 文件
* 2023/01/09 MacOS 版本從13.0更新至13.1
* 2023/02/11 MacOS 版本從13.1更新至13.2
* 2023/02/14 MacOS 版本從13.2更新至13.2.1
* 2023/03/18 OpenCore 版本從0.8.5更新至0.9.0，同時上載新的 Config 及配置文件
* 2023/03/31 MacOS 版本從13.2.1更新至13.3
* 2023/04/10 MacOS 版本從13.3更新至13.3.1
* 2023/05/24 MacOS 版本從13.3.1更新至13.4
* 2023/07/28 MacOS 版本從13.4更新至13.5
* 2023/08/18 MacOS 版本從13.5更新至13.5.1
* 2023/09/27 MacOS 版本從13.5.1更新至13.6, 同時升級Opencore為0.9.6，上載新的配置文件。目前Sonoma還有一些小問題，等網路各種資源完善後再準備升級新系統。

## 🖥 Hardware

*  Case: [ZS A4 V3.1](https://zscases.com/products/zs-a4-v3-2)
*  Motherboard: [ASUS ROG B460i](https://rog.asus.com/motherboards/rog-strix/rog-strix-b460-i-gaming-model/)
*  CPU: [Intel i5 10400](https://www.intel.com/content/www/us/en/products/sku/199271/intel-core-i510400-processor-12m-cache-up-to-4-30-ghz/specifications.html)
*  RAM: [Crucial Ballistix 32G（16x2）3200 c9bjz](https://www.crucial.com/memory/ddr4/bl2k16g32c16u4b)
*  GPU: [XFX Speedster RX 6600XT MERC OC](https://www.xfxforce.com/shop/xfx-speedster-merc-308-amd-radeon-tm-rx-6600-xt-black)
*  Cooler: [IDCOOLING IS47K](http://www.idcooling.com/Product/detail/id/205/name/IS-47K) with [Noctua NF A9](https://noctua.at/en/nf-a9-pwm)
*  Storage: [WD SN750 500G M.2](https://shop.westerndigital.com/products/internal-drives/wd-black-sn750-nvme-ssd#WDS500G3X0C) / [WD SN550 500G M.2](https://shop.westerndigital.com/products/internal-drives/wd-blue-sn550-nvme-ssd#WDS500G2B0C) / [SAMSUNG 870EVO 250G SATA](https://www.samsung.com/us/computing/memory-storage/solid-state-drives/870-evo-sata-2-5-ssd-250gb-mz-77e250b-am/)
* PSU: [Seasonic FOCUS SGX 500W](https://www.amazon.com/Seasonic-SGX-500-Full-Modular-Warranty-SSR-500SGX/dp/B07WVWNZQ3)
* WiFi module: [BCM94352Z](https://www.amazon.com/BCM94352Z/s?k=BCM94352Z)

## ⚙️ BIOS 設置（針對Comet Lake十代酷睿桌面處理器）
### Disable
* Fast Boot
* Secure Boot
* CSM
* Intel SGX


### Enable
* VT-d（需 DisableloMapper 設置為 Ture)
* Above 4G decoding
* Hyper-Threading
* EHCI/XHCI Hand-off
* OS type : Other（如果 Other 選項會導致 CSM 開啟，則選擇 Windows UEFI Mode)
* DVMT Pre-Allocated : 64MB 及以上


## 🗂 EFI Details

### UEFI Drivers
* HfsPlus.efi (用來讀取顯示 HFS 磁盤)
* OpenCanopy.efi (開機引導圖形界面)
* OpenRuntime.efi (修正boot.efi，修復NVRAM和優化記憶體管理)
* ResetNvramEntry.efi（在啟動時可 ResetNvram，Opencore 8.1版本後改為添加才能使用的功能。非必要，但是建議添加）
* ToggleSipEntry.efi（在啟動時可快速開啟/關閉SIP。也是Opencore 8.1版本後改為添加才能使用的功能。非必要，根據需求選擇是否添加）

### SSDTs

* SSDT-AWAC（修復b460系主機板的系統時鐘）
* SSDT-EC-USBX-DESKTOP（修正嵌入式控制器相關問題）
* SSDT-PLUG-DRTNIA（電源管理）
* SSDT-RHUB（用於400系主機板禁用RHUB設備，重建USB端口）

### Kexts
* Lilu.kext
* AirportBrcmFixup.kext 
* AppleALC.kext
* CtlnaAHCIPort.kext
* IntelMausi.kext
* SMCProcessor.kext
* SMCSuperlO.kext
* USBToolBox.kext（如果使用別的方式訂製USB，則不需要添加）
* UTBMap.kext
* VirtualSMC.kext
* WhateverGreen.kext
* XHCI-unsupported.kext
* RadeonSensor （用於調取AMD顯示卡的溫度數值，需要Lilu支持。非必要，根據需求選擇是否添加）
* SMCRadeonGPU （可以將AMD顯示卡溫度數據提供給iStat Menus等App內顯示，需要VirtualSMC支持。非必要，根據需求選擇是否添加）

### 一些細節

1. ***你需要在OC目錄下添加Resources文件夾以獲得完整EFI文件。 Resources文件夾可以在下載的Opencore文件中找到。***
2. 為了驅動iGPU作為輔助加速解碼，在DeviceProperties>PciRoot(0x0)/Pci(0x2,0x0)中添加：
``AAPL,ig-platform-id = 0300C89B`` 
``enable-hdmi20 = 01000000`` 

3. Audio部分經過多次測試，最終確定了聲卡alcid為11。在NVRAM>7C436110-AB2A-4BBB-A880-FE41995C9F82中設置：
``boot-args = keepsyms=1 debug=0x100 alcid=11 agdpmod=pikera brcmfx-driver=2``

4. 之前遇到進入Recovery模式灰屏，只有滑鼠能動的情況。解決方法為在NVRAM>7C436110-AB2A-4BBB-A880-FE41995C9F82中修改語言：
``prev-lang:kbd = 7A682D48 616E743A 2D313638 3939``

5. Quirks中的DisableRtcChecksum設置為Ture，可解決開機出現進入safe mode的錯誤畫面。

6. 從低版本OC升級至0.88或更高版本後，如果啟動項選擇介面出現 OC EFI 的圖標且你想要隱藏它，則需要拷貝BOOT文件夾中的隱藏文件.contentVisibility到你的BOOT文件夾中。



## ✏️ 最終成果
- dGPU，iGPU，聲卡，Wi-Fi，藍芽等所有硬件正常運作；
- USB2.0及3.0速度正常；
- AirDrop，「併行」，硬件解碼功能正常；
- 正常睡眠，正常喚醒。無自動重啟，凍屏等問題。
- 總結：完美。

## 📎 截圖

![Hacktool](https://i.imgur.com/eq3PqKL.png)

![PCI](https://i.imgur.com/mQGsyZa.png)

![USB](https://i.imgur.com/BQdJiKr.png)

![顯示卡](https://i.imgur.com/QgHJRJ4.png)

![硬件解碼](https://i.imgur.com/gazHYfy.png)

![主機1](https://i.imgur.com/0ZmoEhZ.jpg)

![主機2](https://i.imgur.com/Ar27sCH.jpg)
