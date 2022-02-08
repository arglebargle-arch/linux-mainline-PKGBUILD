#
# Maintainer: Arglebargle <arglebargle@arglebargle.dev>
#
# Based on the linux and linux-mainline packages by:
# Contributor: Mikael Eriksson <mikael_eriksson@miffe.org>
# Contributor: Jan Alexander Steffens (heftig) <jan.steffens@gmail.com>
# Contributor: Tobias Powalowski <tpowa@archlinux.org>
# Contributor: Thomas Baechler <thomas@archlinux.org>
#
# shellcheck disable=SC2034,SC2164

## Kernel build options, use the environment variables below to control
##
## Example usage: `_compiler=clang _microarchitecture=15 _O3=y makepkg ...`
##            or  `makepkg <flags> -- _compiler=clang _O3=y ...`

# '_compiler': Support build via default toolchain or LLVM/Clang
#              default: 'gcc' and standard toolchain, set _compiler='clang' for LLVM/Clang and thin LTO
case "${_compiler,,}" in
  "clang" | "gcc") _compiler=${_compiler,,} ;; # tolower, simplifes later checks
                *) _compiler=gcc            ;; # default to GCC
esac

# '_microarchitecture': Optional compiler uArch optimizations, see choose-gcc-optimization.sh for values
#                       default: x86-64-v3, requires gcc >= 11.0
_microarchitecture=${_microarchitecture:-'93'}

# Other uArch values:
#   Zen2 -> 14, Zen3 -> 15, Skylake & Comet Lake -> 38, Tiger Lake -> 44
#   Intel Native -> 98, AMD Native -> 99
#
# Generic x86-64-v3 strongly recommended if you intend to use one kernel package on multiple machines

# '_O3':  Optional -O3 compiler optimizations
#         default: no -O3 optimization, set _O3=<anything> to enable
_O3=${_O3:+'y'}

_pkgbase=linux-mainline
pkgbase=linux-mainline-amd-s0ix
_tag=v5.17-rc3-s0ix
pkgver=5.17rc3
pkgrel=1
pkgdesc="Linux Mainline"
arch=(x86_64)
url="https://kernel.org/"
license=(GPL2)
makedepends=(
  bc kmod libelf pahole cpio perl tar xz
  # NOTE: -docs package is disabled, thus we don't need these deps.
  xmlto # python-sphinx python-sphinx_rtd_theme graphviz imagemagick
  git "gcc>=11.0"
)
if [ "$_compiler" = "clang" ]; then
  pkgver="${pkgver}+clang"
  makedepends+=(clang llvm lld python)
  _LLVM=1
fi
options=('!strip' '!ccache')
_srcname=linux-stable-s0ix
source=(
  "$_srcname::git+https://gitlab.com/smbruce/linux-stable-s0ix.git#tag=$_tag"
  config

  # NOTE: We pull from a stable kernel mirror with all upstream s0ix work included

  # graysky's compiler uarch optimization patch, script courtesy of the `linux-xanmod` AUR package
  "choose-gcc-optimization.sh"
  "more-uarches-for-kernel-5.15+9c9c7e.patch"::"https://raw.githubusercontent.com/graysky2/kernel_compiler_patch/9c9c7e817dd2718566ec95f7742b162ab125316f/more-uarches-for-kernel-5.15%2B.patch"

  # compiler optimization patches from Xanmod; allow -O3 for all architectures, safer -O3
  "Makefile-Turn-off-loop-vectorization-for-GCC-O3.patch"
  "init-Kconfig-Enable-O3-KBUILD_CFLAGS-optimization.patch"

  # Arch: optionally disallow unprivileged USER_NS clone
  "ZEN-Add-sysctl-and-CONFIG-to-disallow-unpriv-USER_NS.patch"

  # squelch overly zealous wifi regdomain not set warnings; this removes spurious module "crashes" from the klog when using iwd
  "cfg80211-dont-WARN-if-a-self-managed-device.patch"

  # patch from Chromium developers; more accurately report battery state changes
  "acpi-battery-Always-read-fresh-battery-state-on-update.patch"

  # NOTE: Optional features; feel free to comment these out (make changes to package-config script as needed)

  # Google's TCP BBRv2
  "squashed-TCP-BBR-v2-for-5.16.y.patch"

  # Multigenerational LRU v6
  "squashed-mm-multigenerational-lru-v6-for-5.16.patch"

  # 5.17: AMD pstate cpufreq driver
  # 5.17: TCP performance improvements
  # 5.17: TCP csum optimization
  # 5.17: ASUS ROG custom fan curves

  # 5.??: Parallelize x86_64 CPU bringup (v4 patchset) for faster bootup
  # see: https://lore.kernel.org/lkml/20220201205328.123066-1-dwmw2@infradead.org/
  "Parallel-boot-v4-on-5.17-rc2.patch"

  # reduce hid-asus object size by consolidating calls
  "HID-asus-Reduce-object-size-by-consolidating-calls.patch"

  # cherry-picked mediatek mt7921 bt/wifi support from -next and patchwork
  "mt76-mt7921-enable-VO-tx-aggregation.patch"
  "1-2-Bluetooth-btusb-Add-Mediatek-MT7921-support-for-Foxconn.patch"
  "2-2-Bluetooth-btusb-Add-Mediatek-MT7921-support-for-IMC-Network.patch"
  "Bluetooth-btusb-Add-support-for-IMC-Networks-Mediatek-Chip.patch"
  "Bluetooth-btusb-Add-support-for-Foxconn-Mediatek-Chip.patch"
  "Bluetooth-btusb-Add-support-for-IMC-Networks-Mediatek-Chip-MT7921.patch"

  # WARNING: hotfix for mt76 reboot issues; only use this when building mt76 as a module
  "mt76-mt7921e-fix-possible-probe-failure-after-reboot.patch"
)
validpgpkeys=(
  'ABAF11C65A2970B130ABE3C479BE3E4300411886'  # Linus Torvalds
  '647F28654894E3BD457199BE38DBBDC86092693E'  # Greg Kroah-Hartman
  'A2FF3A36AAA56654109064AB19802F8B0D70FC30'  # Jan Alexander Steffens (heftig)
)
sha256sums=('SKIP'
            '7cbba374356a189faac71001c5344ce8f02434684b1ce1accefc0cc4bd6718e5'
            '5b8eddb90671f3e8469a023b7ed0d3c5a9521f662affa1d541063e273b64dba8'
            '380bcf40cc8396e97bd1d7f2577ab2ace51885858d3f155b1fb2dd5469efd00d'
            '4ed47b049cfc42289897e9f6dc85b548b712ef77bda6f186125f464cfe8aed91'
            '1e9b3c3fccfaba790335b9a83a87e129d66bfba850548f23865b76ea235b8558'
            '743001364eb7bf9ee208e60b74b7c68b46c4d03feae26dfcb8f7581d3bf14271'
            '3d8961438b5c8110588ff0b881d472fc71a4304d306808d78a4055a4150f351e'
            'f7a4bf6293912bfc4a20743e58a5a266be8c4dbe3c1862d196d3a3b45f2f7c90'
            '70f4c71b7993d80274c29a8076f59b4c15dbd8ce6d7b1334006bf9b82316b780'
            'bd61c315edca3b3ffd99f4830864fcfa6793be195369629506a730d71b8c858e'
            '67b7c5d754698b3f07ae49ed65a11d220b34a7940cebfdb79ae6928d852802a0'
            '544464bf0807b324120767d55867f03014a9fda4e1804768ca341be902d7ade4'
            '1ce9fd988201c4d2e48794c58acda5b768ec0fea1d29555e99d35cd2712281e4'
            '236cdadf0b1472945c0d7570caeed7b95929aabed6872319c9d0969a819689e9'
            'cc2aa580d69801aa1afb0d72ecf094fe13c797363d3d5928c868d3a389910b7b'
            '292a7e32b248c7eee6e2f5407d609d03d985f367d329adb02b9d6dba1f85b44c'
            '7dbfdd120bc155cad1879579cb9dd1185eb5e37078c8c93fef604a275a163812'
            '1444af2e125080934c67b6adb4561fd354a72ce47d3de393b24f53832ee492ac'
            '63ebf908ba2a66865a94e3a4af579d41ec15573522d3ebb07bf8ded3bc57e833')

export KBUILD_BUILD_HOST=archlinux
export KBUILD_BUILD_USER=$pkgbase
export KBUILD_BUILD_TIMESTAMP="$(date -Ru${SOURCE_DATE_EPOCH:+d @$SOURCE_DATE_EPOCH})"

prepare() {
  cd $_srcname

  echo "Setting version..."
  scripts/setlocalversion --save-scmversion
  # HACK: clear the .scmversion file so that our patches don't dirty the kernel version
  echo "" > .scmversion
  echo "-$pkgrel" > localversion.99-pkgrel
  echo "${pkgbase#linux}" > localversion.20-pkgname

  local src
  for src in "${source[@]}"; do
    src="${src%%::*}"
    src="${src##*/}"
    [[ "$src" =~ .*(patch|diff)$ ]] || continue
    echo -e "  ${BLUE}-> ${ALL_OFF}${BOLD}Applying patch: ${ALL_OFF}$src"
    patch -Np1 < "../$src"
  done

  echo "Setting config..."
  cp ../config .config

  # Apply LTO_CLANG_THIN configuration
  if [[ "$_compiler" == "clang" ]]; then
    msg2 "Applying Clang ThinLTO configuration..."
    scripts/config  --module  CONFIG_LTO \
                    --enable  CONFIG_LTO_CLANG \
                    --enable  CONFIG_HAS_LTO_CLANG \
                    --disable CONFIG_LTO_NONE \
                    --disable CONFIG_LTO_CLANG_FULL \
                    --enable  CONFIG_LTO_CLANG_THIN \
                    --disable CONFIG_INIT_STACK_ALL_PATTERN \
                    --disable CONFIG_INIT_STACK_ALL_ZERO \
                    --disable CONFIG_REGULATOR_DA903X 
                    # this module is incompatible with clang, disable to avoid a warning
  fi

  # enable -O3 build config
  if [[ "$_O3" == "y" ]]; then
    msg2 "Enabling -O3 optimizations ..."
    scripts/config --disable CONFIG_CC_OPTIMIZE_FOR_PERFORMANCE \
                   --enable CONFIG_CC_OPTIMIZE_FOR_PERFORMANCE_O3
  fi

  # apply package kernel config customizations
  if [[ -s ${startdir}/package-config-customizations ]]; then
    msg2 "Applying package config customizations..."
    bash -x "${startdir}/package-config-customizations"
  fi

  # apply any user config customizations
  if [[ -s ${startdir}/myconfig ]]; then
    msg2 "Applying user supplied config customizations..."
    bash -x "${startdir}/myconfig"
  fi

  echo "Finalizing kernel config..."
  make LLVM=$_LLVM LLVM_IAS=$_LLVM olddefconfig

  # Apply selected microarchitecture optimization; run *after* make olddefconfig so our new uarch macros exist
  sh "${srcdir}/choose-gcc-optimization.sh" "$_microarchitecture"

  make -s kernelrelease > version
  echo "Prepared $pkgbase version $(<version)"

  # retain config for re-use or inspection
  cat .config > "${startdir}/config.last"
}

build() {
  cd $_srcname
  make LLVM=$_LLVM LLVM_IAS=$_LLVM all
  #make htmldocs
}

_package() {
  pkgdesc="The $pkgdesc kernel and modules"
  depends=(coreutils kmod initramfs)
  optdepends=('crda: to set the correct wireless channels of your country'
              'linux-firmware: firmware images needed for some devices')
  provides=(VIRTUALBOX-GUEST-MODULES WIREGUARD-MODULE ASUS-WMI-FAN-CONTROL linux-rog)

  cd $_srcname
  local kernver="$(<version)"
  local modulesdir="$pkgdir/usr/lib/modules/$kernver"

  echo "Installing boot image..."
  # systemd expects to find the kernel here to allow hibernation
  # https://github.com/systemd/systemd/commit/edda44605f06a41fb86b7ab8128dcf99161d2344
  install -Dm644 "$(make -s image_name)" "$modulesdir/vmlinuz"

  # Used by mkinitcpio to name the kernel
  echo "$pkgbase" | install -Dm644 /dev/stdin "$modulesdir/pkgbase"

  echo "Installing modules..."
  make INSTALL_MOD_PATH="$pkgdir/usr" INSTALL_MOD_STRIP=1 modules_install

  # remove build and source links
  rm "$modulesdir"/{source,build}
}

_package-headers() {
  pkgdesc="Headers and scripts for building modules for the $pkgdesc kernel"
  depends=(pahole)

  cd $_srcname
  local builddir="$pkgdir/usr/lib/modules/$(<version)/build"

  echo "Installing build files..."
  install -Dt "$builddir" -m644 .config Makefile Module.symvers System.map \
    localversion.* version vmlinux
  install -Dt "$builddir/kernel" -m644 kernel/Makefile
  install -Dt "$builddir/arch/x86" -m644 arch/x86/Makefile
  cp -t "$builddir" -a scripts

  # required when STACK_VALIDATION is enabled
  install -Dt "$builddir/tools/objtool" tools/objtool/objtool

  # required when DEBUG_INFO_BTF_MODULES is enabled
  [[ -x tools/bpf/resolve_btfids/resolve_btfids ]] &&
    install -Dt "$builddir/tools/bpf/resolve_btfids" tools/bpf/resolve_btfids/resolve_btfids

  # add xfs and shmem for aufs building
  mkdir -p "$builddir"/{fs/xfs,mm}

  echo "Installing headers..."
  cp -t "$builddir" -a include
  cp -t "$builddir/arch/x86" -a arch/x86/include
  install -Dt "$builddir/arch/x86/kernel" -m644 arch/x86/kernel/asm-offsets.s

  install -Dt "$builddir/drivers/md" -m644 drivers/md/*.h
  install -Dt "$builddir/net/mac80211" -m644 net/mac80211/*.h

  # https://bugs.archlinux.org/task/13146
  install -Dt "$builddir/drivers/media/i2c" -m644 drivers/media/i2c/msp3400-driver.h

  # https://bugs.archlinux.org/task/20402
  install -Dt "$builddir/drivers/media/usb/dvb-usb" -m644 drivers/media/usb/dvb-usb/*.h
  install -Dt "$builddir/drivers/media/dvb-frontends" -m644 drivers/media/dvb-frontends/*.h
  install -Dt "$builddir/drivers/media/tuners" -m644 drivers/media/tuners/*.h

  # https://bugs.archlinux.org/task/71392
  install -Dt "$builddir/drivers/iio/common/hid-sensors" -m644 drivers/iio/common/hid-sensors/*.h

  echo "Installing KConfig files..."
  find . -name 'Kconfig*' -exec install -Dm644 {} "$builddir/{}" \;

  echo "Removing unneeded architectures..."
  local arch
  for arch in "$builddir"/arch/*/; do
    [[ $arch = */x86/ ]] && continue
    echo "Removing $(basename "$arch")"
    rm -r "$arch"
  done

  echo "Removing documentation..."
  rm -r "$builddir/Documentation"

  echo "Removing broken symlinks..."
  find -L "$builddir" -type l -printf 'Removing %P\n' -delete

  echo "Removing loose objects..."
  find "$builddir" -type f -name '*.o' -printf 'Removing %P\n' -delete

  echo "Stripping build tools..."
  local file
  while read -rd '' file; do
    case "$(file -bi "$file")" in
      application/x-sharedlib\;*)      # Libraries (.so)
        strip -v "$STRIP_SHARED" "$file" ;;
      application/x-archive\;*)        # Libraries (.a)
        strip -v "$STRIP_STATIC" "$file" ;;
      application/x-executable\;*)     # Binaries
        strip -v "$STRIP_BINARIES" "$file" ;;
      application/x-pie-executable\;*) # Relocatable binaries
        strip -v "$STRIP_SHARED" "$file" ;;
    esac
  done < <(find "$builddir" -type f -perm -u+x ! -name vmlinux -print0)

  echo "Stripping vmlinux..."
  strip -v "$STRIP_STATIC" "$builddir/vmlinux"

  echo "Adding symlink..."
  mkdir -p "$pkgdir/usr/src"
  ln -sr "$builddir" "$pkgdir/usr/src/$pkgbase"
}

#_package-docs() {
#  pkgdesc="Documentation for the $pkgdesc kernel"
#
#  cd $_srcname
#  local builddir="$pkgdir/usr/lib/modules/$(<version)/build"
#
#  echo "Installing documentation..."
#  local src dst
#  while read -rd '' src; do
#    dst="${src#Documentation/}"
#    dst="$builddir/Documentation/${dst#output/}"
#    install -Dm644 "$src" "$dst"
#  done < <(find Documentation -name '.*' -prune -o ! -type d -print0)
#
#  echo "Adding symlink..."
#  mkdir -p "$pkgdir/usr/share/doc"
#  ln -sr "$builddir/Documentation" "$pkgdir/usr/share/doc/$pkgbase"
#}

#pkgname=("$pkgbase" "$pkgbase-headers" "$pkgbase-docs")
pkgname=("$pkgbase" "$pkgbase-headers")
for _p in "${pkgname[@]}"; do
  eval "package_$_p() {
    $(declare -f "_package${_p#"$pkgbase"}")
    _package${_p#"$pkgbase"}
  }"
done

# vim:set ts=8 sts=2 sw=2 et:
