# Maintainer: Ryan Davies <ryan@ryandavies.co.nz>

pkgname=plymouth-lite
pkgver=0.6.0
pkgrel=5.03
__gittag="${pkgver}-5"
pkgdesc="Boot splash screen based on Fedora's Plymouth code"
arch=('i686' 'x86_64')
url="https://github.com/nemomobile/plymouth-lite"
depends=('libpng12')
conflicts=('plymouth')
makedepends=('git')
license=('GPL')
install="${pkgname}.install"
backup=("usr/share/plymouth/splash.png")
source=(
  "$pkgname"::"git://github.com/nemomobile/plymouth-lite.git#tag=${__gittag}"
  "plymouth-lite-halt.service"
  "plymouth-lite-poweroff.service"
  "plymouth-lite-reboot.service"
  "plymouth-lite-start.service"
  "plymouth-lite-0.6.0-01-fix-build.patch"
  "plymouth-lite-0.6.0-21-16bpp.patch"
  "plymouth-lite_hook"
  "plymouth-lite_install"
)
md5sums=('SKIP'
	 'a03b0fa95566db725b69cfafc1cbf791'
	 'b0c12b2b3fe23118257a4494dac2b4c9'
	 'dd286d9a51d06045fdc510fe9012dae4'
	 'f973ce29a093c5a0ac80914838b1dba7'
         '18561abca5072725729c071ccee895fb'
         'db30b2b2e831d46f1eb4ba5a951828b0'
	 '61896a627a31b5fa6805d258af778276'
	 'e134b6996ceb4479f78b9c232ad404fa'
)
build(){
  cd "${srcdir}/plymouth-lite"
  ./configure

  patch -p1 < "${srcdir}/plymouth-lite-0.6.0-01-fix-build.patch"
  patch -p1 < "${srcdir}/plymouth-lite-0.6.0-21-16bpp.patch"

  nice -n 10 make -j $(cat /proc/cpuinfo | grep vendor |wc -l)
}

package() {
  cd "${srcdir}/plymouth-lite"
  make DESTDIR="${pkgdir}" install
  
  mkdir -p "${pkgdir}/usr/lib/systemd/system"
  install -D -m 0644 -T "${srcdir}/plymouth-lite-halt.service" "${pkgdir}/usr/lib/systemd/system/plymouth-lite-halt.service"
  install -D -m 0644 -T "${srcdir}/plymouth-lite-poweroff.service" "${pkgdir}/usr/lib/systemd/system/plymouth-lite-poweroff.service"
  install -D -m 0644 -T "${srcdir}/plymouth-lite-reboot.service" "${pkgdir}/usr/lib/systemd/system/plymouth-lite-reboot.service"
  install -D -m 0644 -T "${srcdir}/plymouth-lite-start.service" "${pkgdir}/usr/lib/systemd/system/plymouth-lite-start.service"

  mkdir -p "${pkgdir}/usr/share/plymouth/"
  install -D -m 0644 -T "${srcdir}/plymouth-lite/splash.png" "${pkgdir}/usr/share/plymouth/splash.png"

  mkdir -p ${pkgdir}/usr/lib/systemd/system/sysinit.target.wants
  cd ${pkgdir}/usr/lib/systemd/system/sysinit.target.wants
  ln -s ../plymouth-lite-start.service

  mkdir -p ${pkgdir}/usr/lib/systemd/system/halt.target.wants
  cd ${pkgdir}/usr/lib/systemd/system/halt.target.wants
  ln -s ../plymouth-lite-halt.service

  mkdir -p ${pkgdir}/usr/lib/systemd/system/poweroff.target.wants
  cd ${pkgdir}/usr/lib/systemd/system/poweroff.target.wants
  ln -s ../plymouth-lite-poweroff.service

  mkdir -p ${pkgdir}/usr/lib/systemd/system/reboot.target.wants
  cd ${pkgdir}/usr/lib/systemd/system/reboot.target.wants
  ln -s ../plymouth-lite-reboot.service

  install -Dm0644 -T "${srcdir}/plymouth-lite_hook" "${pkgdir}/usr/lib/initcpio/hooks/plymouth-lite"
  install -Dm0644 -T "${srcdir}/plymouth-lite_install" "${pkgdir}/usr/lib/initcpio/install/plymouth-lite"

}
