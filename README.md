linux-sk
=============
 Fast and pure, many options, stable as rock. For archlinux

 Also this kernel support modprobed-db feature, [read](https://wiki.archlinux.org/index.php/Modprobed-db) about it on archwiki

 My performance [patch](https://github.com/Sudokamikaze/makefile_patchset) source in my repositories.



##### Guidelines

```
use_optimization This variable expects these definitions:
```

```
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
