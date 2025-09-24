# AT2020-USB-Fix
AT2020 USB/USB+ microphones can cause a frustrating issue where:

* Browser tabs won't load until you click away or switch to another tab
* Spotify desktop app won't play music
* YouTube/media players auto-rewind constantly
* Windows key may stop working

This is caused by the microphone's HID-interface continuously sending phantom "mediatrackprevious" button presses.

## Quick Fix (Powershell)
Run PowerShell as Administrator and use one of these commands:

Check device status to confirm we can detect the HID interface:
```powershell
Get-PnpDevice | Where {$_.HardwareID -like "*VID_0909&PID_001C*MI_03*"} | Select Name, Status
```
The command should display the following when the HID-Interface is enabled:
```powershell
Name                                  Status
----                                  ------
HID-compliant headset                 OK
USB Input Device                      OK
HID-compliant consumer control device OK
```
Run this command to disable the problematic HID device & fix the issue:
```powershell
Get-PnpDevice | Where {$_.HardwareID -like "*VID_0909&PID_001C*MI_03*"} | Disable-PnpDevice -Confirm:$false -ErrorAction SilentlyContinue
```
Run the device status command again to verify that the HID device is disabled (alternatively confirm with device manager):
```powershell
Name                                  Status
----                                  ------
HID-compliant headset                 Unknown
USB Input Device                      Error
HID-compliant consumer control device Unknown
```
The fix persists through reboots, If Windows Update re-enables the device, just run the disable command again.

## Alternative Temporary Fix
If you can't run PowerShell commands:
1. Unplug the USB cable from the AT2020 microphone (or from the mic itself).
2. Plug it back in.
3. This should temporarily fix the issue but it will return after a reboot.

⭐ If this helped you, please star this repo to help others find it!
