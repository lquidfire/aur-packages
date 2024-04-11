# Maintainer: Edmund Lodewijks <e.lodewijks at gmail.com>

pkgname=openarc
pkgver=1.0.0.Beta3
pkgrel=4
_commit=eb430dbdeee9f502295fe7a7d5041dfca3f00745
pkgdesc="Open source implementation of the ARC email authentication system."
arch=(x86_64)
url="https://github.com/trusteddomainproject/OpenARC"
license=('BSD-2-Clause' 'LicenseRef-Sendmail-1.1')
depends=('sh' 'glibc' 'jansson' 'openssl' 'libbsd')
optdepends=('smtp-server: for using a local mail server'
	    'bind: required only for signature verification (alternatives available)')
makedepends=('libmilter' 'git')
source=("git+https://github.com/trusteddomainproject/OpenARC.git#commit=${_commit}"
        configure.ac.patch
        openarc.service
        openarc.sysusers
        openarc.tmpfiles)
sha256sums=('51fe59502f4428e5516b31ef1e63f33bddc5b4fb9d0c7752a212ec1918b18145'
            '3df10339f7f83a9b4449ee13aaeb23f78f6ba75a49d363f80f2f757d0540b6d2'
            '14c3cce4d0b78b34c79eb3f50e6b48c9fa5aa2def11593b537190dfa46ed9536'
            '31c399c0e3a69bb845b033ab5c0ad92d44cacb0fd58e0113cd1901e75900515e'
            'db0f857ca6b3ef8e7210dc18b051dde26540e7ce70f611d72e2a16e6d2d71d3e')
validpgpkeys=(5CDD574C22FF4D2480ACABDF5254B96BC608B511) # The OpenDKIM Project <security@opendkim.org>

prepare() {
  cd "$srcdir/OpenARC"
  patch -p0 -i "$srcdir"/configure.ac.patch
  autoreconf -i
}

build() {
  cd "$srcdir/OpenARC"
  ./configure \
     --prefix=/usr \
     --sbindir=/usr/bin \
     --sysconfdir="/etc/$pkgname" \
     --localstatedir=/var \
     --with-privsep-user=openarc
  make
}

check() {
  cd "$srcdir/OpenARC"
  make -j1 check
}

package() {
  cd "$srcdir/OpenARC"
  
  make -j1 DESTDIR="$pkgdir/" install
  install -Dm644 "$srcdir/openarc.sysusers" "$pkgdir/usr/lib/sysusers.d/$pkgname.conf"
  install -Dm644 "$srcdir/openarc.tmpfiles" "$pkgdir/usr/lib/tmpfiles.d/$pkgname.conf"

  # License
  mkdir -p "$pkgdir/usr/share/licenses/$pkgname"
  for f in LICENSE LICENSE.Sendmail; do
    ln -s ../../doc/$pkgname/$f "$pkgdir/usr/share/licenses/$pkgname/$f"
  done


  install -Dm644 "$srcdir/$pkgname.service" "$pkgdir/usr/lib/systemd/system/$pkgname.service"

  rm -rf "$pkgdir"/var
}
