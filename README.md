# 💻 Dell Vostro 13 5471 - Hackintosh (macOS Ventura)

<img width="313" height="530" alt="WhatsApp Image 2026-05-03 at 22 03 06" src="https://github.com/user-attachments/assets/4432a9a3-8f3c-4100-bc80-f01ee0516dd3" />

---

## 📌 EFI Details

* **macOS**: Ventura (13.x)
* **OpenCore**: 1.0.0
* **Release date**: 03/05/2026

---

## 🧠 Specifications

| Component   | Description                  |
| ----------- | ---------------------------- |
| CPU         | Intel Core i5-8250U          |
| RAM         | 16GB DDR4                    |
| Storage     | NVMe (varia por modelo)      |
| iGPU        | Intel UHD Graphics 620       |
| dGPU        | AMD Radeon (desativada)      |
| Ethernet    | Realtek RTL8111/8168/8411    |
| Wi-Fi       | Intel (AirportItlwm / itlwm) |
| Bluetooth   | Intel                        |
| Audio       | Realtek (layout-id 21)       |
| Display     | 13.3" 60Hz                   |
| Webcam      | USB                          |
| Card Reader | USB                          |
| Input       | PS/2 (Synaptics / ELAN)      |

---

## 📂 Estrutura

```
EFI_Vostro13_5471_Ventura/
└── EFI/
    └── OC/
        └── config.plist
```

---

## ⚙️ Configuração

### 🔧 Ajustes aplicados

* SMBIOS: **MacBookPro15,2**
* iGPU: `0x59160000` (UHD 620)
* Boot-args:

  ```
  -wegnoegpu alcid=21
  ```
* dGPU desativada (WhateverGreen)
* SecureBootModel: Default
* Power Management (CPU) reforçado com `AppleCpuPmCfgLock`, `AppleXcpmExtraMsrs`, `AppleXcpmForceBoost` e `ProvideCurrentCpuInfo`
* AirportItlwm compatível com Ventura (22.x)

---

## 🧩 Kexts

* Lilu
* VirtualSMC
* SMCProcessor
* SMCBatteryManager
* WhateverGreen
* AppleALC
* VoodooPS2Controller (+ plugins)
* RealtekRTL8111
* IntelBluetoothFirmware
* IntelBTPatcher
* USBInjectAll *(temporário)*

---

## 🎨 Tema

* OpenCanopy: **Flavours-OSX26**
  (`chris1111/Flavours-OSX26`)

---

## 🧬 BIOS Setup

* UEFI: Enabled
* Secure Boot: Disabled
* SATA Mode: AHCI
* VT-d: Disabled
* Fast Boot: Disabled

---

## ✅ Working

* Intel UHD 620
* Áudio + Microfone
* USB
* Wi-Fi
* Bluetooth
* Teclado e Trackpad

---

## ⚠️ Não funcionando / Limitado

* Sleep instável
* AirDrop limitado (Wi-Fi Intel)
* USB não mapeado (uso de USBInjectAll)

---

## 🚨 Importante

Antes do primeiro boot:

1. Gere um novo SMBIOS:

   * Serial
   * MLB
   * UUID

2. Faça **Reset NVRAM** pelo OpenCore

   > Obrigatório após trocar a EFI para aplicar os novos ajustes de energia da CPU e de input.

3. Wi-Fi não funcionando?

   * Use versão correta do AirportItlwm para Ventura
   * Alternativa: `itlwm + HeliPort`

---

## 📝 Observações

* Layout padrão atual de áudio: `21` (prioriza speakers internos)
* Se não houver áudio, teste:

  ```
  alcid=3, 11, 13, 21, 28
  ```

* USBInjectAll é temporário
  → Faça mapeamento USB após estabilidade

---

## 👆 Trackpad

* Trackpad ajustado para stack PS/2 estável (`VoodooPS2Trackpad` + `VoodooInput` do PS2)
* `VoodooPS2Mouse` desativado para priorizar a identificação de trackpad nas Preferências do Sistema
* Stack I2C (`VoodooI2C`/`VoodooI2CELAN`) desativada temporariamente para evitar perda total do cursor
* SSDTs atuais (`SSDT-EC`, `SSDT-PLUG`, `SSDT-PNLF`, `SSDT-SBUS-MCHC`, `SSDT-AWAC-DISABLE`) mantidos ativos

---

## ⭐ Créditos

* **Desenvolvido por:** https://github.com/MVoikolesco
* OpenCore Team
* Acidanthera
* Comunidade Hackintosh


## ⚠️ Aviso

Este projeto é apenas para fins educacionais.
Use por sua conta e risco.
