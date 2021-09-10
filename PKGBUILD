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
_tag=v5.14.2-s0ix
pkgver=5.14.2
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
options=('!strip')
#_srcname=linux-mainline
_srcname=linux-stable-s0ix
source=(
  #"$_srcname::git+https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git#tag=$_tag"
  "$_srcname::git+https://gitlab.com/smbruce/linux-stable-s0ix.git#tag=$_tag"
  config              # the main kernel config file
  myconfig-fragment   # kernel config customizations

  # graysky's compiler uarch optimization patch, script courtesy of the `linux-xanmod` AUR package
  "choose-gcc-optimization.sh"
  "more-uarches-for-kernel-5.8+.patch"::"https://raw.githubusercontent.com/graysky2/kernel_compiler_patch/a8d200f422f4b2abeaa6cfcfa37136b308e6e33e/more-uarches-for-kernel-5.8%2B.patch"

  ## NOTE: We now pull from a kernel tree with all s0ix related patches included; all patches current as of PKGBUILD release

  # patch from Chromium developers; more accurately report battery state changes
  "acpi-battery-Always-read-fresh-battery-state-on-update.patch"

  # AMD; don't drop shared caches on C3 state transitions
  "x86-ACPI-State-Optimize-C3-entry-on-AMD-CPUs.patch"

  # k10temp support for Zen3 APUs
  #"x86-amd_nb-Add-AMD-family-19h-model-50h-PCI-ids.patch"                  # included in 5.14
  "hwmon-k10temp-support-Zen3-APUs.patch"

  ## NOTE: Optional features; feel free to comment these out

  # TCP BBRv2
  "squashed-net-tcp_bbr-bbr2-for-5.14.y.patch"

  # Multigenerational LRU v4
  "squashed-mm-multigenerational-lru-v4-for-5.14.y.patch"

  # AMD pstate cpufreq driver
  "amd-pstate-v1.patch"

  ## NOTE: All patches below this line can be removed if you're not using an ASUS laptop; though they won't cause problems if left in

  # ROG enablement patches; commented patches have hit upstream already
  "asus-wmi-Add-panel-overdrive-functionality.patch"
  "asus-wmi-Add-dgpu-disable-method.patch"
  "asus-wmi-Add-egpu-enable-method.patch"
  #"HID-asus-Filter-keyboard-EC-for-old-ROG-keyboard.patch"                 # previously applied in mainline/stable
  #"HID-asus-filter-G713-G733-key-event-to-prevent-shutd.patch"
  #"HID-asus-Remove-check-for-same-LED-brightness-on-set.patch"
  #"ALSA-hda-realtek-Fix-speakers-not-working-on-Asus-Fl.patch"
  #"ACPI-video-use-native-backlight-for-GA401-GA502-GA50.patch"
  #"Revert-platform-x86-asus-nb-wmi-Drop-duplicate-DMI-q.patch"
  "HID-asus-Prevent-Claymore-sending-suspend-event.patch"
  "HID-asus-Reduce-object-size-by-consolidating-calls.patch"
  "v5-asus-wmi-Add-support-for-platform_profile.patch"
  "v11-asus-wmi-Add-support-for-custom-fan-curves.patch"

  # improve mediatek mt7921 bt/wifi support
  #"Bluetooth-btusb-Fixed-too-many-in-token-issue-for-Me.patch"             # included in 5.14
  #"Bluetooth-btusb-Add-support-for-Lite-On-Mediatek-Chi.patch"
  #"mt76-mt7921-continue-to-probe-driver-when-fw-already.patch"
  "mt76-mt7921-Fix-out-of-order-process-by-invalid-even.patch"
  "mt76-mt7921-Add-mt7922-support.patch"
  "1-1-Bluetooth-btusb-Enable-MSFT-extension-for-Mediatek-Chip-MT7921.patch"
  "1-2-mt76-mt7915-send-EAPOL-frames-at-lowest-rate.patch"
  "2-2-mt76-mt7921-send-EAPOL-frames-at-lowest-rate.patch"
  "mt76-mt7921-enable-VO-tx-aggregation.patch"
  "mt76-mt7921-fix-dma-hang-in-rmmod.patch"
  "mt76-mt7921-fix-firmware-usage-of-RA-info-using-legacy-rates.patch"
  "mt76-mt7921-fix-the-inconsistent-state-between-bind-and-unbind.patch"
  "mt76-mt7921-report-HE-MU-radiotap.patch"
  "v2-mt76-mt7921-fix-kernel-warning-from-cfg80211_calculate_bitrate.patch"
)
validpgpkeys=(
  'ABAF11C65A2970B130ABE3C479BE3E4300411886'  # Linus Torvalds
  '647F28654894E3BD457199BE38DBBDC86092693E'  # Greg Kroah-Hartman
  'A2FF3A36AAA56654109064AB19802F8B0D70FC30'  # Jan Alexander Steffens (heftig)
)
sha256sums=('SKIP'
            'ba0f9af0a6af0f2839daf3eef9bafeb6b6efc07455b05273f5398aca0ec8ab69'
            'e1546a72b40ae0d27844ececad7f57cb9f836ae43f11fc9eca52fa4ce8ec050f'
            '1ac18cad2578df4a70f9346f7c6fccbb62f042a0ee0594817fdef9f2704904ee'
            'fa6cee9527d8e963d3398085d1862edc509a52e4540baec463edb8a9dd95bee0'
            'f7a4bf6293912bfc4a20743e58a5a266be8c4dbe3c1862d196d3a3b45f2f7c90'
            '923230ed8367e28adfdeed75d3cdba9eec6b781818c37f6f3d3eb64101d2e716'
            'de8c9747637768c4356c06aa65c3f157c526aa420f21fdd5edd0ed06f720a62e'
            'ce15527dd8a3553d239ef4ff089b5b3a99076d306cf0f87a971e1fecbc6ac476'
            '69ecf5456468935958f2cbf35691c2533a56344005537902b6051b6323ffff1f'
            'e62cbe1cb1577b1d80095fbb566d0516592e6174e7740e61a340164aff9bf2ec'
            '1ab75535772c63567384eb2ac74753e4d5db2f3317cb265aedf6151b9f18c6c2'
            '8cc771f37ee08ad5796e6db64f180c1415a5f6e03eb3045272dade30ca754b53'
            'f3461e7cc759fd4cef2ec5c4fa15b80fa6d37e16008db223f77ed88a65aa938e'
            'ec317cc2c2c8c1186c4f553fdd010adc013c37600a499802473653fd8e7564df'
            '544464bf0807b324120767d55867f03014a9fda4e1804768ca341be902d7ade4'
            '4ef12029ea73ca924b6397e1de4911e84d9e77ddaccdab1ef579823d848524e8'
            '4640313efcffe48dc182486e1f948679ad92c1a4871a2140c1d4624526453497'
            '2163cb2e394a013042a40cd3b00dae788603284b20d71e262995366c5534e480'
            'a01cf700d79b983807e2285be1b30df6e02db6adfd9c9027fe2dfa8ca5a74bc9'
            'ea1d552f8fe6907e4fbd374842a655a9a64529e021c45d8459a0595c739e5cc6'
            '051769c129e0e3a5b516b8799712e1a39dd36216d77879b33b416c8e0fd67d7a'
            'fa96d4e690f3e0b51075be06fe47fe5b6d94b10835767c13416701690e842e4b'
            '3ed940a006bc1846daac9ca1194bcbffc0b7b71266d0527b7508f2263cdba9d6'
            '1687b5d7cefdcdbe9f0152d0b38e204229ce75994b1ba5f9fee5eff65580e6a2'
            '16c30e45665f8be034b25d3a21a9ed4cba025dd38293b77aaa12426892091adb'
            '5b7a106d371fcf880920967d7e36728f1bcc0368eaa7bf75ebf67a4ddb93c6d5'
            'aa5bb422421cb7e1340d8f07b5471995bbc3c7dd7cf91db76ab1dbe7efc2777a'
            '5e66b5a6a775ad42489dfd0f6057b69dae696a5ec8be428da329f68c1265764a')

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
