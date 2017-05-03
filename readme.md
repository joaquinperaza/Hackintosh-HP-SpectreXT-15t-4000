# Hackintosh Spectre XT 15t-400 Touchsmart Sierra

- I made this repo to collect all related info about this laptop.
- I am not using other dsdt or ssdt patches than clover ones, but I am open to your contributions.

### Where to start:
Have a Mac? So prepare a USB using your preferred method, you can choose:
- Clover
- Unibeast (Base guide: https://www.tonymacx86.com/threads/unibeast-install-macos-sierra-on-any-supported-intel-based-pc.200564/)


Dont have a mac? Dont worry you can use a virtual box machine in your HP:
To set up the vm use: http://www.wikigain.com/install-macos-sierra-10-12-virtualbox/

`I discovered and posted how to run unibeast in VirtualBox VM using Linux/Windows host:`
https://www.reddit.com/r/hackintosh/comments/5zks4c/create_sierra_bootable_usb_from_virtualbox/

I followed this guide that was the most similar to this model:
https://www.google.com.uy/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&cad=rja&uact=8&ved=0ahUKEwiT2KW8v9HTAhWGIJAKHRtkAZIQFgggMAA&url=https%3A%2F%2Fwww.reddit.com%2Fr%2Fhackintosh%2Fcomments%2F5pwtdd%2Fhp_spectre_xt_pro_13b000_running_macos_sierra%2F&usg=AFQjCNEXuPraFVEtqD7am308-wVd7ssCZQ

So I did it with unibeast, with this options:
UEFI Mode and Inject Intel

### USB installer tunning:

- After completing the Unibeast installation donwload clover configurator and add the settings that appear in cloverconfIMG folder.
- PD:You should also select -v (verbose) flag in boot section in first tests.
- PD:Ignore ket patches for the moment
- Copy the drivers inside the drivers64UEFI folder to your EFI/CLOVER/drivers64UEFI folder.
- Copy the kexts from the kext folder to your EFI/CLOVER/kexts `EXCLUDE VoodooHDA inside kext/10.12 folder`.

### Sierra install:
Now you're ready to run Sierra installer (if you are lucky the only issue you would have is no mouse and keyboard during install).
Follow normal instructions for install OSX.
Key points:
- Disable Secure boot in bios and disable legacy, enable cpu virtualization
- Boot vith -v flags in first tests.

### Post installation:
Install those 'post install kext' with EasyKext Pro:
- VoodooPS2Controller.kext (Enable mouse and keyboard)
- USBInjectAll.kext (enable touchscreen and webcam)
Dont install the optionals fixes yet.

Reagarding audio i have tested two solution, choose one:
- Apply AppleHDA patched kext with EasyKext (Inside other files folder) //No microphone nor beats audio subwoofer
- Use VoodooHDA installer and add the `VoodooHDA inside kext/10.12 folder` to clover.
VoodooHDA provide microphone and beats audio but less quality ~~and no volume control~~.
VoodooHDA volume workaround:
- Download soundflower from this repo
- Go to Audio Midi Setup in Utilities and create a new multi-output device
- Check both speakers
- Uncheck drift correction if it is automatically checked for you
- Open soundflower and select the multi-output device with 2ch Soundflower, then select soundflower as output device
- Go to 'Users and Groups' in system preferences, select 'login items', and add 'Soundflowerbed.app' to the list. It's located in the sound flower folder now in your Applications

Hdmi fix:
The plattform 0x01660004 dont have connectors, but is the onlyone compatible with our screen, so I patched the HEX table for you to make our screen compatible with 0x01660008 wich natively support our HDMI port.
Just delete AppleIntelFramebufferCapri.kext from your System/Library/Estensions folder and immediately install the one inside kext folder/optional fixes with EasyKext Pro
Then add this kextspatches in CloverConfigurator and change 0x01660004 in graphic section to 0x01660008:


| Name | Find | Replace | Plist checked? |
| ------ | ------ | ------ | ------ |
| AppleIntelFramebufferCapri | 04006601 01030101 00000002  | 04006601 01020402 00000004  | NO |
| AppleIntelFramebufferCapri | 05030000 02000000 30020000 00000000 01000000 40000000 00000000 01000000 40000000 00000000 01000000 40000000 00000000 00000000 | 05030000 02000000 30020000 02050000 00080000 06000000 03040000 00040000 81000000 04060000 00040000 81000000 00000000 00020011 | NO |

Wifi fix:
Unfortunately our Intel Centrino N wifi card is not compatible i bought an Atheros AR9285 AR5B95, just replace and add install kexts inside Other files/Atheros AR9285 AR5B95 Sierra with EasyKext.

| Name | Find | Replace | Plist checked? |
| ------ | ------ | ------ | ------ |
| AirPortAtheros40 | pci168c,30 | pci168c,2b | YES |

### Author Disclaimer:
The majority of the software is public on the internet, I only have patched some files and wrote this guide, I am not responsible of any possible damage to your PC, do it at your own risk despite I consider it worth it.
If you have any question/issue/suggestion/collaboration please report it via issue tab I will look it immediately.

As i started from zero i know this guide will look chinese, but I would be glade to help you, just report the issue to clarify the unknown for future readers.
