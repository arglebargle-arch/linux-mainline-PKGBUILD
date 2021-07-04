#
# Maintainer: Arglebargle <arglebargle AT arglebargle DOT dev>
#
# Based on the linux package by:
# Maintainer: Mikael Eriksson <mikael_eriksson@miffe.org>
# Maintainer: Jan Alexander Steffens (heftig) <jan.steffens@gmail.com>
# Maintainer: Tobias Powalowski <tpowa@archlinux.org>
# Maintainer: Thomas Baechler <thomas@archlinux.org>

pkgbase=linux-mainline-amd-s0ix   # rename to custom pkgbase
_tag=v5.13
pkgver=5.13
pkgrel=1.4                        # apply asus rog enablement patches
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
_srcname=linux-mainline
source=(
  "$_srcname::git+https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git#tag=$_tag"
  config         # the main kernel config file

  # USER_NS_UNPRIVILEGED security option from the Zen kernel
  "ZEN-disallow-unprivileged-CLONE_NEWUSER.patch"

  # graysky's compiler uarch optimization patch, script courtesy of the `linux-xanmod` AUR package
  "choose-gcc-optimization.sh"
  "more-uarches-for-kernel-5.8+.patch"::"https://raw.githubusercontent.com/graysky2/kernel_compiler_patch/a8d200f422f4b2abeaa6cfcfa37136b308e6e33e/more-uarches-for-kernel-5.8%2B.patch"

  # backported s0ix enablement patches, d3hot quirk and amd_pmc debugging
  "backport-from-5.14-s0ix-enablement-no-d3hot.diff"
  "PCI-quirks-Quirk-PCI-d3hot-delay-for-AMD-xhci.patch"
  "v5-platform-x86-amd-pmc-s0ix+smu-counters.diff"
  "ACPI-PM-Only-mark-EC-GPE-for-wakeup-on-Intel-systems.patch"

  # ROG enablement patches; commented patches have hit upstream already
  "0001-asus-wmi-Add-panel-overdrive-functionality.patch"
  "0002-asus-wmi-Add-dgpu-disable-method.patch"
  "0003-asus-wmi-Add-egpu-enable-method.patch"
  #"0004-HID-asus-Filter-keyboard-EC-for-old-ROG-keyboard.patch"
  #"0005-HID-asus-filter-G713-G733-key-event-to-prevent-shutd.patch"
  "0006-HID-asus-Remove-check-for-same-LED-brightness-on-set.patch"
  "0007-ALSA-hda-realtek-Fix-speakers-not-working-on-Asus-Fl.patch"
  #"0008-ACPI-video-use-native-backlight-for-GA401-GA502-GA50.patch"
  #"0009-Revert-platform-x86-asus-nb-wmi-Drop-duplicate-DMI-q.patch"
)
validpgpkeys=(
  'ABAF11C65A2970B130ABE3C479BE3E4300411886'  # Linus Torvalds
  '647F28654894E3BD457199BE38DBBDC86092693E'  # Greg Kroah-Hartman
  'A2FF3A36AAA56654109064AB19802F8B0D70FC30'  # Jan Alexander Steffens (heftig)
)
sha256sums=('SKIP'
            '6dde032690644a576fd36c4a7d3546d9cec0117dd3fb17cea6dc95e907ef9bef'
            '5723f61e6811cd3db649f08aafb7b1cd08cd5e66433d349bf2500d7beabbd0cd'
            '1ac18cad2578df4a70f9346f7c6fccbb62f042a0ee0594817fdef9f2704904ee'
            'fa6cee9527d8e963d3398085d1862edc509a52e4540baec463edb8a9dd95bee0'
            'e4cbedbcf939961af425135bb208266c726178c4017309719341f8c37f65c273'
            'dab4db308ede1aa35166f31671572eeccf0e7637b3218ce3ae519c2705934f79'
            'b108959c4a53d771eb2d860a7d52b4a6701e0af9405bef325905c0e273b4d4fe'
            '30c3ebf86e6b70ca9e35b5b9bcf39a3b3d14cb9ca18b261016b7d02ed37a0c4b'
            '09cf9fa947e58aacf25ff5c36854b82d97ad8bda166a7e00d0f3f4df7f60a695'
            '7a685e2e2889af744618a95ef49593463cd7e12ae323f964476ee9564c208b77'
            '663b664f4a138ccca6c4edcefde6a045b79a629d3b721bfa7b9cc115f704456e'
            '034743a640c26deca0a8276fa98634e7eac1328d50798a3454c4662cff97ccc9'
            '32bbcde83406810f41c9ed61206a7596eb43707a912ec9d870fd94f160d247c1')

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

  # -- make any configuration changes that involve our added patches here:

  # let user choose microarchitecture optimization in GCC; run *after* make olddefconfig so our new uarch macros exist
  sh ${srcdir}/choose-gcc-optimization.sh $_microarchitecture

  ## CONFIG_STACK_VALIDATION gives better stack traces. Also is enabled in all official kernel packages by Archlinux team
  scripts/config --enable CONFIG_STACK_VALIDATION

  ## Enable IKCONFIG following Arch's philosophy
  scripts/config --enable CONFIG_IKCONFIG \
                 --enable CONFIG_IKCONFIG_PROC

  ## Disallow unprivileged namespaces
  #scripts/config --disable USER_NS_UNPRIVILEGED

  # --
  # enable scheduler autogrouping
  scripts/config --enable CONFIG_SCHED_AUTOGROUP_DEFAULT_ENABLED
  # larger log buffer
  scripts/config --set-val CONFIG_LOG_BUF_SHIFT 18
  # msr as a module
  scripts/config --module CONFIG_X86_MSR
  # enable EFI var access
  scripts/config --enable CONFIG_EFI_VARS
  # turn a few non-default options into modules
  scripts/config --module CONFIG_MQ_IOSCHED_DEADLINE
  scripts/config --module CONFIG_MQ_IOSCHED_KYBER
  scripts/config --module CONFIG_BINFMT_MISC
  scripts/config --module CONFIG_ZBUD
  scripts/config --module CONFIG_ZSMALLOC
  # enable module versioning
  scripts/config --enable CONFIG_MODVERSIONS
  # usb bbr by default; bbr2 would be better but we'll take what we can get
  scripts/config --module CONFIG_TCP_CONG_CUBIC
  scripts/config --enable CONFIG_TCP_CONG_BBR
  scripts/config --disable CONFIG_DEFAULT_CUBIC
  scripts/config --enable CONFIG_DEFAULT_BBR
  scripts/config --set-str CONFIG_DEFAULT_TCP_CONG "bbr"
  # use a smaller connection tracking table
  scripts/config --set-val CONFIG_IP_VS_TAB_BITS 12
  # use fq_pie by default
  scripts/config --module CONFIG_NET_SCH_FQ_CODEL
  scripts/config --enable CONFIG_NET_SCH_PIE
  scripts/config --enable CONFIG_NET_SCH_FQ_PIE
  scripts/config --disable CONFIG_DEFAULT_FQ_CODEL
  scripts/config --enable CONFIG_DEFAULT_FQ_PIE
  scripts/config --set-str CONFIG_DEFAULT_NET_SCH "fq_pie"
  # enable bluetooth high speed
  scripts/config --enable CONFIG_BT_HS
  # nein
  scripts/config --disable CONFIG_UEVENT_HELPER
  scripts/config --disable CONFIG_FW_LOADER_USER_HELPER
  # enable zram memory tracking
  scripts/config --enable CONFIG_ZRAM_MEMORY_TRACKING
  # --

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
  provides=(VIRTUALBOX-GUEST-MODULES WIREGUARD-MODULE)
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

  # http://bugs.archlinux.org/task/13146
  install -Dt "$builddir/drivers/media/i2c" -m644 drivers/media/i2c/msp3400-driver.h

  # http://bugs.archlinux.org/task/20402
  install -Dt "$builddir/drivers/media/usb/dvb-usb" -m644 drivers/media/usb/dvb-usb/*.h
  install -Dt "$builddir/drivers/media/dvb-frontends" -m644 drivers/media/dvb-frontends/*.h
  install -Dt "$builddir/drivers/media/tuners" -m644 drivers/media/tuners/*.h

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
