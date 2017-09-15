###########################################################################################################
#                                         Build Options
###########################################################################################################

use_optimization="IVYBRIDGE"
# "yes": Use native gcc optimizations
# "no": Disable any optimization
# or define your cpu family in _use_optimization. Supported codenames listed bellow
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
# If you have AMD cpu - just define "yes"

use_modprobed="no"
# "yes" Use modprobed-db to load modules

use_customtc="no"
# "yes" Enable variable below

pathto=""
# Write path to your compiled toolchan, please, write like in this example
# example: "/home/haze/x-tools/x86_64-pc-linux-gnu/bin/x86_64-pc-linux-gnu-" 

use_reiser="no"
# Currently not implemented

disable_numa="yes"
# If your pc have more then 1 CPU(not thread or core) define here "no"

hard_optimization="yes"
# Maybe unstable, define "no" if you had any problems with it

###########################################################################################################

pkgbase=linux-sk
_srcname=linux-4.13
_zenpatch=zen-4.13.2-6b7b0a3850fe6190ad8b660911d7984403ee7345.diff
pkgver=4.13.2
pkgrel=1
arch=('i686' 'x86_64')
url="https://github.com/zen-kernel/zen-kernel"
license=('GPL2')
makedepends=('xmlto' 'kmod' 'inetutils' 'bc' 'libelf')
options=('!strip')
source=("https://www.kernel.org/pub/linux/kernel/v4.x/${_srcname}.tar.xz"
        "https://www.kernel.org/pub/linux/kernel/v4.x/${_srcname}.tar.sign"
        "https://www.kernel.org/pub/linux/kernel/v4.x/patch-${pkgver}.xz"
        "https://www.kernel.org/pub/linux/kernel/v4.x/patch-${pkgver}.sign"
        "https://pkgbuild.com/~heftig/zen-patches/${_zenpatch}.xz"
        "https://pkgbuild.com/~heftig/zen-patches/${_zenpatch}.sign"
        "https://raw.githubusercontent.com/Sudokamikaze/makefile_patchset/master/4.13.patch"
        'config.x86_64'
        # pacman hook for initramfs regeneration
        '90-linux.hook'
        # standard config files for mkinitcpio ramdisk
        'linux.preset')

sha256sums=('2db3d6066c3ad93eb25b973a3d2951e022a7e975ee2fa7cbe5bddf84d9a49a2c'
            'SKIP'
            'dfb5753b89580494a54d1021d14f1fa91b54d4ab9bf6175fc8cf8cb8c41d8e3f'
            'SKIP'
            '6fbfd8bb699539973da3bac817efc75509910ad66d404d63c41a862323c4cd63'
            'SKIP'
            '81ed89cdab9ae579e9cb1602893fd618ddc2d90bc0460e262d3e5934bd440114'
            '3b9417070ed7716e2e55897e348175e2c2fe935b888a7050a57bdb906cd68e6b'
            '834bd254b56ab71d73f59b3221f056c72f559553c04718e350ab2a3e2991afe0'
            'ad6344badc91ad0630caacde83f7f9b97276f80d26a20619a87952be65492c65')

_kernelname=${pkgbase#linux}

prepare() {
  cd ${_srcname}

  # add upstream patch
  patch -p1 -i ../patch-${pkgver}

  # security patches

  # add latest fixes from stable queue, if needed
  # http://git.kernel.org/?p=linux/kernel/git/stable/stable-queue.git

  # add zen patch
  patch -p1 -i ../${_zenpatch}

  cp -Tf ../config.x86_64 .config

  # Our tweaks begin here

  case "$use_optimization" in
  yes) sed -i -e 's/# CONFIG_MNATIVE is not set/CONFIG_MNATIVE=y/' ./.config ;;
  no) sed -i -e 's/# CONFIG_GENERIC_CPU is not set/CONFIG_GENERIC_CPU=y/' ./.config ;;
  ATOM) sed -i -e 's/# CONFIG_MATOM is not set/CONFIG_MATOM=y/' ./.config ;;
  CORE2) sed -i -e 's/# CONFIG_MCORE2 is not set/CONFIG_MCORE2=y/' ./.config ;;
  NEHALEM) sed -i -e 's/# CONFIG_MNEHALEM is not set/CONFIG_MNEHALEM=y/' ./.config ;;
  WESTMERE) sed -i -e 's/# CONFIG_MWESTMERE is not set/CONFIG_MWESTMERE=y/' ./.config ;;
  SANDYBRIDGE) sed -i -e 's/# CONFIG_MSANDYBRIDGE is not set/CONFIG_MSANDYBRIDGE=y/' ./.config ;;
  IVYBRIDGE) sed -i -e 's/# CONFIG_MIVYBRIDGE is not set/CONFIG_MIVYBRIDGE=y/' ./.config ;;
  HASWELL) sed -i -e 's/# CONFIG_MHASWELL is not set/CONFIG_MHASWELL=y/' ./.config ;;
  BROADWELL) sed -i -e 's/# CONFIG_MBROADWELL is not set/CONFIG_MBROADWELL=y/' ./.config ;;
  SKYLAKE) sed -i -e 's/# CONFIG_MSKYLAKE is not set/CONFIG_MSKYLAKE=y/' ./.config ;;
  *) echo "Correct your typo in variable." && exit 1 ;;
  esac

  # disable NUMA since 99.9% of users do not have multiple CPUs but do have multiple cores in one CPU
  # see, https://bugs.archlinux.org/task/31187
  if [ -n "$disable_numa" == "yes" ]; then
     sed -i -e 's/CONFIG_NUMA=y/# CONFIG_NUMA is not set/' \
      -i -e '/CONFIG_AMD_NUMA=y/d' \
      -i -e '/CONFIG_X86_64_ACPI_NUMA=y/d' \
      -i -e '/CONFIG_NODES_SPAN_OTHER_NODES=y/d' \
      -i -e '/# CONFIG_NUMA_EMU is not set/d' \
      -i -e '/CONFIG_NODES_SHIFT=6/d' \
      -i -e '/CONFIG_NEED_MULTIPLE_NODES=y/d' \
      -i -e '/CONFIG_USE_PERCPU_NUMA_NODE_ID=y/d' \
      -i -e '/CONFIG_ACPI_NUMA=y/d' ./.config
  fi

  # Enable hard optimization in kernel
  if [ "$hard_optimization" == "yes" ]; then
    patch -p1 -i "${srcdir}/4.13.patch"
  fi

  # Enable reiser4
  if [ "$use_reiser" == "yes" ]; then
echo "I implement it lately..."
#    patch -p1 -i "${srcdir}/reiser.patch"
  fi

  if [ "${_kernelname}" != "" ]; then
    sed -i "s|CONFIG_LOCALVERSION=.*|CONFIG_LOCALVERSION=\"${_kernelname}\"|g" ./.config
    sed -i "s|CONFIG_LOCALVERSION_AUTO=.*|CONFIG_LOCALVERSION_AUTO=n|" ./.config
  fi

  # set extraversion to pkgrel
  sed -ri "s|^(EXTRAVERSION =).*|\1 -${pkgrel}|" Makefile

  # don't run depmod on 'make install'. We'll do this ourselves in packaging
  sed -i '2iexit 0' scripts/depmod.sh

  # get kernel version
  make prepare

if [ "$use_modprobed" == "yes" ]; then
  make LSMOD=$HOME/.config/modprobed.db localmodconfig  
fi

  # load configuration
  # Configure the kernel. Replace the line below with one of your choice.
  #make menuconfig # CLI menu for configuration
  #make nconfig # new CLI menu for configuration
  #make xconfig # X-based configuration
  #make oldconfig # using old config from previous kernel version
  # ... or manually edit .config

  # rewrite configuration
  yes "" | make config >/dev/null
}

build() {
  cd ${_srcname}
 
  if [ "$use_customtc" == "yes" ]; then
  export CROSS_COMPILE="$pathto"
  fi

  make ${MAKEFLAGS} LOCALVERSION= bzImage modules
}

_package() {
  pkgdesc="The ${pkgbase/linux/Linux} kernel and modules"
  [ "${pkgbase}" = "linux" ] && groups=('base')
  depends=('coreutils' 'linux-firmware' 'kmod' 'mkinitcpio>=0.7')
  optdepends=('crda: to set the correct wireless channels of your country')
  backup=("etc/mkinitcpio.d/${pkgbase}.preset")
  install=linux.install

  cd ${_srcname}

  KARCH=x86

  # get kernel version
  _kernver="$(make LOCALVERSION= kernelrelease)"
  _basekernel=${_kernver%%-*}
  _basekernel=${_basekernel%.*}

  mkdir -p "${pkgdir}"/{lib/modules,lib/firmware,boot}
  make LOCALVERSION= INSTALL_MOD_PATH="${pkgdir}" modules_install
  cp arch/$KARCH/boot/bzImage "${pkgdir}/boot/vmlinuz-${pkgbase}"

  # set correct depmod command for install
  sed -e "s|%PKGBASE%|${pkgbase}|g;s|%KERNVER%|${_kernver}|g" \
    "${startdir}/${install}" > "${startdir}/${install}.pkg"
  true && install=${install}.pkg

  # install mkinitcpio preset file for kernel
  sed "s|%PKGBASE%|${pkgbase}|g" ../linux.preset |
    install -Dm644 /dev/stdin "${pkgdir}/etc/mkinitcpio.d/${pkgbase}.preset"

  # install pacman hook for initramfs regeneration
  sed "s|%PKGBASE%|${pkgbase}|g" ../90-linux.hook |
    install -Dm644 /dev/stdin "${pkgdir}/usr/share/libalpm/hooks/90-${pkgbase}.hook"

  # remove build and source links
  rm "${pkgdir}"/lib/modules/${_kernver}/{source,build}

  # remove the firmware
  rm -r "${pkgdir}/lib/firmware"

  # make room for external modules
  ln -s "../extramodules-${_basekernel}${_kernelname:--ARCH}" "${pkgdir}/lib/modules/${_kernver}/extramodules"

  # add real version for building modules and running depmod from post_install/upgrade
  echo "${_kernver}" |
    install -Dm644 /dev/stdin "${pkgdir}/lib/modules/extramodules-${_basekernel}${_kernelname:--ARCH}/version"

  # Now we call depmod...
  depmod -b "${pkgdir}" -F System.map "${_kernver}"

  # move module tree /lib -> /usr/lib
  mv -t "${pkgdir}/usr" "${pkgdir}/lib"

  # add vmlinux
  install -Dm644 vmlinux "${pkgdir}/usr/lib/modules/${_kernver}/build/vmlinux"
}

_package-headers() {
  pkgdesc="Header files and scripts for building modules for ${pkgbase/linux/Linux} kernel"

  cd ${_srcname}
  local _builddir="${pkgdir}/usr/lib/modules/${_kernver}/build"

  install -Dt "${_builddir}" -m644 Makefile .config Module.symvers
  install -Dt "${_builddir}/kernel" -m644 kernel/Makefile

  mkdir "${_builddir}/.tmp_versions"

  cp -t "${_builddir}" -a include scripts

  install -Dt "${_builddir}/arch/${KARCH}" -m644 arch/${KARCH}/Makefile
  install -Dt "${_builddir}/arch/${KARCH}/kernel" -m644 arch/${KARCH}/kernel/asm-offsets.s

  if [[ ${CARCH} = i686 ]]; then
    install -t "${_builddir}/arch/${KARCH}" -m644 arch/${KARCH}/Makefile_32.cpu
  fi

  cp -t "${_builddir}/arch/${KARCH}" -a arch/${KARCH}/include

  install -Dt "${_builddir}/drivers/md" -m644 drivers/md/*.h
  install -Dt "${_builddir}/net/mac80211" -m644 net/mac80211/*.h

  # http://bugs.archlinux.org/task/9912
  install -Dt "${_builddir}/drivers/media/dvb-core" -m644 drivers/media/dvb-core/*.h

  # http://bugs.archlinux.org/task/13146
  install -Dt "${_builddir}/drivers/media/dvb-frontends" -m644 drivers/media/dvb-frontends/lgdt330x.h
  install -Dt "${_builddir}/drivers/media/i2c" -m644 drivers/media/i2c/msp3400-driver.h

  # http://bugs.archlinux.org/task/20402
  install -Dt "${_builddir}/drivers/media/usb/dvb-usb" -m644 drivers/media/usb/dvb-usb/*.h
  install -Dt "${_builddir}/drivers/media/dvb-frontends" -m644 drivers/media/dvb-frontends/*.h
  install -Dt "${_builddir}/drivers/media/tuners" -m644 drivers/media/tuners/*.h

  # add xfs and shmem for aufs building
  mkdir -p "${_builddir}"/{fs/xfs,mm}

  # copy in Kconfig files
  find . -name Kconfig\* -exec install -Dm644 {} "${_builddir}/{}" \;

  # add objtool for external module building and enabled VALIDATION_STACK option
  if [[ -e tools/objtool/objtool ]]; then
    install -Dt "${_builddir}/tools/objtool" tools/objtool/objtool
  fi

  # remove unneeded architectures
  local _arch
  for _arch in "${_builddir}"/arch/*/; do
    if [[ ${_arch} != */${KARCH}/ ]]; then
      rm -r "${_arch}"
    fi
  done

  # remove files already in linux-docs package
  rm -r "${_builddir}/Documentation"

  # Fix permissions
  chmod -R u=rwX,go=rX "${_builddir}"

  # strip scripts directory
  local _binary _strip
  while read -rd '' _binary; do
    case "$(file -bi "${_binary}")" in
      *application/x-sharedlib*)  _strip="${STRIP_SHARED}"   ;; # Libraries (.so)
      *application/x-archive*)    _strip="${STRIP_STATIC}"   ;; # Libraries (.a)
      *application/x-executable*) _strip="${STRIP_BINARIES}" ;; # Binaries
      *) continue ;;
    esac
    /usr/bin/strip ${_strip} "${_binary}"
  done < <(find "${_builddir}/scripts" -type f -perm -u+w -print0 2>/dev/null)
}

_package-docs() {
  pkgdesc="Kernel hackers manual - HTML documentation that comes with the ${pkgbase/linux/Linux} kernel"

  cd ${_srcname}
  local _builddir="${pkgdir}/usr/lib/modules/${_kernver}/build"

  mkdir -p "${_builddir}"
  cp -t "${_builddir}" -a Documentation

  # Fix permissions
  chmod -R u=rwX,go=rX "${_builddir}"
}

pkgname=("${pkgbase}" "${pkgbase}-headers" "${pkgbase}-docs")
for _p in ${pkgname[@]}; do
  eval "package_${_p}() {
    $(declare -f "_package${_p#${pkgbase}}")
    _package${_p#${pkgbase}}
  }"
done

# vim:set ts=8 sts=2 sw=2 et:
