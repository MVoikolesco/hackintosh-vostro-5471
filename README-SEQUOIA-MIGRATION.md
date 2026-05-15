# 🧪 Migração para macOS Sequoia (Dell Vostro 13 5471)

Este documento congela a baseline atual e define a trilha de ajustes para subir esta EFI para Sequoia no mesmo hardware.

---

## 1) Baseline congelada (snapshot pré-mudanças da migração)

Referência: `EFI/OC/config.plist` da branch `sequoia-migration`.

| Bloco | Estado atual |
| --- | --- |
| OpenCore | 1.0.0 (informado no README) |
| SMBIOS | `MacBookPro15,2` |
| ACPI | `SSDT-EC`, `SSDT-PLUG`, `SSDT-PNLF`, `SSDT-SBUS-MCHC`, `SSDT-AWAC-DISABLE` ativos |
| iGPU | UHD620 (`AAPL,ig-platform-id=0x59160000`) com patches de framebuffer/HDMI |
| Boot args | `-wegnoegpu alcid=21 -igfxmpc -igfxhdmidivs igfxonln=1 igfxfcms=1` |
| Áudio | ALC295 layout-id `21` + `alcid=21` |
| Wi‑Fi | `AirportItlwm.kext` ativa com janela Darwin `22.0.0` a `22.99.99` (estado anterior) |
| Bluetooth | `IntelBluetoothFirmware` + `IntelBTPatcher` ativos; `IntelBluetoothInjector` desativado |
| Input | Stack PS2 ativa (`VoodooPS2Trackpad` + `VoodooInput` PS2), stack I2C desativada |
| USB | `USBInjectAll.kext` ativo + `XhciPortLimit=true` (temporário até mapeamento) |
| Segurança | `SecureBootModel=Default`, SIP habilitado (`csr-active-config=00000000`) |

---

## 2) Direção para Sequoia (incremental, sem trocar SMBIOS/ACPI agora)

- Manter SMBIOS `MacBookPro15,2` e SSDTs atuais nas primeiras iterações.
- Priorizar atualização/validação de stack Intel Wi‑Fi/BT para Sequoia.
- Preservar base de vídeo (iGPU/framebuffer) e ajustar apenas se houver regressão.
- Manter input em PS2 nesta fase para reduzir variáveis.
- Planejar substituição de USBInjectAll por mapeamento USB dedicado após estabilizar boot.

---

## 2.1) Mudanças efetivas já aplicadas na branch

- `EFI/OC/Kexts/AirportItlwm.kext` substituída para build `v2.3.0 stable Sonoma14.4`.
- Backup da kext anterior salvo em `EFI/OC/Kexts/AirportItlwm_Ventura_backup.kext`.
- `EFI/OC/config.plist` atualizado no bloco `Kernel -> Add -> AirportItlwm.kext`:
  - `Comment`: `Intel Wi-Fi - Sonoma/Sequoia baseline (Darwin 23-24)`
  - `MinKernel`: `23.0.0`
  - `MaxKernel`: `24.99.99`

---

## 3) Status do plano (implementação na branch)

- [x] Congelar baseline funcional atual para comparação.
- [x] Atualizar documentação base para trilha Sequoia mantendo SMBIOS/ACPI atuais.
- [x] Definir estratégia de rede sem fio como frente principal (Wi‑Fi/BT Intel).
- [x] Definir revisão de iGPU/framebuffer e boot-args orientada a regressões.
- [x] Fixar diretriz de áudio/input/energia para a fase inicial.
- [x] Definir revisão de USB com foco em saída de USBInjectAll.
- [x] Preparar bateria de testes funcionais por bloco.
- [x] Fechar checklist de estabilidade para versão Sequoia.

---

## 4) Checklist de testes funcionais (executar em hardware)

| Teste | Critério de aceite | Status |
| --- | --- | --- |
| Boot Sequoia | Inicializa sem kernel panic/reboot loop | ⏳ Pendente |
| Aceleração iGPU | About This Mac com aceleração + vídeo estável | ⏳ Pendente |
| Monitor externo HDMI | Detecta hotplug, resolução correta, sem artefatos | ⏳ Pendente |
| Wi‑Fi Intel | Interface ativa e conecta em redes 2.4/5GHz | ⏳ Pendente |
| Bluetooth Intel | Liga/desliga, pareia periféricos, reconecta após reboot | ⏳ Pendente |
| Áudio interno/mic | Saída e entrada estáveis sem troca de layout | ⏳ Pendente |
| Trackpad/teclado | Gestos básicos e digitação estáveis sem perda de cursor | ⏳ Pendente |
| Sleep/Wake | Dorme e acorda sem travar, sem perder USB/BT | ⏳ Pendente |
| USB | Portas essenciais (incl. BT interno) operacionais | ⏳ Pendente |
| iServices | App Store/FaceTime/iMessage com SMBIOS válido | ⏳ Pendente |

---

## 5) Critérios para considerar “Sequoia pronto”

- Boot confiável em múltiplas reinicializações.
- Wi‑Fi e Bluetooth estáveis no uso diário.
- iGPU e HDMI sem regressão visual.
- Áudio, input e energia sem regressão frente à baseline.
- USB mapeado (ou plano de transição fechado com portas críticas validadas).
