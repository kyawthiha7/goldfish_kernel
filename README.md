# install twrp in android emulator

## tools we need
- android ndk (https://developer.android.com/ndk/downloads/index.html) 
- goldfish kernel (3.18) (https://android.googlesource.com/kernel/goldfish/)
- twrp for android emulator (https://twrp.me/devices/androidemulator.html)


first things first , need to compile goldfish kernel to run in emulator
so first choose compile machine and emulator running machine.
android ndk will depend on your chosen compile machine
I choose Ubuntu 64 bit for goldfish compile and emulator for windows 10
- download and extract android ndk
- download and extract goldfish kernel 3.18 older version don't work with emulator anymore
- find goldfish_defconfig file at older goldfish kernel and place at arch/arm/config directory ( this is if you want to compile arm kernel)
- at goldfish kernel directory, compile as following
```
make ARCH=arm goldfish_defconfig
make ARCH=arm SUBARCH=arm CROSS_COMPILE=/directory-of-android-ndk/toolchains/toolchains/arm-linux-androideabi-4.9/prebuilt/linux-x86_64/bin/arm-linux-androideabi- -j5
```

- if driver/net/ethernet/smsc error show up , edit driver/net/ethernet/Kconfig 
```
config SMC91X
        tristate "SMC 91C9x/91C1xxx support"
        select CRC32
        select MII
        depends on (ARM || M32R || SUPERH || MIPS || BLACKFIN || \
                    MN10300 || COLDFIRE || ARM64 || XTENSA) && (!OF || GPIOLIB)
```
- then zImage will be ready
- run emulator with zimage and twrp image as follow
```
emulator.exe -avd Nexus_6_API_23 -ramdisk android-emulator-twrp\twrp-3.1.0-0-twrp.img -kernel android-emulator-twrp\bzImage
```
prebuild will be found in this repo
bzImage for x86 and zImage for arm
