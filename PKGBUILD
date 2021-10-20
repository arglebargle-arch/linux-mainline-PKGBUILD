#
# Maintainer: Arglebargle <arglebargle AT arglebargle DOT dev>
#
# Based on the linux package by:
# Maintainer: Mikael Eriksson <mikael_eriksson@miffe.org>
# Maintainer: Jan Alexander Steffens (heftig) <jan.steffens@gmail.com>
# Maintainer: Tobias Powalowski <tpowa@archlinux.org>
# Maintainer: Thomas Baechler <thomas@archlinux.org>

_pkgbase=linux-mainline
pkgbase=linux-mainline-amd-s0ix   # rename to custom pkgbase
_tag=v5.15-rc6-s0ix
pkgver=5.15rc6
pkgrel=3
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
options=('!strip')
#_srcname=linux-mainline
_srcname=linux-stable-s0ix
source=(
  #"$_srcname::git+https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git#tag=$_tag"
  "$_srcname::git+https://gitlab.com/smbruce/linux-stable-s0ix.git#tag=$_tag"
  config              # the main kernel config file

  # graysky's compiler uarch optimization patch, script courtesy of the `linux-xanmod` AUR package
  "choose-gcc-optimization.sh"
  "more-uarches-for-kernel-5.15+.patch"::"https://raw.githubusercontent.com/graysky2/kernel_compiler_patch/4b184fc25abff605d47695713471fddc8292cd94/more-uarches-for-kernel-5.15%2B.patch"

  # optimization flags
  "Makefile-Turn-off-loop-vectorization-for-GCC-O3.patch"
  "init-Kconfig-Enable-O3-KBUILD_CFLAGS-optimization.patch"

  ## NOTE: We now pull from a kernel tree with all s0ix related patches included; all patches current as of PKGBUILD release

  # patch from Chromium developers; more accurately report battery state changes
  "acpi-battery-Always-read-fresh-battery-state-on-update.patch"

  # AMD; don't drop shared caches on C3 state transitions
  "x86-ACPI-State-Optimize-C3-entry-on-AMD-CPUs.patch"

  # k10temp support for Zen3 APUs                                           # included in 5.14/5.15

  ## NOTE: Optional features; feel free to comment these out (make changes to myconfig script as needed)

  # Google's TCP BBRv2
  "squashed-net-tcp_bbr-bbr2-for-5.14.y.patch"

  # Multigenerational LRU v4
  #"squashed-mm-multigenerational-lru-v4-for-5.14.y.patch"                  # TODO: not yet compatible with 5.15

  # AMD pstate cpufreq driver
  "amd-pstate-v2.patch"

  # Updated zstd: v1.4.10
  "zstd-v1.4.10-v12.patch"

  # futex2 work backported from 5.16
  "futex2-backports-from-tip-5.16.patch"

  ## NOTE: All patches below this line can be removed if you're not using an ASUS laptop; though they won't cause problems if left in

  # ROG enablement patches; commented patches have hit upstream already
  "HID-asus-Reduce-object-size-by-consolidating-calls.patch"
  "v15-asus-wmi-Add-support-for-custom-fan-curves.patch"

  # improve mediatek mt7921 bt/wifi support
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
            'be9551f441bb1c01678d3877785666715b92c3874f19eab9cf766dacac15fcd7'
            '1ac18cad2578df4a70f9346f7c6fccbb62f042a0ee0594817fdef9f2704904ee'
            '14baea3bc9ffbe41737cfce6f0c5e6b536021571de7b5c7a9e39b31b94b23668'
            '4ed47b049cfc42289897e9f6dc85b548b712ef77bda6f186125f464cfe8aed91'
            '1e9b3c3fccfaba790335b9a83a87e129d66bfba850548f23865b76ea235b8558'
            'f7a4bf6293912bfc4a20743e58a5a266be8c4dbe3c1862d196d3a3b45f2f7c90'
            '923230ed8367e28adfdeed75d3cdba9eec6b781818c37f6f3d3eb64101d2e716'
            'ce15527dd8a3553d239ef4ff089b5b3a99076d306cf0f87a971e1fecbc6ac476'
            '9025ca0788fbacea25200e6ac17036960000424843f544cdd781052231da7903'
            'bcd501bdb51c4774b0470729ab64008e129a59200fb4bc8eb2dd2a6bce2e9223'
            'ce6754d9911239df5677ceae375ea25ad92aeb44a6352e3bfe400dfd654cd3ad'
            '544464bf0807b324120767d55867f03014a9fda4e1804768ca341be902d7ade4'
            '1e547bddf80d201f77da1d876cd280e4d40b377bbd8ebc218f0ba57cd959ff76'
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
    echo "Applying patch $src..."
    patch -Np1 < "../$src"
  done

  echo "Setting config..."
  cp ../config .config
  make olddefconfig

  # let user choose microarchitecture optimization in GCC; run *after* make olddefconfig so our new uarch macros exist
  sh ${srcdir}/choose-gcc-optimization.sh $_microarchitecture

  ## CONFIG_STACK_VALIDATION gives better stack traces. Also is enabled in all official kernel packages by Archlinux team
  scripts/config --enable CONFIG_STACK_VALIDATION

  ## Enable IKCONFIG following Arch's philosophy
  scripts/config --enable CONFIG_IKCONFIG \
                 --enable CONFIG_IKCONFIG_PROC

  ## apply any user config customizations
  if [[ -s ${startdir}/myconfig-fragment ]]; then
    msg2 "Applying config fragment..."
    bash -x ${startdir}/myconfig-fragment
  fi

  make -s kernelrelease > version
  echo "Prepared $pkgbase version $(<version)"

  # retain config for re-use
  cat .config > "${startdir}/config.last"
}

build() {
  cd $_srcname
  make all
  #make htmldocs
}

_package() {
  pkgdesc="The $pkgdesc kernel and modules"
  depends=(coreutils kmod initramfs)
  optdepends=('crda: to set the correct wireless channels of your country'
              'linux-firmware: firmware images needed for some devices')
  provides=(VIRTUALBOX-GUEST-MODULES WIREGUARD-MODULE ${_pkgbase})
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
  provides=(${_pkgbase}-headers)

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
        strip -v $STRIP_SHARED "$file" ;;
      application/x-archive\;*)        # Libraries (.a)
        strip -v $STRIP_STATIC "$file" ;;
      application/x-executable\;*)     # Binaries
        strip -v $STRIP_BINARIES "$file" ;;
      application/x-pie-executable\;*) # Relocatable binaries
        strip -v $STRIP_SHARED "$file" ;;
    esac
  done < <(find "$builddir" -type f -perm -u+x ! -name vmlinux -print0)

  echo "Stripping vmlinux..."
  strip -v $STRIP_STATIC "$builddir/vmlinux"

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
    $(declare -f "_package${_p#$pkgbase}")
    _package${_p#$pkgbase}
  }"
done

# vim:set ts=8 sts=2 sw=2 et:
