# Setup Contiki Toolchain in Arch Linux

## Getting started
Setting up the Contiki toolchain in Arch Linux is a *2 step process*. Step 1 is to *clone the Contiki repository* from GitHub <https://github.com/contiki-os/contiki>). Step 2 is to install the packages that are required to run and build Contiki's examples/simulations. The packages that Contiki depends on are 32-bit, although 64-bit processors are backwards compatible with 32-bit, thus running 32-bit software is possible on 64-bit Arch Linux. All except
one of Contiki's dependencies can be installed from Arch Linux's official repositories (core, extra, community, multilib). The *one package...* (see bellow).

## gcc-msp430 a.k.a. mspgcc-ti (equivalent package from the arch user repository)
In order to use MSP430 based platforms you need to install the gcc-msp430 package **VERSION <= 4.7.x** (In this guide 4.7.2 was used) which is available in *Ubuntu*. Arch has its equivalent, *package mspgcc-ti* from the aur repository, which is essentially the installer which Texas Instruments has in their website (you can verify this by comparing the _installer=msp430-gcc-full-linux-installer-x.x.x.xx.run in the PKGBUILD with the version that Texas Instruments offer in their *website* <http://software-dl.ti.com/msp430/msp430_public_sw/mcu/msp430/MSPGCC/latest/index_FDS.html>). The latest version of msp430 **doesn't work** with Contiki. **This** repository provides the mspgcc-4.7.2 precompiled.

## Step 1
Clone the Contiki GitHub repository to a local directory:

```sh
git clone 'url-contiki-repository'
```

Once cloned, run:

```sh
cd contiki/
git submodule update --init
```

The above command will check out the *submodules* that are included in the contiki repository (e.g. mspsim).

## Step 2
Install the required packages. First though, the **multilib** repository must be enabled in `/etc/pacman.conf`. To do this uncomment the lines:

```
[multilib]
Include = /etc/pacman.d/mirrorlist
```

Mirrors can be out of sync so *pacman* needs to be forced to refresh the package lists. The following command forces pacman to refresh all package lists even if they are considered to be up to date:

```sh
pacman -Syy
```

Now, install the packages:

```
pacman -S libmpc zlib apache-ant jre-openjdk jdk8-openjdk arm-none-eabi-gcc arm-none-eabi-gdb lib32-gcc-libs lib32-glibc lib32-libstdc++5 lib32-zlib lib32-fakeroot
```

Finally "install" the mspgcc-4.7.2 package:

```sh
cd msp430/
./msp430-installer.sh
```

An alternative would be to create *symbolic links* from the contents of `msp430/` to `/usr/local/`. A script for this purpose is provided in **this** repository.

## DONE
Try openning a simulation in *cooja* to test if it compiles.
