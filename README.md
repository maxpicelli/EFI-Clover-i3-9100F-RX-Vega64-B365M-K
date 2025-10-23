# EFI Clover - i3-9100F + RX Vega 64 + ASUS B365M-K

![Clover](https://img.shields.io/badge/Clover-5164-blue)
![macOS](https://img.shields.io/badge/macOS-Tahoe%2026.0.1-brightgreen)
![macOS](https://img.shields.io/badge/macOS-Ventura%2013.7.8-success)

EFI do **Clover 5164** para Hackintosh com Intel Coffee Lake 9ª geração.

---

## 💻 Hardware

| Componente | Modelo |
|------------|--------|
| **CPU** | Intel Core i3-9100F @ 3.60GHz |
| **Placa-Mãe** | ASUS PRIME B365M-K |
| **RAM** | 32GB (2x16GB) Corsair DDR4 2400MHz |
| **GPU** | AMD Radeon RX Vega 64 8GB |
| **SSD** | Samsung 970 EVO Plus NVMe |
| **Ethernet** | Realtek RTL8168 |
| **Wi-Fi** | Broadcom BCM43A0 |
| **SMBIOS** | MacPro7,1 |

---

## ✅ Funcionando

- macOS Tahoe 26.0.1 e Ventura 13.7.8
- AMD RX Vega 64 (aceleração nativa)
- Áudio (AppleALC - layout-id 1)
- Ethernet Realtek
- Wi-Fi e Bluetooth
- USB (todas as portas)
- Sleep/Wake
- Power Management
- NVMe

---

## 📂 Estrutura

```
EFI/
├── BOOT/
│   └── BOOTX64.efi
└── CLOVER/
    ├── ACPI/patched/      # SSDTs para ACPI
    ├── config.plist       # Configuração principal
    ├── drivers/UEFI/      # Drivers essenciais
    ├── kexts/Other/       # Kexts necessárias
    └── themes/Glass/      # Tema
```

**Kexts principais:**
- Lilu 1.7.2
- AppleALC 1.9.6
- FakeSMC 3.5.6
- RealtekRTL8111 2.4.2
- NVMeFix 1.1.4
- USBPorts (mapeamento customizado)

---

## ⚙️ BIOS (UEFI)

### Desabilitar:
- Fast Boot
- Secure Boot
- CSM
- VT-d (ou use com DisableIoMapper)

### Habilitar:
- VT-x
- Above 4G Decoding
- Hyper-Threading
- XHCI Hand-off
- SATA Mode: **AHCI**

---

## 🚀 Instalação

### 1. Preparar USB
```bash
# Criar instalador (macOS)
sudo /Applications/Install\ macOS\ Ventura.app/Contents/Resources/createinstallmedia --volume /Volumes/USB

# Montar EFI
sudo diskutil mount disk0s1
```

### 2. Copiar EFI
- Copie a pasta `EFI` para a partição EFI do USB

### 3. Gerar Serial (OBRIGATÓRIO!)

⚠️ **NÃO use os serials deste repositório!**

```bash
# Use GenSMBIOS
./GenSMBIOS.command
# 1. Install MacSerial
# 3. Generate SMBIOS
# Digite: MacPro7,1
```

Edite em `config.plist`:
- `SMBIOS → SerialNumber`
- `SMBIOS → BoardSerialNumber`
- `SMBIOS → SmUUID`
- `RtVariables → MLB`

Verifique em: https://checkcoverage.apple.com
- ✅ "Unable to verify" = OK
- ❌ "Valid Purchase Date" = Gere outro

### 4. Instalar
1. Boot pelo USB
2. Selecione instalador no Clover
3. Formate disco como APFS/GUID no Utilitário de Disco
4. Instale o macOS
5. Copie EFI para disco interno após instalação

---

## 🔧 Configurações

### Boot Arguments
```
alcid=1 revcpu=1 revpatch=cpuname,sbvmm,diskread,pci -wegnoigpu watchdog=0 -lilubetaall
```

### Quirks Importantes
- `DisableIoMapper`: True
- `XhciPortLimit`: True
- `AppleRTC`: True
- `BlockSkywalk`: True

### Áudio
- Codec: Realtek ALC887
- Layout-id: 1 (teste 2, 3 ou 5 se não funcionar)

---

## 🐛 Problemas Comuns

| Problema | Solução |
|----------|---------|
| Não boota | Desabilite Secure Boot na BIOS |
| Kernel Panic | Adicione `-v` para ver detalhes |
| Sem áudio | Teste `alcid=2` ou `alcid=3` |
| USB não funciona | Verifique USBPorts.kext e XhciPortLimit |
| Ethernet não funciona | Instale RealtekRTL8111.kext |

**Logs:** `/EFI/CLOVER/misc/preboot.log`

---

## 📝 Notas

- **SIP**: Parcialmente desabilitado (`0xA87`)
- **FileVault**: Suportado
- **iGPU**: i3-9100F não possui gráficos integrados
- **Versão do Clover**: 5164 (17/10/2024)

---

## 🔗 Links

- [Clover Releases](https://github.com/CloverHackyColor/CloverBootloader/releases)
- [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS)
- [Acidanthera](https://github.com/acidanthera)
- [r/hackintosh](https://www.reddit.com/r/hackintosh/)

---

## ⚖️ Aviso

Projeto educacional. macOS é propriedade da Apple Inc. Use por sua conta e risco.

---

**Última atualização:** 22/10/2025 | **Clover:** 5164 | **Testado:** Tahoe 26.0.1 & Ventura 13.7.8

