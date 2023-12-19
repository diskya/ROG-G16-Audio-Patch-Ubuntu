# Ubuntu Audio Patch for ROG Zephyrus G16 (2023 GU603V)

# ROG幻16 2023版 Ubuntu 声音补丁

If your ROG G16 experiences distorted audio on Ubuntu, which is likely due to bass speakers not working, here's the quick fix to save you some time.
For other ASUS models, see the fix on [ASUS Linux Wiki](https://asus-linux.org/wiki/cirrus-amps/)

```bash
mkdir cirrus && cd cirrus
```
Download cirrus_ssdt_patch.dsl and copy to /cirrus

Download the 3 files starting with cs35l41, and copy to /lib/firmware/cirrus/

```bash
sudo cat /sys/firmware/acpi/tables/DSDT > dsdt.dat
iasl -d dsdt.dat
iasl -tc cirrus_ssdt_patch.dsl
mkdir -p kernel/firmware/acpi
cp cirrus_ssdt_patch.aml kernel/firmware/acpi
find kernel | cpio -H newc --create > patched_cirrus_acpi.cpio
sudo cp patched_cirrus_acpi.cpio /boot/patched_cirrus_acpi.cpio

# If you use grub for booting, update your /etc/default/grub to add the line:
GRUB_EARLY_INITRD_LINUX_CUSTOM="patched_cirrus_acpi.cpio"

# If you use systemd-boot for booting, please refer to ASUS Linux Wiki above.
```

After reboot, the sound should be largely improved.
