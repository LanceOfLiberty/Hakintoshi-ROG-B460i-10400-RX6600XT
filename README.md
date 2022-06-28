# Hakintoshi-ROG B460i-10400-RX6600XT
This repository is about Hackintosh based on the ASUS ROG STRIX B460i motherboard and XFX Speedster RX 6600XT MERC OC GPU.  
這是一個使用華碩 ROG STRIX B460i 主機板和訊景 RX 6600XT 海外版顯示卡的黑蘋果配置文件。

### ***Opencore版本为8.1，其他AMD免驅動（韌體）顯卡同樣適用。***

## Hardware

*  Case: [ZS A4 V3.1](https://zscases.com/products/zs-a4-v3-2)
*  Motherboard: [ASUS ROG B460i](https://rog.asus.com/motherboards/rog-strix/rog-strix-b460-i-gaming-model/)
*  CPU: [Intel i5 10400](https://www.intel.com/content/www/us/en/products/sku/199271/intel-core-i510400-processor-12m-cache-up-to-4-30-ghz/specifications.html)
*  RAM: [Crucial Ballistix 32G（16x2）3200 c9bjz](https://www.crucial.com/memory/ddr4/bl2k16g32c16u4b)
*  GPU: [XFX Speedster RX 6600XT MERC OC](https://www.xfxforce.com/shop/xfx-speedster-merc-308-amd-radeon-tm-rx-6600-xt-black)
*  Cooler: [IDCOOLING IS47K](http://www.idcooling.com/Product/detail/id/205/name/IS-47K) with [Noctua NF A9](https://noctua.at/en/nf-a9-pwm)
*  Storage: [WD SN750 500G M.2](https://shop.westerndigital.com/products/internal-drives/wd-black-sn750-nvme-ssd#WDS500G3X0C) / [WD SN550 500G M.2](https://shop.westerndigital.com/products/internal-drives/wd-blue-sn550-nvme-ssd#WDS500G2B0C) / [SAMSUNG 870EVO 250G SATA](https://www.samsung.com/us/computing/memory-storage/solid-state-drives/870-evo-sata-2-5-ssd-250gb-mz-77e250b-am/)
* PSU: [Seasonic FOCUS SGX 500W](https://www.amazon.com/Seasonic-SGX-500-Full-Modular-Warranty-SSR-500SGX/dp/B07WVWNZQ3)
* WiFi module: [BCM94352Z](https://www.amazon.com/BCM94352Z/s?k=BCM94352Z)

## EFI Details

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

### OC配置中一些細節

1. 你需要在OC目錄下添加Resources文件夾以獲得完整EFI文件。 Resources文件夾可以在下載的Opencore文件中找到。
2. 為了驅動iGPU作為輔助加速解碼，在DeviceProperties>PciRoot(0x0)/Pci(0x2,0x0)中添加：
- AAPL,ig-platform-id = 0300C89B 
- enable-hdmi20 = 01000000 

3. Audio部分經過多次測試，最終確定了聲卡alcid為11。在NVRAM>7C436110-AB2A-4BBB-A880-FE41995C9F82中設置：
- boot-args = keepsyms=1 debug=0x100 alcid=11 agdpmod=pikera brcmfx-driver=2

4. 之前遇到進入Recovery模式灰屏，只有滑鼠能動的情況。解決方法為在NVRAM>7C436110-AB2A-4BBB-A880-FE41995C9F82中修改語言：
- prev-lang:kbd = 7A682D48 616E743A 2D313638 3939



## 最終成果
完美黑蘋果，dGPU，iGPU，聲卡，Wi-Fi，藍芽等所有硬件正常運作；
AirDrop，「併行」，硬件解碼功能正常；
正常睡眠，正常喚醒。無自動重啟，凍屏等問題。

