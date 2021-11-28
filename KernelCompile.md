# Credits to Jeff Geerling
These instructions have been pulled from https://github.com/geerlingguy

# Install dependencies
sudo apt install -y git bc bison flex libssl-dev make libncurses5-dev

# Clone source
git clone --depth=1 https://github.com/raspberrypi/linux

# Apply default configuration
cd linux
export KERNEL=kernel7l # use kernel8 for 64-bit, or kernel7l for 32-bit
make bcm2711_defconfig

# Customize the .config further with menuconfig
make menuconfig
# Enable the following:
# Device Drivers:
#   -> Serial ATA and Parallel ATA drivers (libata)
#     -> AHCI SATA support
#     -> Marvell SATA support
#
# Alternatively add the following in .config manually:
# CONFIG_ATA=m
# CONFIG_ATA_VERBOSE_ERROR=y
# CONFIG_SATA_PMP=y
# CONFIG_SATA_AHCI=m
# CONFIG_SATA_MOBILE_LPM_POLICY=0
# CONFIG_ATA_SFF=y
# CONFIG_ATA_BMDMA=y
# CONFIG_SATA_MV=m

nano .config
# (edit CONFIG_LOCALVERSION and add a suffix that helps you identify your build)

# Build the kernel and copy everything into place
make -j4 zImage modules dtbs # 'Image' on 64-bit
sudo make modules_install
sudo cp arch/arm/boot/dts/*.dtb /boot/
sudo cp arch/arm/boot/dts/overlays/*.dtb* /boot/overlays/
sudo cp arch/arm/boot/dts/overlays/README /boot/overlays/
sudo cp arch/arm/boot/zImage /boot/$KERNEL.img