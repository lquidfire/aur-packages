# Maintainer: Jonathan Steel <jsteel at archlinux.org>
# Contributor: Daniel J Griffiths <ghost1227@archlinux.us>
# Contributor: Stefan Clarke <fm0nk3y@yahoo.co.uk>

pkgname=gnubg
pkgver=1.05.000
pkgrel=1
pkgdesc="World class backgammon application"
arch=('i686' 'x86_64')
url="http://www.gnubg.org"
license=('GPL3')
depends=('python2' 'gtkglext' 'hicolor-icon-theme' 'curl')
install=$pkgname.install
source=($url/media/sources/$pkgname-release-$pkgver-sources.tar.gz
        $pkgname.desktop)
md5sums=('2c2d291deedcb72d1fcbbd8faf90d55c'
         '965f5c7c25f60b27d06cc6fef7befd30')

build() {
  cd $pkgname-$pkgver

  ./autogen.sh

  ./configure --prefix=/usr --bindir=/usr/bin --sysconfdir=/etc \
    --mandir=/usr/share/man --enable-simd=sse2

  make
}

package() {
  cd $pkgname-$pkgver

  make DESTDIR="$pkgdir" install

  install -Dm644 "$srcdir"/$pkgname.desktop \
    "$pkgdir"/usr/share/applications/$pkgname.desktop
}
