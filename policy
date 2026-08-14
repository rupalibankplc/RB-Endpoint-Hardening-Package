For a banking environment (Rupali Bank PLC), I would implement a GPO + BIOS baseline that prevents almost all unauthorized Windows reinstallation attempts while keeping IT administrators fully in control. The key principle is: users cannot boot external media, cannot change firmware settings, and cannot access recovery tools without authorization.

BIOS / UEFI baseline (HP 250 G8)

Configure these manually or through HP BIOS Configuration Utility (BCU) / HP Client Management Script Library.

BIOS Setting	Recommended
Administrator (Setup) Password	Set and keep in IT vault
Power-On Password	Optional for high-security devices
USB Boot	Disabled
External Device Boot	Disabled
Network/PXE Boot	Disabled (unless IT deployment network)
Boot Order	OS Boot Manager first
Legacy Support	Disabled
Secure Boot	Enabled
TPM	Enabled
Allow BIOS Updates	Admin only

This alone prevents booting a Windows installer from USB or changing firmware settings.

Active Directory Group Policy baseline

Create a GPO named RB-Endpoint-Security-Baseline and link it to the OU containing branch workstations.

BitLocker (mandatory)

Computer Configuration → Administrative Templates → Windows Components → BitLocker Drive Encryption

Enable:

* Require additional authentication at startup
* Allow TPM
* Store BitLocker recovery information in Active Directory Domain Services
* Do not enable BitLocker until recovery information is stored

This ensures every workstation’s recovery key is escrowed in AD.

Disable Windows Recovery Environment

Computer Configuration → Administrative Templates → System → Recovery

Enable:

* Turn off Windows Recovery Environment

This prevents users from easily accessing advanced recovery and reset options.

Remove local administrator rights

Use Restricted Groups or Group Policy Preferences.

Computer Configuration → Preferences → Control Panel Settings → Local Users and Groups

Ensure only:

* Domain Admins
* IT Support Group
* Authorized Service Accounts

are members of Administrators.

Prevent installation media and USB devices

Computer Configuration → Administrative Templates → System → Removable Storage Access

Enable:

* All Removable Storage classes: Deny all access

If branch operations require USB for printers or scanners, create exceptions via device installation policies.

Block unauthorized installers

Use AppLocker.

Computer Configuration → Windows Settings → Security Settings → Application Control Policies → AppLocker

Allow:

* %WINDIR%\*
* %PROGRAMFILES%\*
* Signed Microsoft binaries

Block execution from:

* USB drives
* Downloads folder
* Temp folders

Device installation restrictions

Computer Configuration → Administrative Templates → System → Device Installation → Device Installation Restrictions

Enable:

* Prevent installation of removable devices
* Prevent installation of devices not described by other policy settings

Audit and monitoring

Enable:

* Audit Process Creation
* Audit Removable Storage
* Audit Logon Events

Forward logs to Wazuh / SIEM / Windows Event Collector.

BIOS password management

For HP devices, use HP BIOS Configuration Utility (BCU) or HP Client Management Script Library to:

* Set BIOS passwords remotely
* Disable USB boot
* Enforce Secure Boot
* Export and import BIOS configurations

Result

With this baseline, a branch user cannot:

* Boot a Windows installation USB
* Change boot order
* Disable Secure Boot
* Clear TPM
* Access Windows Recovery to reset the PC
* Install unauthorized operating systems
* Read the disk if removed from the laptop

An IT administrator can still reinstall Windows using the BIOS password and approved deployment media.

For Rupali Bank, I would package this as “Branch Workstation Hardening Baseline v1.0” and deploy it through Active Directory GPO + HP BCU, with BitLocker recovery keys stored in AD and monitored through your existing infrastructure.