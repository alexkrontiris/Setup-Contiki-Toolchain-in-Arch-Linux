# Setup Contiki Toolchain in Arch Linux

## Getting started
Setting up the Contiki toolchain in Arch Linux is a *2 step process*. Step 1 is to *clone the Contiki repository* from GitHub <https://github.com/contiki-os/contiki>). Step 2 is to install the packages that are required to run and build Contiki's examples/simulations. The packages that Contiki depends on are 32-bit, although 64-bit processors are backwards compatible with 32-bit, thus running 32-bit software is possible on 64-bit Arch Linux. All except
one of Contiki's dependencies can be installed from Arch Linux's official repositories (core, extra, community, multilib). The *one package...* (see bellow).

## gcc-msp430 a.k.a. mspgcc-ti (aur package)
In order to use MSP430 based platforms you need to install the gcc-msp430 package **VERSION <= 4.7.x** (version used in this guide: 4.7.2) which is available in *Ubuntu*. Arch has *package mspgcc-ti* from the aur which contains *msp430-elf-gcc*. This package is essentially the GNU toolchain for the Texas Instruments msp430 embedded processors that TI has in its website (you can verify the version by comparing the line:
`_installer=msp430-gcc-full-linux-installer-x.x.x.xx.run`
from the PKGBUILD with the version number that Texas Instruments has in its *website* <http://software-dl.ti.com/msp430/msp430_public_sw/mcu/msp430/MSPGCC/latest/index_FDS.html>). Unfortunately, *mspgcc-ti* **doesn't work** with Contiki. However, you can find in **this** repository the mspgcc-4.7.2 precompiled which works.

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
tar xjf mspgcc-4.7.2-compiled.tar.bz2
sudo cp -r -f msp430/* /usr/local/
```

Verify:
```sh
msp430-gcc --version
```

An alternate solution in case you prefer not to copy the contents of `msp430/` would be to create *symbolic links* from `msp430/*` to `/usr/local/`.

## DONE
Try openning a simulation in *cooja* and see if it runs.

## TROUBLESHOOTING
If *cooja* fails to open the simulation and outputs an error in the console complaining about the size of `.text` file or something similar, download the *contiki.zip* file from **this** repository which contains the contiki directory from Instant Contiki 3.0 and test *cooja* in this version which is the same as the one used in this guide. Don't forget to checkout the submodules (`git submodule update --init`).

## Tips and tricks
Instead of doing `cd /path/to/cooja` and then `ant run` each time you run *cooja*, you could create an *alias* in your `.bashrc` like this: `alias cooja='sh -c "cd /path/to/cooja && ant run"'`.
