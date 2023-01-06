# Steps To Flash Raspberry Pi OS On CM4 eMMC

Today we are going to play with Raspberry Pi Compute Module 4, by trying to flash its **eMMC** storage with Rasbian OS instead of MicroSD Card.

This post shows how to mount eMMC storage on another computer (MAC in my case), and then how to flash a new Operating System image to it.

## IO Board mount preparation 
First of all you need the [Raspberry Pi Offical IO Board](https://www.raspberrypi.com/products/compute-module-4-io-board/ "IO Board").

![CM4 IO BOARD](images/cm4-io-board.png "IO BOARD")

Before you can put the eMMC storage into USB mass storage mode, you have to put a **jumper** over the first set of pins on the board--the jumper labeled **"Fit jumper to disable eMMC Boot"**.

Then, plug a USB cable from your computer (in my case, Mac--but it could be a Windows or Linux too) into the **USB Slave** micro USB port on the IO Board, and plug in power.

The board will power on, and you will see the red **D1 LED** turn on, but the Compute Module won't boot. The eMMC should now be ready for the next step.

## Using usbboot script to mount the eMMC storage
The next step is to download the Raspberry Pi usbboot repository, install a required USB library on your computer, and build the `rpiboot` executable, which you will use to mount the storage on your computer. I did all of this in the Terminal application on my Mac

First, be sure that `libusb` library has been installed:
- On my Mac, I used homebrew (package manager), to run this command:
``` 
$ brew install pkgconfig libusb
```
- On Linux, use terminal to run: 
``` 
$ sudo apt install libusb-1.0-0-dev
```

Second, clone the usbboot repository to your computer:
``` 
$ git clone --depth=1 https://github.com/raspberrypi/usbboot 
```

Third, `cd` into the `usbboot` directory and build using `rpiboot`:
```
$ cd usbboot
$ make
```

Now there should be an `rpiboot` executable in the directory. To mount the eMMC storage, run:
```
$ sudo ./rpiboot
```

And few seconds later, after if finishes doing its magic, you should see the `boot` volume mounted on your Mac (or whatever OS your are using).
You might also notice the D2 LED lighting up; that indicate there is disk **read/write** activity on the eMMC.

## Flashing Raspberry Pi OS onto the eMMC
Moment of truth, now the eMMC storage behaves just like a **microSD card** or **USB drive** that you plugged into your computer. Use an application like the **Raspberry Pi Imager** to flash Raspberry Pi OS (or any other OS of your choice) to the eMMC.

At this point, if you are not planning to do any modifications to the contents  of the `boot` volume, you could disconnect the IO Board **USB Slave port** connection, eject the `boot` volume if it is still mounted, disconnect power from the board, then remove the **J2 jumber** from eMMC Boot Disable pins.

Aterwards, plug the power back in, and the **CM4** should now boot off it's new freshly-flashed Operating System from the eMMC storage!
