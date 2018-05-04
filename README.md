linux-sk - ZEN based kernel with additional features which described bellow
==========
    
 Kernel support modprobed-db feature, [read](https://wiki.archlinux.org/index.php/Modprobed-db) about it on archwiki

 My performance [patch](https://github.com/Sudokamikaze/makefile_patchset) source in my repositories.

* Current -ZEN sources version: 4.16.7

Table of contents
-----------------

- [Features](#our-features)
- [About Toolchain](#our-toolchain)
- [About patches](#our-patches)
- [Modules & Hooks](#our-modules--hooks)
- [FAQ](#faq)

Our features
========

* Latest ZEN sources
* Auto re-building modules for our kernel
* LZ4 Compression. Inspired by @petter3k
* Modprobed-db support
* GCC optimized
* Reiser4 filesystem support
* Tweaked with `-O3` flags
* NUMA disabled by default(But you can easily enable back)
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

FAQ
========

```
use_optimization This variable expects these definitions:
```

```
Enabled - Automaticaly detects your CPU and optimize kernel for it
Disabled - Disables this featur
```
