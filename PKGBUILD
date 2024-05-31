# Maintainer: Edmund Lodewijks <e.lodewijks@gmail.com>
# Contributor: Jerome Leclanche <jerome@leclan.ch>
# Previous contributors:
# William Gathoye, Emil Vanherp, Alad Wenter, Xavier D., Valere Monseur
# Troubleshooting: https://eid.belgium.be/en/faq/google-chromechromium-how-do-i-log
# Test if login works: https://iamapps.belgium.be/tma/

pkgname=eid-mw
pkgver=5.1.19
pkgrel=1
pkgdesc="The Belgian e-ID (electronic identity card) viewer and Firefox extension"
arch=("x86_64")
url="https://eid.belgium.be/"
license=("LGPL-3.0-only")
depends=("gtk3" "libproxy" "curl" "libbsd")
makedepends=("autoconf-archive" "pcsclite")
optdepends=(
    "firefox: Extension for Belgian eid"
    "acsccid: ACS CCID smart card readers"
    "ccid: Needed for Belgian Digipass 870"
    "pcsc-tools: PC/SC smartcard tools"
)
source=(
    "https://dist.eid.belgium.be/continuous/sources/$pkgname-$pkgver-v$pkgver.tar.gz"
    "https://dist.eid.belgium.be/continuous/sources/$pkgname-$pkgver-v$pkgver.tar.gz.asc"
)
sha256sums=('fc38d298cd2295f1db0043c8c80f53370fa3ee319041831d67b9b4ee593c141f'
            'SKIP')
#    Upstream only signs the "continuous releases" of the software, so that is the version
#    we are using.. 
#    The following key is taken from https://files.eid.belgium.be/info.html.
validpgpkeys=("D95426E309C0492990D8E8E2824A5E0010A04D46")

#prepare() {
#    # This optional autoreconf command could help prevent malware. Uncomment if wanted.
     # The makedepends "autoconf-archive" is required for this.
#    cd "${pkgname}-${pkgver}-v${pkgver}"
#    NOCONFIGURE=1 autoreconf -vfi
#}

build() {
    cd "$pkgname-$pkgver-v$pkgver"
    sed -i "s/c_rehash/openssl rehash/g" plugins_tools/eid-viewer/Makefile.in
    SSL_PREFIX=/usr ./configure --prefix=/usr --libexecdir=/usr/bin --sysconfdir=/etc
    make
}

package() {
    cd "$pkgname-$pkgver-v$pkgver"
    make install DESTDIR="$pkgdir"
}
