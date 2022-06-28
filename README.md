# Hakintoshi-ROG B460i-10400-RX6600XT
This repository is about Hackintosh based on the ASUS ROG STRIX B460i motherboard and XFX Speedster RX 6600XT MERC OC GPU.  
這是一個使用華碩 ROG STRIX B460i 主機板和訊景 RX 6600XT 海外版顯示卡的黑蘋果配置文件。

### ***Opencore版本为8.1，MacOS版本為12.4。其他AMD免驅動（韌體）顯卡同樣適用。***

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
完美黑蘋果，dGPU，iGPU，聲卡，Wi-Fi，藍芽等所有硬件正常運作,USB2.0及3.0速度正常；
AirDrop，「併行」，硬件解碼功能正常；
正常睡眠，正常喚醒。無自動重啟，凍屏等問題。

## 截圖
![主機概覽](https://lh3.googleusercontent.com/m9nK90gaN-F4FiXEQQIeBIMxnd9jgu0MNmyv-J15rlh3HexQWap4WvAMHLR1zh0cBxmogeCgY6H5tbYEGvGT6BWjGA7EYqbfauBZGrJyLSsnhgSIxOIXsUwDaRo5immYma2OJ2-DDLG1z5j0g62hxzia5uPYCWGrVbtdyqIpiFYxxdLD8w8sG0vilUm2i3o0IV2tvbNGeiHNRehkT4MP6yl8lGqJsHrm4AbXQAuiNt96OiIBjCwUz365Tvs8dlAkAGfjXPswsmh6nmdZl4SW5vFgnlsPYM1IMkqAIpMzOdkpUoaDAc2nzsDl70-wglI3iRkPPSgljwm3bPUjr-UhA7X2zh2gvYqXAuu_8mijIhZIfTw6keyzzISzDegveVH6YY-D_AFkMs3Rya1PxAobO87jSQH_0Jtl6uyiL7A0chSkLSqOctWBdOqhtX_ZOlkLbUIK2d_DCcneEWfOZebRnqPdkHV8cuSLkxuS15fYzurlpS1d7lcE-ybf7Gn9cXvIunQuFPPkxx4LirslXaYJVfiUZ01ma6QKDxZ_eocGViv8lRTlKWFPxxObEMb4tpQZZqgRUfGUe2di9M5WQsoGtnYs9hJ3FXaYXoo_SmSjlBK7XJz4PGyiMS8Uy39xzYa0fDL0xqbCRM3eBxArUvIhlCSEvCvh8DJfTn7cpWZkVULdBf57JTR3NWI15TjiT4Ekns3TdI_9X0jijrUA1TeX2tajqmUO3x2TvPDwO-xRZKE9mcwo12-l6fIMJwouzjIIhnWYUaMX2mb5EjT00ghIkwcTVrV1BhC8TFb4ubAigohfAAFaz3Z8FW5ZUnb9CoGsPJc=w1747-h1464-no?authuser=0)

![Hacktool](https://lh3.googleusercontent.com/dVL-gNKk6Nd0sjqPlm0EgDiDIH4XM81kTFzc9beLkBSAw4VMjlwtwJub4YKT4L6cdBcYe4XSYIcxmoZQEq_J6zd9X0t1_4MvsgwmSapyRHspGArIuUNLUC4hlJyKjoLk83Tj0ne6bPb6SmYW1nzDsqnpH2dDGqA_bhRc98zA2cBXwoVCOVm798l7hijx3heBNqj2X3I0vE2DcMbP7XagbMFYsRjCfk1EK80NJ56_8chjxnVv5YLJvQx_Yqp9yglW1XJreXBAaDlRmh1Q9_Zrp-k0Z0WpjK8eZLvhdC-g9_icNcaPJq4jDrbgdnKWL5t2QaLudtG0HR-uXvr8exnqFA9y4Id_PhhuXzQgNaS7nts2KJlcgp4olrOh_kE7j8rw-B6H0et4MRi0LHodjzLhIkZ0UyE-BEnJovpRlF3rj7QIrteFZuTtbiiwwRGolfzztBDFq4XNLyDGUq1QTXzaEFGY9M0uXp3k2JJxwBhrByeTL_LX0xcbbwBpy9_vIowOe9g1u_hf61NQRSIWMMR34WePeyOGeDvcS17jXD3wkOYstH4WLq0GmkXpvehiR7RzJuiJgpbdR_yEo_j8Y0dx6diVbc1_xaT6Md03bEe3JmykwGdTqiARxe0aANoQO3RcX2rnpSZ8Bx4GCAfjjfSIjYpcwPDBfPrQzpZrf1LSM4tYbyXYqyvJgKBCgsadeaQ2VmZIeg7jPiWGO7t49dJtPhqXjkj6VT3RKgcCGwqXhVAOu4Urg3hjF0V_fdnBAKLb6qc65qI6VSjw8p1AUEqQGDv1kQKgyw_ZpHRk5hN04k3aNp4B0v-P8f03LEbUPdDo_wU=w2846-h1694-no?authuser=0)

![PCI](https://lh3.googleusercontent.com/cypx2h2ZZQ2_L9us3GlGFRVuSRWn0Zdg6TpljvvL5r--xELXlYR1_F2V1uSPitJX_1GRXkwQ2NEBXPGV5wAEkwdznLxlBq6WZTGPRbFviOCinPT38N-eUpUahHmCQPrdlT9NfQNulOKTRrkpWRQGMsAB9R9nATLbidRWcfEEw389TNiR_u9fVUs-1jV5XHGzzSehRCFMXJS9yW8C2th_xpzX-Jk4RQ00wx8T4RGRJG4yepAeKnW-AgBgWZ_bx8AimLXR-tdOO7REXFSGX1w9fUNBgfs0bpCxPEHs3BSxdYoC1nSLTIq2T98dc0q1ppQWpM3FngZ2fh2CMg32dz0GArdJrQqVYbdV95lNCow7k3BRgU8L9Va0EB8-66yusvSjqq2Vs3UJxWZzfraStlW0S3gAeDNmkFQoXJ9VCPvy-ejm5Wk11e8hSLa289ks5DimsZMMUZ3ZBDKe_IhXUicuRn65itvTTgIT2G_nac4zlwLpm0u1e93jJhS7-G8i6swasS972Xg3IrhmaAOho1dPo1CaOySzHgjQqOWpzVkX-BqUTgqi_XseiECNr_01Ij0kSjR6DBn9ayXL9bRBGTzRBdWSQPF624yR1muqnl8tWdl4Z_PjdCkWVHddn4YMg-J0exr2Aw6oaA2GLZeTYBLKh7oZKgU-7plqq1auyJWhvec-lgcV_S4O_Q7LRpn11OixaJVBkpZxnPMGpaoxbvK-RGeoXdQ0BEpBLBA4BRftisoCqYLKovD7KExX1VONsWmX0oN1az8LhR8v0pchUAZFwfhuuvme4HmNTzYMjAP00sqFxAbc6gP4_BSPPS_hPIV1NU8=w1836-h1388-no?authuser=0)

![USB](https://lh3.googleusercontent.com/O-hmPMGyKdSqAvqIJ5Cy_A9_wwGQWiOivmhgG4gthbtCRvvAFsoewV5jnYzs_fEQah8S32K_5Q_LOMYLX3y43hBQ0Mef_FBmgSLOosmkDfBAUa4H1vulOuKEa1vq4Ty8zj4rFX_k3eJs4DlhX0MynuJypTopfVLmT3K4xPWqVIVv3a8194xmLVYyUD2Cm0JyLDC86JlI95MyGIzaQCQ0-yzxk1OutFwYQIV-nrOx2hXqyrvuzxQ9aFnJHj-ZwRPUBHeKi55gZCDKbXysaHlgt-BLZlJ6GATzNagkhld0c4IisJSk1UDuboyE-6oKVcWhi75bFjIWnhesj-uMw-9EkGLBsGNO2gl8jUAG_bHBKQhJACB5TfNYs6nCpFvVykHouj_es2s1rootmccpnoWV7i_Kf8nJ0BwiLFuRVbdtFsj21SGm63VZq9Q2VW_95xWTtpQM45yeNgCSLOajkX5QkAxMVGRJcVIqMEzqbDf-YjyRGzQmqx4-xDnJ39zdtTo5cTP9OTfwuC0aNY2QV6IqW4m86wF-DK9ox8fTTy90GRir5dOoICKQ0r7swpcve_PS1Ik9S2ErVX4Z4M9aD3kPvdS76LOpq77g2PtAXyLhl3-KppePJUjibG3D3y8xUILNUQKV6dOG8iUW2WrBdJel7W0Qv6QtuG5xv0PliyYzsQwEoOz4ePNzX2rwTC2aFMETMKSjhsnd7Z5-zpZ6TuiPpTN1GGY8LOwk1ll0YlE8OkYBgZLSC5SXu5hlLcNS94lLst3NXzFythI9fIqTU36wO-ePfPfXKFuX1jENFwKeuQio-JghRpR1nS6NSE23nIbYp0U=w1840-h1384-no?authuser=0)

![顯示卡](https://lh3.googleusercontent.com/Q-yI6qrjB3BL7GrWFnTHzcJw4o3-7V16ww-48bHiKlgWuRwhqjjYLJJQf4RVfh1v5XohL1OAZNurYwUqZGU8sQOwiD4ULgKR9C7cA_J-Ycd3K9vXhpwPCtUesA20R-c-0LgjNAiQKLuNarOwyCZUkf1BCJidOxEjKVubjbckB0K3na6art-QlsRuEZ8Yu5GV0Lcs88SjI0Ki1xuuxM3lcUKo7axRgHDcLtZKiyPHoMV9htymcZVWp1FVCl4CA12wCMtoedn9eyhBidzbO5YUS1q-jWNX0ks4r6eB5f5MGul9i4wizOZJp0CwSWmQ_C1gICGE797kN26WiBRdereYoNM0OItEM7pwbbI7xzIJpHhTqLCNbcUtHEJErcXElJz3mw-P7QhV6yphuuTkgfveGpxro7FKTYciPGWlFUrQ4k_MuI7rUzs6g_iJsNh1kpDDAATLOli31SruYE2pL7Sdc8Km-1rZPPf4Hn3r6NWSo9qmXKXz1Bno0YPLmpS1fbpT6K0uC6iV7W6Wz_slAWBE0ScUwi_lhiASQ6QL2kwi8tji_3laq9XfHbEIdOnXfRTd0yfn-zxchxASwUL93w-_0xfcMWklu6RdfKFCj_cnTW-yaYuDiL2VWCdKvF3LORe9YTa8uwqN4RD4urpEa8eTbRxAo_UybYBOYWwSrzfRuwY0JuAfEAgRRjMISmH7t_5-sFjTFPlOuENmMbYo6sKEIsGRhbgQQYT3CFV1k504qQQJOVQENgTYJTgHgf_lZgiNuBcFItr7blbu8mK-FzLYr33UPcYOSsbzSo105mQA_eWW7C7uOq0Wdk8TlYz41sJX41I=w1840-h1386-no?authuser=0)


![硬件解碼](https://lh3.googleusercontent.com/zeKHyezLs17T-bQNz7KCNJE0iBgrxlKkmBWK78xTWmjU43PZ9xkaSnBcetwrIBIG9LQ8lctjKXdrOym_QeixXJSjk3X304qtpZeohwQcn5Pbera7LmAUfA-6PSq0rkdqi5R_RUYOd2kTa0ecZpMDyEe-voH2JbiC-XcQ9ooJ4sJuFY3zf9ETwKx4RsyvrW5fEyb7-nwuIRv-8rijBl_YF2DHrKTo-6h94Q4y9uVTq5kmortdPrsfdw_9IkcKIigTaBWe6cbxg2JqrWvIkKuXWflxNS3eGCYRb-MovgB1PkYUx0za1BEmAQQXzcq2ZEwohLc2E53wJikVH-BeuY1lTP0vCYuxLAOen7L6JYTF__Mda8bcWbEdtfm6DNEcHX6SarFvwSgObd4B0_6eyE3ckib55s-uHmxzOuqe8LpRmTf2PmrUjwjvyBybSIFIBuYtJU7PGenKbl89PBjI9NI2_txaWl_n0D1s9rhVJQremFTE4nq0CF5QwCsGA0KxvsIewO-LvMEwNfvaV5uoXR6QC3nJni2UTAj31MNzDd_a8bU3bcDniDcVcDe23V1HJrSidAebIAvk_q-D3Yj4C5d4YeiGvp9wCKHqNUNbu2cA8yvSOUBA8Mu8H2o-iW8yXn-zcpEokfdJj_aP5Ey6SOkRipn1_ZHHVILUa8T3B8EWOGS-JmcqT_Yw3-iRPzoLaldfcF9y5CI5-ZGxD6qBmol2vJEXS6ohT7jyEXQeerYw1Y99qFBQJbR3tRVc3W7W1QqDUTiDnoBjwfG98b0xiAzEUbS3i6ugFsrkobHLEmyOgVe42an_nx4kP5ZXEHGhsWrpy1s=w1572-h1034-no?authuser=0)
