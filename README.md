# Omarchy-setup
My way of setting up Omarchy.  
## Installation
ISO : https://iso.omarchy.org/omarchy-3.1.4.iso  
Etcher : https://etcher.balena.io/#download-etcher
### Dual boot
1. Edit `/boot/limine.conf`  with `sudo nvim /boot/limine.conf`
2. Add  
```
 /Window
     protocol: efi
     path: uuid(xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx):/EFI/Microsoft/Boot/bootmgfw.efi
```
  - [How to find UUID](dualboot-detail.md)
3. Execute `limine -update`
