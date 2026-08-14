# RB-Endpoint-Hardening-Package
RB-Endpoint-Hardening-Package

Folder Structure

RB-Endpoint-Hardening/
├── README.md
├── GPO/
│   ├── RB-Allow-Approved-USB-Devices.txt
│   ├── RB-USB-Device-Installation-Control.txt
│   ├── RB-USB-Removable-Storage-Block.txt
│   ├── RB-Network-Adapter-Restriction.txt
│   └── RB-BitLocker-Baseline.txt
├── Dell-BIOS/
│   ├── RB_BIOS_Baseline.cctk
│   ├── Apply-Dell-BIOS.ps1
│   └── Dell-Command-Configure-Deployment.md
├── Scripts/
│   ├── Verify-USB-Policy.ps1
│   ├── Verify-BitLocker.ps1
│   ├── Verify-BIOS.ps1
│   └── Generate-Compliance-Report.ps1
├── Security-Groups/
│   ├── RB-USB-Exception-IT.txt
│   └── RB-BIOS-Exception-IT.txt
└── Compliance/
├── Branch-Checklist.docx
└── Audit-Procedure.md

GPO Deployment Order

1. RB-Allow-Approved-USB-Devices
2. RB-USB-Device-Installation-Control
3. RB-Network-Adapter-Restriction
4. RB-USB-Removable-Storage-Block
5. RB-BitLocker-Baseline
6. RB-Dell-BIOS-Deployment

GPO Link Order

1. RB-Allow-Approved-USB-Devices
2. RB-USB-Device-Installation-Control
3. RB-Network-Adapter-Restriction
4. RB-USB-Removable-Storage-Block
5. RB-BitLocker-Baseline
6. RB-Dell-BIOS-Deployment

Dell BIOS Startup Script

$DCC = “C:\Program Files\Dell\Command Configure\X86_64\cctk.exe”
$CFG = “\\RBL-DC01\NETLOGON\Dell\RB_BIOS_Baseline.cctk”

if (Test-Path $DCC) {
Start-Process -FilePath $DCC -ArgumentList “–import=$CFG” -Wait -WindowStyle Hidden
}

Verification Script

gpupdate /force
gpresult /h C:\RB-GPO-Report.html

Expected Result

Allowed:

* USB Keyboard
* USB Mouse
* USB Printer
* USB Scanner

Blocked:

* USB Pen Drive
* External HDD/SSD
* USB Wi-Fi Dongle
* USB Ethernet Adapter
* Bluetooth USB Adapter
* Mobile USB Tethering
* USB Boot
* Boot Order Changes
* BIOS Configuration Changes

This package is suitable for Dell OptiPlex, Dell Latitude, and Dell Precision systems joined to Active Directory in a banking environment.