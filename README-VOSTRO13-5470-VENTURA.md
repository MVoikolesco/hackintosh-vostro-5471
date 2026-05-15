# EFI Vostro 13 5471 - macOS Ventura

Esta pasta foi preparada para:
- Dell Vostro 13 5471
- Intel Core i5-8250U
- Intel UHD 620
- dGPU Radeon desativada no boot
- OpenCore com tema Flavours-OSX26

## Estrutura
- EFI pronta em `EFI_Vostro13_5471_Ventura/EFI`
- Config principal em `EFI_Vostro13_5471_Ventura/EFI/OC/config.plist`

## Ajustes aplicados
- SMBIOS para MacBookPro15,2
- iGPU ajustada para UHD 620 (ig-platform-id 0x59160000)
- boot-args: `-wegnoegpu alcid=11`
- Radeon dGPU desativada via boot-arg (WhateverGreen)
- SecureBootModel em Default
- Power Management de CPU reforçado com `AppleCpuPmCfgLock`, `AppleXcpmExtraMsrs`, `AppleXcpmForceBoost` e `ProvideCurrentCpuInfo`
- AirportItlwm limitado ao kernel do Ventura (22.x)
- Kexts essenciais habilitadas:
  - Lilu
  - VirtualSMC
  - SMCProcessor
  - SMCBatteryManager
  - WhateverGreen
  - AppleALC
  - VoodooPS2Controller (teclado)
  - VoodooI2C + VoodooGPIO + VoodooI2CServices + VoodooInput
  - VoodooI2CELAN
  - RealtekRTL8111
  - IntelBluetoothFirmware
  - IntelBTPatcher
  - USBInjectAll (temporario)
- Tema OpenCanopy configurado em `chris1111\\Flavours-OSX26`

## BIOS recomendada
- UEFI: Enabled
- Secure Boot: Disabled
- SATA: AHCI
- VT-d: Disabled
- Fast Boot: Disabled

## Muito importante antes de usar
1. Gerar SMBIOS novo (Serial, MLB e UUID) para esta máquina.
2. Se o Wi-Fi não subir no Ventura, substituir AirportItlwm.kext por build compatível com Ventura (ou usar itlwm + HeliPort).
3. Fazer Reset NVRAM no primeiro boot pelo OpenCore.
   - Obrigatório para aplicar os novos ajustes de gerenciamento de energia da CPU e do trackpad I2C.

## Observações
- O codec de áudio pode variar. Se não houver som, testar outros layout-id (ex: 3, 11, 13, 21, 28).
- USBInjectAll está temporário. O ideal é mapear USB depois que o sistema estiver estável.
- Stack de trackpad foi ajustada para ELAN I2C, com plugins PS2 de mouse/trackpad desativados para evitar conflito.
- SSDTs atuais (EC/PLUG/PNLF/SBUS-MCHC/AWAC-DISABLE) foram revisados e mantidos ativos.
