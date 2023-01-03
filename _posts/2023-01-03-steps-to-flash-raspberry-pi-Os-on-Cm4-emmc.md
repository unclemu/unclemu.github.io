---
published: true
---

Today we are going to play with Raspberry Pi Compute Module 4, by tring to flash its **eMMC** storage with Rasbian OS instead of MicroSD Card.

This post shows how to mount eMMC storage on another computer (MAC in my case), and then how to flash a new Operating System image to it.

## IO Board mount preparation 
First of all you need the [Raspberry Pi Offical IO Board](https://www.raspberrypi.com/products/compute-module-4-io-board/ "IO Board").
Before you can put the eMMC storage into USB mass storage mode, you have to put a **jumper** over the first set of pins on the board--the jumper labeled **"Fit jumper to disable eMMC Boot"**.

Then, plug a USB cable from your computer (in my case, Mac--but it could be a Windows or Linux too) into the **USB Slave** micro USB port on the IO Board, and plug in power.

The board will power on, and you will see the red **D1 LED** turn on, but the Compute Module won't boot. The eMMMC should now be ready for the next step.

## Using usbboot script to mount the eMMC storage
The next step is to download the Raspberry Pi usbboot repository, install a required USB library on your computer, and build the `rpiboot` executable, which you will use to mount the storage on your computer. I did all of this in the Terminal application on my Mac

First, be sure that `libusb` library has been installed:
- On my Mac, I used homebrew (package manager), to run this command:
``` 
brew install pkgconfig libusb
```
- On Linux, use terminal to run: 
``` 
sudo apt install libusb-1.0-0-dev 
```

Second, clone the usbboot repository to your computer:
``` 
$ get clone --depth=1 https://github.com/raspberrypi/usbboot 
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
You might also notice the D2 LED lighting up; that indicate there is disk read/write activity on the eMMC.
