# Overview

`sunxi-armbian-flasher-osx` is an example application wrapper around `sunxi-fel` and https://github.com/zador-blood-stained/fel-mass-storage

It has been made using [Platypus](https://www.macupdate.com/app/mac/12046/platypus) and the main parts are contained inside the `Resources` directory and can also be updated there in place. 

The purpose is to [FEL boot](https://linux-sunxi.org/FEL) a sunxi device and then expose either SD card or eMMC as USB Mass Storage device to the host so the sunxi device itself acts as an card/eMMC reader and allows access to either SD card or eMMC. This way you can backup the eMMC contents or flash images directly to SD card or eMMC.

## Structure:

Application bundle structure currently looks like this:

	Contents:
		Info.plist
		MacOS:
			sunxi-armbian-flasher-osx
		Resources:
			AppSettings.plist
			fel-mass-storage:
				doc:
					BUILDING.md
					LICENSE.sunxi-tools
					LICENSE.u-boot
					u-boot.config
				fel-sdboot.img
				h3:
					boot.scr
					script.bin
					u-boot-sunxi-with-spl.bin
					uInitrd
					zImage
				README.md
				src:
					mass_storage
				start.bat
				start.sh
				win32:
					sunxi-fel.exe
			keys
			libusb-1.0.0.dylib
			MainMenu.nib
			script
			sunxi-fel

## Updating contents:

In case you want to update the `fel-mass-storage` stuff (eg. a new SoC has been added) simply chdir into the application bundle, then

    cd Contents/Resources
    rm -rf fel-mass-storage
    git clone https://github.com/zador-blood-stained/fel-mass-storage

If a new SoC should be supported you also have to adjust the case construct in the `StartFlashing` function inside `Contents/Resources/script`.

In case a newer `sunxi-fel` binary should be integrated you need to clone https://github.com/linux-sunxi/sunxi-tools and follow the instructions (requires a 
`brew install libusb` before). You need to move the new `sunxi-fel` binary and `libusb-1.0.0.dylib` into the `Resources` directory since the app expects it there. Also library paths have to be adjusted if you plan to distribute the application bundle later. Please see [here](http://forum.armbian.com/index.php/topic/2454-fel-mass-storage-or-writing-images-directly-to-emmc/?p=16463) for details how to use XCode's `install_name_tool` to do this.