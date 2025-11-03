#Detail for dual boot setup
## Find Window EFI partition  
1. Find disk  

    `lsblk -f` to list all disks.  
    It should look some like:
    ```
    NAME        FSTYPE LABEL  UUID                                 MOUNTPOINT
    nvme0n1
    ├─nvme0n1p1 vfat   SYSTEM 1234-ABCD                             /boot/efi
    ├─nvme0n1p2 ntfs   Windows C 1111111111111111
    └─nvme0n1p3 ext4   ArchRoot abcdefgh-1234-5678-90ab-cdef12345678 /
    ```
    Look for:
    - `vfat`
    - `SYSTEM`
    - `boot/efi`

2. Confirm(Optional, if you are unsure)
  
    Mount to confirm:
    ```
    sudo mkdir /mnt/esp
    sudo mount /dev/DISKNAME /mnt/esp #replace DISKNAME with the name of the disk you found
    ls /mnt/esp/EFI/Microsoft/Boot/
    ```
    If you see `bootmgfw.efi`, that’s your Windows EFI partition.  

    After you checked, you can unmount by:
   ```
   cd ~
   sudo umount /mnt/esp
   ```

## Find PARTUUID
Once you know which partition is the Windows EFI one (say /dev/nvme0n1p1), get its PARTUUID using:
```
sudo blkid /dev/nvme0n1p1
```
Example output:
```
/dev/nvme0n1p1: UUID="1234-ABCD" TYPE="vfat" PARTUUID="abcd1234-ef56-7890-ab12-cd34ef567890"
```

