# rkDevelopTool, but can be compiled by MinGW toolkit
#### if you ask me why, just because official RKDevTool tool is too unstable and it doesn't have some features that I need. also official rkDevelopTool repo sucks, kinda shame that cannot be compiled easily

btw this tool can be used as like `upgrade_tool` but some features losted like "DI", but you can use it to push binaries directly

## Compiling
To compile this tool you should have packages [`libusb-1.0`](https://packages.msys2.org/base/mingw-w64-libusb) ~~and `iconv`~~ installed.
> (the main rkdeveloptool use `iconv` I have no idea for what reason, but you can still compile this tool without it, so I removed it from dependencies)

### Build environment: MinGW x64 with UCRT variant
```shell
# 1. update repo database and install libusb-1.0 dependency. 
# can be skipped if you have libusb latest version already
# "mingw-w64-ucrt-x86_64-libusb" can be changed to "mingw-w64-x86_64-libusb" if you dont use ucrt variant
# package details: https://packages.msys2.org/base/mingw-w64-libusb
pacman -Sy
pacman -Sq mingw-w64-ucrt-x86_64-libusb

# 2. clone this repository and cd to this folder
git clone https://github.com/appleneko2001/rkdeveloptool-mingw; cd rkdeveloptool-mingw 

# 3. prepare build directory by using cmake
cmake -B build

# 4. build it 
cmake --build build
```
<!-- prob not necessary but i leave it :P-->
<!--
# 5. time to use this tool enjoy!
cd build
./rkdevtool.exe
```
-->

## Usage
> [!IMPORTANT]
> Replace your Rockchip download driver with "[Zadig](https://zadig.akeo.ie/)" to allow this tool access your Rockchip-based SBC or devices! 

All supported commands can be queried by using command `./rkdevtool.exe -h`
Example usage output:
```
---------------------Tool Usage ---------------------
Help:                   -h or --help
Version:                -v or --version
ListDevice:             ld
DownloadBoot:           db <Loader>
UpgradeLoader:          ul <Loader>
ReadLBA:                rl  <BeginSec> <SectorLen> <File>
WriteLBA:               wl  <BeginSec> <File>
WriteLBA:               wlx  <PartitionName> <File>
WriteGPT:               gpt <gpt partition table>
WriteParameter:         prm <parameter>
PrintPartition:         ppt
EraseFlash:             ef
TestDevice:             td
ResetDevice:            rd [subcode]
ReadFlashID:            rid
ReadFlashInfo:          rfi
ReadChipInfo:           rci
ReadCapability:         rcb
PackBootLoader:         pack
UnpackBootLoader:       unpack <boot loader>
TagSPL:                 tagspl <tag> <U-Boot SPL>
-------------------------------------------------------
```

## Examples
### 1. Download kernel.img binary to kernel partition
```shell
# download usbplug to device
./rkdevtool db RKXXLoader.bin

# 0x8000 is base of kernel partition,unit is sector.
./rkdevtool wl 0x8000 kernel.img

# reset device
./rkdevtool rd
```
