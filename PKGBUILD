# Contributor: Thomas Dziedzic < gostrc at gmail >
# Contributor: Michael Eckert <michael.eckert@linuxmail.org>

# Uncomment to enable Python 2 bindings
#_with_python2=1

pkgname=deltarpm
pkgver=3.6.5
pkgrel=1
pkgdesc="Create deltas between rpms"
arch=('i686' 'x86_64')
license=('BSD-3-Clause')
url="https://github.com/rpm-software-management/$pkgname"
depends=('rpm-tools' 'zlib' 'zstd>=1.3.8')
makedepends=('python')
((_with_python2)) && makedepends+=('python2')
optdepends=('perl: for drpmsync command'
            'python: for python3 module')
((_with_python2)) && optdepends+=('python2: for python2 module')
source=("$url/archive/$pkgver/$pkgname-$pkgver.tar.gz")
sha256sums=('f3fba9b96c55be21696466bbfc3b2a623a4fb35646ff9a135f5c29406b412f22')

PYTHONS='python'
((_with_python2)) && PYTHONS+=' python2'

build() {
	cd "$pkgname-$pkgver"

	make CPPFLAGS="$CPPFLAGS"     \
	     CFLAGS="$CFLAGS -fPIC -DWITH_ZSTD=1" \
	     LDFLAGS="$LDFLAGS"       \
	     PYTHONS="$PYTHONS"       \
	     prefix=/usr              \
	     zlibbundled=''           \
	     zlibldflags='-lz'        \
	     zlibcppflags=''          \
	     all python
}

package() {
	cd "$pkgname-$pkgver"

	make DESTDIR="$pkgdir/"       \
	     PYTHONS="$PYTHONS"       \
	     prefix=/usr              \
	     mandir=/usr/share/man    \
	     install

	install -Dp -m644 README      "$pkgdir/usr/share/doc/$pkgname/README"
	install -Dp -m644 LICENSE.BSD "$pkgdir/usr/share/licenses/$pkgname/LICENSE.BSD"
}

# vim: set ft=sh ts=4 sw=4 noet:
