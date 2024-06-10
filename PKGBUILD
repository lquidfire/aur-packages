# Maintainer: Edmund Lodewijks <e.lodewijks at gmail.com>

pkgname=openarc
pkgver=1.0.0.Beta3
pkgrel=8
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
	openarc-headerdebug.patch
	0001-Remove-t-from-the-list-of-required-AS-tags.patch
        openarc.service
        openarc.sysusers
        openarc.tmpfiles)
sha256sums=('51fe59502f4428e5516b31ef1e63f33bddc5b4fb9d0c7752a212ec1918b18145'
            '21f6bacae998c8d206ffcd59d9b2c91c8596b1f908cf074df3941ae0134f39ca'
            '484dd6330972edd903c9a6f65ec044b886afbe92606a43b29347c3198cc4b0ec'
            '7a41b393aa02adb9f258fde0ec55a0a968fb5453d2803a36f083944d9b3f539c'
            '64fcea18737a1d8acc7d7a9c6811bab8419a66ceff349fa2144010d8f6fd6c57'
            '31c399c0e3a69bb845b033ab5c0ad92d44cacb0fd58e0113cd1901e75900515e'
            '1072aa91e24e866e5997891808b9169512f3a6b164b2e517b6d3223db93e60ba')
validpgpkeys=(5CDD574C22FF4D2480ACABDF5254B96BC608B511) # The OpenDKIM Project <security@opendkim.org>

prepare() {
  cd "$srcdir/OpenARC"
  patch -p0 -i "$srcdir"/configure.ac.patch
  patch -p0 -i "$srcdir"/0001-Remove-t-from-the-list-of-required-AS-tags.patch
  patch -p0 -i "$srcdir"/openarc-headerdebug.patch
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
