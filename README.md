# WinPrep

Honestly, this is a custom configuration of [flare-vm](https://github.com/mandiant/flare-vm) with some additions for my needs.

## Installation

0. All commands below must be run in Powershell with an elevated context.

1. Disable Tamper Protection and any Anti-Malware solution.

    ```powershell
    Set-MpPreference -DisableRealtimeMonitoring $true -DisableScriptScanning $true -DisableBehaviorMonitoring $true -DisableIOAVProtection $true -DisableIntrusionPreventionSystem $true;
    cd $env:TEMP;
    Invoke-WebRequest https://github.com/ionuttbara/windows-defender-remover/releases/latest/download/DefenderRemover.exe -OutFile .\DefenderRemover.exe;
    .\DefenderRemover.exe A;

    ```

2. Disable automatic Windows updates.

    ```powershell
    New-Item HKLM:\SOFTWARE\Policies\Microsoft\Windows -Name WindowsUpdate;
    New-Item HKLM:\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate -Name AU;
    New-ItemProperty HKLM:\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU -Name NoAutoUpdate -Value 1;

    ```

    > it can be enabled back with:
    >
    > ```powershell
    > New-ItemProperty HKLM:\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU -Name NoAutoUpdate -Value 0
    > ```

3. VM configuration, don't forget to replace "YourPassword" with your password :)

    ```powershell
    cd $([Environment]::GetFolderPath("Desktop"));
    Invoke-WebRequest https://raw.githubusercontent.com/mandiant/flare-vm/main/install.ps1 -OutFile .\install.ps1;
    Unblock-File .\install.ps1;
    Set-ExecutionPolicy Unrestricted -Force;
    .\install.ps1 -noGui -customConfig "https://raw.githubusercontent.com/D00Movenok/WinPrep/refs/heads/main/config.xml" -customLayout "https://raw.githubusercontent.com/D00Movenok/WinPrep/refs/heads/main/layout.xml" -password "YourPassword";
    ```
