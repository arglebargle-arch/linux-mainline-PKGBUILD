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
_tag=v5.14-rc1-s0ix
pkgver=5.14rc1
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
  myconfig-fragment   # kernel config customizations

  # hotfix 5.14-rc1; fix systemd-timesyncd & ntpsec
  "hotfix-5.14rc1-sock-fix-error-in-sock_setsockopt.patch"

  # graysky's compiler uarch optimization patch, script courtesy of the `linux-xanmod` AUR package
  "choose-gcc-optimization.sh"
  "more-uarches-for-kernel-5.8+.patch"::"https://raw.githubusercontent.com/graysky2/kernel_compiler_patch/a8d200f422f4b2abeaa6cfcfa37136b308e6e33e/more-uarches-for-kernel-5.8%2B.patch"

  # NOTE: We're now pulling from a linux tree mirror with the s0ix patches integrated
  #       builds include all neccessary s0ix patches as of the current PKGBUILD release

  # k10temp support for Zen3 APUs
  #"8001-x86-amd_nb-Add-AMD-family-19h-model-50h-PCI-ids.patch"             # included in 5.14
  "8002-hwmon-k10temp-support-Zen3-APUs.patch"

  # ROG enablement patches; commented patches have hit upstream already
  "0101-asus-wmi-Add-panel-overdrive-functionality.patch"                   # scheduled for 5.15?
  "0102-asus-wmi-Add-dgpu-disable-method.patch"
  "0103-asus-wmi-Add-egpu-enable-method.patch"
  #"0004-HID-asus-Filter-keyboard-EC-for-old-ROG-keyboard.patch"
  #"0005-HID-asus-filter-G713-G733-key-event-to-prevent-shutd.patch"
  "0006-HID-asus-Remove-check-for-same-LED-brightness-on-set.patch"
  "0007-ALSA-hda-realtek-Fix-speakers-not-working-on-Asus-Fl.patch"
  #"0008-ACPI-video-use-native-backlight-for-GA401-GA502-GA50.patch"
  #"0009-Revert-platform-x86-asus-nb-wmi-Drop-duplicate-DMI-q.patch"

  # improve mediatek mt7921 bt/wifi support
  #"8010-Bluetooth-btusb-Fixed-too-many-in-token-issue-for-Me.patch"        # included in 5.14rc1
  #"8011-Bluetooth-btusb-Add-support-for-Lite-On-Mediatek-Chi.patch"
  "8012-mt76-mt7921-continue-to-probe-driver-when-fw-already.patch"
  "8013-mt76-mt7921-Fix-out-of-order-process-by-invalid-even.patch"
  "8014-mt76-mt7921-Add-mt7922-support.patch"
)
validpgpkeys=(
  'ABAF11C65A2970B130ABE3C479BE3E4300411886'  # Linus Torvalds
  '647F28654894E3BD457199BE38DBBDC86092693E'  # Greg Kroah-Hartman
  'A2FF3A36AAA56654109064AB19802F8B0D70FC30'  # Jan Alexander Steffens (heftig)
)
sha256sums=('SKIP'
            '6030ad40747f2055165a6a9081122034ed45283b51533c9018eda6ebec200b84'
            'c43d97768e5961ed3d53ae2328a711cee1eddcc64ce7cd1091a9c41759b95ab3'
            '410bcdcec7d16d2a5a5d646915ab9c34c0929c1af6d51f2fe239695d448e3346'
            '1ac18cad2578df4a70f9346f7c6fccbb62f042a0ee0594817fdef9f2704904ee'
            'fa6cee9527d8e963d3398085d1862edc509a52e4540baec463edb8a9dd95bee0'
            'de8c9747637768c4356c06aa65c3f157c526aa420f21fdd5edd0ed06f720a62e'
            '1ab75535772c63567384eb2ac74753e4d5db2f3317cb265aedf6151b9f18c6c2'
            '8cc771f37ee08ad5796e6db64f180c1415a5f6e03eb3045272dade30ca754b53'
            'f3461e7cc759fd4cef2ec5c4fa15b80fa6d37e16008db223f77ed88a65aa938e'
            '034743a640c26deca0a8276fa98634e7eac1328d50798a3454c4662cff97ccc9'
            '32bbcde83406810f41c9ed61206a7596eb43707a912ec9d870fd94f160d247c1'
            '13f1c3a15fb1418b4aee0594e1f7871151303ca4f7eaab3c6f2ea21af965d85b'
            '2163cb2e394a013042a40cd3b00dae788603284b20d71e262995366c5534e480'
            'a01cf700d79b983807e2285be1b30df6e02db6adfd9c9027fe2dfa8ca5a74bc9')

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
