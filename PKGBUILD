# Maintainer: Edmund Lodewijks <e.lodewijks at gmail.com>

pkgname=openarc
pkgver=1.2.0
pkgrel=1
pkgdesc="OpenARC with patches from not-yet-merged PRs - by flowerysong"
arch=(x86_64)
url="https://github.com/flowerysong/OpenARC"
license=('BSD-2-Clause' 'LicenseRef-Sendmail-1.1')
depends=('sh' 'glibc' 'jansson' 'openssl' 'libbsd' 'libidn2')
optdepends=('smtp-server: for using a local mail server'
	    'bind: required only for signature verification (alternatives available)')
makedepends=('git' 'python-miltertest' 'libmilter')
conflicts=('openarc' 'openarc-unofficial-patches-git')
#source=("git+https://github.com/flowerysong/OpenARC.git#tag=${pkgver}"
source=("https://github.com/flowerysong/OpenARC/releases/download/v${pkgver}/${pkgname}-${pkgver}.tar.gz"
        openarc.service
        openarc.sysusers
        openarc.tmpfiles)
backup=('etc/openarc/openarc.conf')
sha256sums=('450e989279e2eb3e2e2a1b754d9e6d4670abff58e3bed719d9175a03dfe2cec3'
            'd438b4a2e0ab5b247938213da7e8062fa5865e750e4f89d41471311edc163022'
            '31c399c0e3a69bb845b033ab5c0ad92d44cacb0fd58e0113cd1901e75900515e'
            'a27619fe3bbea2a0fd7c555851089722b1d67818bc014d1dce20620b5eb4bbc5')

prepare() {
  cd "$srcdir"/"${pkgname}-${pkgver}"
  autoreconf -i
}

build() {
  cd "$srcdir"/"${pkgname}-${pkgver}"
  ./configure \
     --prefix=/usr \
     --sbindir=/usr/bin \
     --sysconfdir="/etc/pkgname" \
     --localstatedir=/var \
     --disable-static
  make
}

check() {
  cd "$srcdir"/"${pkgname}-${pkgver}"
  make -j1 check
}

package() {
  cd "$srcdir"/"${pkgname}-${pkgver}"
  
  make -j1 DESTDIR="$pkgdir/" install
  
  # systemd integration
  install -Dm644 "$srcdir/$pkgname.sysusers" "$pkgdir/usr/lib/sysusers.d/$pkgname.conf"
  install -Dm644 "$srcdir/$pkgname.tmpfiles" "$pkgdir/usr/lib/tmpfiles.d/$pkgname.conf"
  install -Dm644 "$srcdir/$pkgname.service" -t "$pkgdir/usr/lib/systemd/system"

  # license
  mkdir -p "$pkgdir/usr/share/licenses/$pkgname"
  for f in LICENSE LICENSE.Sendmail; do
    ln -s $f "$pkgdir/usr/share/licenses/$pkgname/$f"
  done
}
