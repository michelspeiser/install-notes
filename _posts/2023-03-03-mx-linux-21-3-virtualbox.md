---
title: "Set up an MX Linux 21.3 VM and install R 4.2 with tidyverse"
date: 2023-03-03
---

These pages contain installation and configuration steps taken. 

They should not be regarded as a manual; rather, they serve as an external memory to me, so I can retrace my steps when something breaks.

If you find some use in these contents, so much the better!

# Set up an MX Linux VM in VirtualBox

Create a VM from the ISO image of MX Linux 21.3, using the XFCE flavor.

The displayed image is very small, even with 2560x1600 resolution. Can improve this with:

Settings Manager -> Appearance -> Settings -> Window Scaling (2x)

However this messes up the tiny title bar at the top, whose font is suddenly too big. 
Apparently the title bar can't be made bigger, so we make the font smaller.

Settings Manager -> Window Manager -> Style -> Title font (8)

Install the additions:
```
# menu -> Devices > Insert Guest Additions CD Image
sudo ./VBoxLinuxAdditions.run
```

Allow Shift-Insert (Fn-Shift-F9):
```
xmodmap -e "keycode 75 = Insert Insert Insert" 
echo "keycode 75 = Insert Insert Insert" >> ~/.Xmodmap
```

Note: to resize a window, use Alt + right-click.


## Install R 4.2 and tidyverse

### Add R 

MX Linux 21.3 is [based](https://mxlinux.org/blog/mx-21-3-wildflower-released/) 
on Debian 11.6 "bullseye", which comes with R 4.0.4 (released Feb 2021).

So we install R 4.2 by using another repository.

Create file `/etc/apt/sources.list.d/r-project.list` and insert the line:
```
deb http://cloud.r-project.org/bin/linux/debian bullseye-cran40/
```

Then add the key and update:
```
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-key '95C0FAF38DB3CCAD0C080A7BDC78B2DDEABC47B7'
sudo apt update
```

Now, in Synaptic Package Manager, under "Gnu R statistical system", 
add all packages that have a `~bullseyeran` version.
This gets us R 4.2.

Now, for tidyverse, add some dependencies (may not all be necessary):
```
sudo apt-get install g++ build-essential 
sudo apt install libcurl4-openssl-dev libssl-dev libxml2-dev 
sudo apt install libfontconfig1-dev
sudo apt install pkg-config
sudo apt install libfribidi-dev
sudo apt install libharfbuzz-dev
sudo apt install libtiff-dev
sudo apt install libfreetype6-dev libpng-dev libtiff5-dev libjpeg-dev
```

Then inside an R session:
```
install.packages("tidyverse")
```

