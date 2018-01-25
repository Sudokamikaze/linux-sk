linux-sk
=============
 Fast and pure, many options, stable as rock. For archlinux, based on -ZEN sources

 Also this kernel support modprobed-db feature, [read](https://wiki.archlinux.org/index.php/Modprobed-db) about it on archwiki

 My performance [patch](https://github.com/Sudokamikaze/makefile_patchset) source in my repositories.

* Current -ZEN sources version: 4.14.15

Our features
========

* Latest ZEN sources
* Auto re-building modules for our kernel(read about it below)
* LZ4 Compression. Inspired by @petter3k
* HZ=250
* Modprobed-db support
* Reiser4 support
* GCC optimized
* Have custom patches(Read about it below)
* NUMA disabled by default(Also you can easily enable back)
* Disabled some unneeded debugging
* Have native & easy support for custom toolchains

Our toolchain
=======

For now, we have few prebuilded toolchains for IVYBRIDGE/SANDYBRIDGE/HASWELL target

If you want to compile kernel with our toolchain use this [repo](https://github.com/QUVNTNM-TC/DESKTOP-TC)

If you want to support your cpu and publish your builded toolchain at our official repo. Just email me to my telegram. I'll give you a config for toolchain building

Our modules & hooks
=======

We support re-building modules(acpi_call & bbswitch) after updating kernel

How-to setup:

```
git clone https://github.com/Sudokamikaze/sk-modules.git && cd sk_modules
./install.sh
```

After this you can call this script or it will called automatically when you update kernel

Our patches
========

Includes high level GCC optimizations. Inspired & Ported from my android kernels

It have graphite and some -O3-related flags

BTW, this patch doesn't apply on kernel modules, because building were failed

##### Guidelines

```
use_optimization This variable expects these definitions:
```

```

If you have AMD cpu, just define "yes" in variable

"yes" If you want to get native optimizations for your cpu
"no" Disable any GCC cpu optimization

And cpu codenames below:
# "ATOM"
# "CORE2"
# "NEHALEM"
# "WESTMERE"
# "SILVERMONT"
# "SANDYBRIDGE"
# "IVYBRIDGE"
# "HASWELL"
# "BROADWELL"
# "SKYLAKE"

```

How to use custom toolchain? Firstfully you need to build it, i'd recommend to use Crosstool-NG. After building you need to enable it in PKGBUILD:

Firstfully type "yes" in this variable

```
use_customtc="no"
```

After this you need to specify path to it

```
pathto=""
# Write path to your compiled toolchan, please, write like in this example
# example: "/home/haze/x-tools/x86_64-pc-linux-gnu/bin/x86_64-pc-linux-gnu-"
```
