# Maintainer: Edmund Lodewijks <e.lodewijks at gmail.com>

pkgname=openarc-git
_pkgname=openarc
pkgver=1.2.0.r17.g3ff4b07
pkgrel=1
pkgdesc="Open source implementation of the ARC email authentication system"
arch=(x86_64)
url="https://github.com/flowerysong/OpenARC"
license=('BSD-2-Clause' 'LicenseRef-Sendmail-1.1')
depends=('sh' 'glibc' 'jansson' 'openssl' 'libbsd' 'libmilter' 'libidn2')
optdepends=('smtp-server: for using a local mail server'
	    'bind: required only for signature verification (alternatives available)'
	    'dkimpy-milter: for optional tests'
	    'perl-mail-dkim: for optional tests')
makedepends=('git' 'python-miltertest')
provides=('openarc')
conflicts=('openarc' 'openarc-unofficial-patches')
source=("git+https://github.com/flowerysong/OpenARC.git#branch=main"
        openarc.service
        openarc.sysusers
        openarc.tmpfiles)
backup=('etc/openarc/openarc.conf')
sha256sums=('SKIP'
            'd438b4a2e0ab5b247938213da7e8062fa5865e750e4f89d41471311edc163022'
            '31c399c0e3a69bb845b033ab5c0ad92d44cacb0fd58e0113cd1901e75900515e'
            'a27619fe3bbea2a0fd7c555851089722b1d67818bc014d1dce20620b5eb4bbc5')

pkgver() {
  cd "$srcdir/OpenARC"

  git describe --long --tags | sed 's/rel.openarc.//;s/^v//;s/\([^-]*-g\)/r\1/;s/-/./g'
}

prepare() {
  cd "$srcdir/OpenARC"
  autoreconf -fvi
}

build() {
  cd "$srcdir/OpenARC"
  ./configure \
     --prefix=/usr \
     --sbindir=/usr/bin \
     --sysconfdir="/etc/$_pkgname" \
     --localstatedir=/var \
     --disable-static
  make -j1
}

check() {
  cd "$srcdir/OpenARC"
  make -j1 check
}

package() {
  cd "$srcdir/OpenARC"

  make -j1 DESTDIR="$pkgdir/" install

  # systemd integration
  install -Dm644 "$srcdir/$_pkgname.sysusers" "$pkgdir/usr/lib/sysusers.d/$_pkgname.conf"
  install -Dm644 "$srcdir/$_pkgname.tmpfiles" "$pkgdir/usr/lib/tmpfiles.d/$_pkgname.conf"
  install -Dm644 "$srcdir/$_pkgname.service" -t "$pkgdir/usr/lib/systemd/system"

  # license
  mkdir -p "$pkgdir/usr/share/licenses/$_pkgname"
  for f in LICENSE LICENSE.Sendmail; do
    ln -s $f "$pkgdir/usr/share/licenses/$_pkgname/$f"
  done
  ln -s "$pkgdir/usr/share/licenses/$_pkgname" "$pkgdir/usr/share/licenses/$pkgname"
}
