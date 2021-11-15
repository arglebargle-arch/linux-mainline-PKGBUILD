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

# Choose between GCC or CLANG compilation (default is GCC)
case "${_compiler,,}" in
  "clang" | "gcc") _compiler=${_compiler,,} ;; # tolower, simplifes later checks
                *) _compiler=gcc            ;; # default to GCC
esac

_pkgbase=linux-mainline
pkgbase=linux-mainline-amd-s0ix
_tag=v5.15.2-s0ix
pkgver=5.15.2
pkgrel=1
pkgdesc="Linux Mainline"
arch=(x86_64)
url="https://kernel.org/"
license=(GPL2)
makedepends=(
  bc kmod libelf pahole cpio perl tar xz
  xmlto
  git
  "gcc>=11.0"
)
if [ "$_compiler" = "clang" ]; then
  pkgver="${pkgver}+clang"
  makedepends+=(clang llvm lld python)
  _LLVM=1
fi
options=('!strip')
_srcname=linux-stable-s0ix
source=(
  "$_srcname::git+https://gitlab.com/smbruce/linux-stable-s0ix.git#tag=$_tag"
  config

  #   NOTE: We pull from a stable kernel mirror with all upstream s0ix work included

  # graysky's compiler uarch optimization patch, script courtesy of the `linux-xanmod` AUR package
  "choose-gcc-optimization.sh"
  "more-uarches-for-kernel-5.15+.patch"::"https://raw.githubusercontent.com/graysky2/kernel_compiler_patch/4b184fc25abff605d47695713471fddc8292cd94/more-uarches-for-kernel-5.15%2B.patch"

  # compiler optimization flag changes from Xanmod; allow -O3, safer -O3
  "Makefile-Turn-off-loop-vectorization-for-GCC-O3.patch"
  "init-Kconfig-Enable-O3-KBUILD_CFLAGS-optimization.patch"

  # 5.16: AMD; don't drop shared caches on C3 state transitions
  "x86-ACPI-State-Optimize-C3-entry-on-AMD-CPUs.patch"

  # k10temp support for Zen3 APUs included in 5.15

  # Arch: optionally disallow unprivileged USER_NS clone
  "ZEN-Add-sysctl-and-CONFIG-to-disallow-unpriv-USER_NS.patch"

  # 5.16: change default spec_store_bypass_disable=prctl spectre_v2_user=prctl
  # see: https://git.kernel.org/kees/linux/c/id=h=for-next/seccomp&id=2f46993d83ff4abb310ef7b4beced56ba96f0d9d
  "x86-change-default-to-spec_store_bypass_disable-prct.patch"

  # squelch overly zealous wifi regdomain not set warnings; this removes spurious module "crashes" from the klog when using iwd
  "cfg80211-dont-WARN-if-a-self-managed-device.patch"

  # patch from Chromium developers; more accurately report battery state changes
  "acpi-battery-Always-read-fresh-battery-state-on-update.patch"

  #   NOTE: Optional features; feel free to comment these out (make changes to myconfig script as needed)

  # Google's TCP BBRv2
  "squashed-net-tcp_bbr-bbr2-for-5.14.y.patch"

  # Multigenerational LRU v5
  "squashed-mm-multigenerational-lru-v5-for-5.15.y.patch"

  # AMD pstate cpufreq driver
  "squashed-amd-pstate-v3-for-5.14.patch"

  # Updated zstd: v1.4.10
  "zstd-v1.4.10-v12.patch"

  # futex2 work backported from 5.16
  "futex2-backports-from-tip-5.16.patch"

  #   NOTE: All patches below this line support ASUS ROG laptops

  # ROG enablement patches; commented patches have hit upstream already
  "HID-asus-Reduce-object-size-by-consolidating-calls.patch"
  "v16-asus-wmi-Add-support-for-custom-fan-curves.patch"

  # cherry-picked iwlwifi patches from -next
  "iwlwifi-next-patches-through-2020-11-08.patch"

  # cherry-picked mediatek mt7921 bt/wifi support from -next and patchwork
  "mt76-mt7921-Fix-out-of-order-process-by-invalid-even.patch"
  "mt76-mt7921-Add-mt7922-support.patch"
  #"1-1-Bluetooth-btusb-Enable-MSFT-extension-for-Mediatek-Chip-MT7921.patch"   # 5.15
  "1-2-mt76-mt7915-send-EAPOL-frames-at-lowest-rate.patch"
  "2-2-mt76-mt7921-send-EAPOL-frames-at-lowest-rate.patch"
  "mt76-mt7921-enable-VO-tx-aggregation.patch"
  "mt76-mt7921-fix-dma-hang-in-rmmod.patch"
  "mt76-mt7921-fix-firmware-usage-of-RA-info-using-legacy-rates.patch"
  "mt76-mt7921-fix-the-inconsistent-state-between-bind-and-unbind.patch"
  "mt76-mt7921-report-HE-MU-radiotap.patch"
  "v2-mt76-mt7921-fix-kernel-warning-from-cfg80211_calculate_bitrate.patch"
  "1-2-mt76-mt7921-robustify-hardware-initialization-flow.patch"
  "2-2-mt76-mt7921-fix-retrying-release-semaphore-without-end.patch"
  "1-2-Bluetooth-btusb-Add-Mediatek-MT7921-support-for-Foxconn.patch"
  "2-2-Bluetooth-btusb-Add-Mediatek-MT7921-support-for-IMC-Network.patch"
  "Bluetooth-btusb-Add-support-for-IMC-Networks-Mediatek-Chip.patch"
  "Bluetooth-btusb-Add-support-for-Foxconn-Mediatek-Chip.patch"
  "Bluetooth-btusb-Add-support-for-IMC-Networks-Mediatek-Chip-MT7921.patch"
)
validpgpkeys=(
  'ABAF11C65A2970B130ABE3C479BE3E4300411886'  # Linus Torvalds
  '647F28654894E3BD457199BE38DBBDC86092693E'  # Greg Kroah-Hartman
  'A2FF3A36AAA56654109064AB19802F8B0D70FC30'  # Jan Alexander Steffens (heftig)
)
sha256sums=('SKIP'
            '0eecdc29b3bde4bccda282c4c04ef114250fedddcf3dbf07a3cc201dce35838f'
            '1ac18cad2578df4a70f9346f7c6fccbb62f042a0ee0594817fdef9f2704904ee'
            '14baea3bc9ffbe41737cfce6f0c5e6b536021571de7b5c7a9e39b31b94b23668'
            '4ed47b049cfc42289897e9f6dc85b548b712ef77bda6f186125f464cfe8aed91'
            '1e9b3c3fccfaba790335b9a83a87e129d66bfba850548f23865b76ea235b8558'
            '923230ed8367e28adfdeed75d3cdba9eec6b781818c37f6f3d3eb64101d2e716'
            '743001364eb7bf9ee208e60b74b7c68b46c4d03feae26dfcb8f7581d3bf14271'
            'cc401107f1bf7b7d8e8a78ee594f9db4b6fa252b7239b6aa88f678aef84d935c'
            '3d8961438b5c8110588ff0b881d472fc71a4304d306808d78a4055a4150f351e'
            'f7a4bf6293912bfc4a20743e58a5a266be8c4dbe3c1862d196d3a3b45f2f7c90'
            'ce15527dd8a3553d239ef4ff089b5b3a99076d306cf0f87a971e1fecbc6ac476'
            '7675116fc7da55cfb9c6ceaaf120c5ebd285a13b1f1641ca810dd05516c88e0b'
            '4968c6118011e07b34d925f064c65eb4fc007f470c3fb839f074c8dee4715ff6'
            'bcd501bdb51c4774b0470729ab64008e129a59200fb4bc8eb2dd2a6bce2e9223'
            'f839d4d43a20d5506357e860cf23bcd354d091afb4a6bcda411a7daa1cc48f00'
            '544464bf0807b324120767d55867f03014a9fda4e1804768ca341be902d7ade4'
            '0c422d8f420c1518aab1b980c6cdb6e029a4fa9cde1fd99a63670bb105a44f36'
            '4d0888b23d330310667572f204bd0f964b265ddb3cc5472c4d25ef76bfa8bf1d'
            '2163cb2e394a013042a40cd3b00dae788603284b20d71e262995366c5534e480'
            'a01cf700d79b983807e2285be1b30df6e02db6adfd9c9027fe2dfa8ca5a74bc9'
            '4bfbff4eba07fc9de2ce78097a4a269509468ba0e24c15a82905cd94e093ad55'
            '021f8539ab2fb722b46937b95fdab22a2308236a24ecc1a9ea8db4853721dd39'
            '1ce9fd988201c4d2e48794c58acda5b768ec0fea1d29555e99d35cd2712281e4'
            'e7e37c7c433c58e2f5a79e2a7724823bef1dccaa01e857584397b4e3c837d991'
            'f075ac354acfd65dff4db49dc9798747cb9b7a3dd9839987bc46495bdbbd22dc'
            '1770fec49335bc93194e9e55ced49e1cb67f2df4bf6948e80712a0b2ba50fa49'
            '6da4010f86a74125969fd3dbc953da7b45209d33ff3d216474c3399e82e893ff'
            'eb391b6d1ebf7ef99ece00b23609b94180a1f3c0149bcf05f6bbeb74d0b724c7'
            'c368cc4eefff20b7ae904eec686b7e72b46ff02b32c8a4fbd6bd4039f087e7ba'
            '1a8639167a1ee1b66f580c0c6f8304e6ef359a68cfa3eb869d9200a9f0234098'
            '236cdadf0b1472945c0d7570caeed7b95929aabed6872319c9d0969a819689e9'
            'cc2aa580d69801aa1afb0d72ecf094fe13c797363d3d5928c868d3a389910b7b'
            '292a7e32b248c7eee6e2f5407d609d03d985f367d329adb02b9d6dba1f85b44c'
            '7dbfdd120bc155cad1879579cb9dd1185eb5e37078c8c93fef604a275a163812'
            '1444af2e125080934c67b6adb4561fd354a72ce47d3de393b24f53832ee492ac')

export KBUILD_BUILD_HOST=archlinux
export KBUILD_BUILD_USER=$pkgbase
export KBUILD_BUILD_TIMESTAMP="$(date -Ru${SOURCE_DATE_EPOCH:+d @$SOURCE_DATE_EPOCH})"

# default to x86-64-v3 microarch if not set
# other notable values:
#  Zen2: 14
#  Zen3: 15
#  Skylake & Comet Lake: 38
#  Intel/AMD Native: 98/99
_microarchitecture=${_microarchitecture:-'93'}

prepare() {
  cd $_srcname

  echo "Setting version..."
  scripts/setlocalversion --save-scmversion
  echo "-$pkgrel" > localversion.99-pkgrel
  echo "${pkgbase#linux}" > localversion.20-pkgname

  local src
  for src in "${source[@]}"; do
    src="${src%%::*}"
    src="${src##*/}"
    [[ "$src" =~ .*(patch|diff)$ ]] || continue
    echo "  ${BLUE}-> ${ALL_OFF}${BOLD}Applying patch: ${ALL_OFF}$src"
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

  ## CONFIG_STACK_VALIDATION gives better stack traces. Also is enabled in all official kernel packages by Archlinux team
  scripts/config --enable CONFIG_STACK_VALIDATION

  ## Enable IKCONFIG following Arch's philosophy
  scripts/config --enable CONFIG_IKCONFIG \
                 --enable CONFIG_IKCONFIG_PROC

  ## apply package kernel config customizations
  if [[ -s ${startdir}/package-config-customizations ]]; then
    msg2 "Applying package config customizations..."
    bash -x "${startdir}/package-config-customizations"
  fi

  ## apply any user config customizations
  if [[ -s ${startdir}/myconfig ]]; then
    msg2 "Applying user supplied config customizations..."
    bash -x "${startdir}/myconfig"
  fi

  echo "Finalizing kernel config..."
  make LLVM=$_LLVM LLVM_IAS=$_LLVM olddefconfig

  # let user choose microarchitecture optimization in GCC; run *after* make olddefconfig so our new uarch macros exist
  sh "${srcdir}/choose-gcc-optimization.sh" "$_microarchitecture"

  make -s kernelrelease > version
  echo "Prepared $pkgbase version $(<version)"

  # retain config for re-use
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
  replaces=(virtualbox-guest-modules-mainline wireguard-maineline)

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

  # add objtool for external module building and enabled VALIDATION_STACK option
  install -Dt "$builddir/tools/objtool" tools/objtool/objtool

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
