# Maintainer: Tom Wambold <tom5760@gmail.com>
# Contributor: Antonin Décimo <antonin dot decimo at gmail dot com>
# Contributor: Markus Sommer <markus@splork.de>
pkgname=core
pkgver=8.1.0
pkgrel=2
pkgdesc="Common Open Research Emulator"
arch=('i686' 'x86_64')
url="https://github.com/coreemu/core/"
license=('BSD')
# 'python-certifi' should' be a dependency of 'python-pyproj',
# but the corresponding bug report seems to be orphaned
depends=('ebtables' 'ethtool' 'fabric' 'iproute2' 'nftables' 'libev' 'python'
         'python-grpcio' 'python-invoke' 'python-lxml' 'python-mako'
         'python-netaddr' 'python-pillow' 'python-protobuf' 'python-pyproj'
         'python-yaml' 'python-certifi' 'python-decorator')
makedepends=('help2man' 'imagemagick' 'python-dephell' 'python-grpcio-tools'
             'python-setuptools')
optdepends=('openvswitch: Open vSwitch SDN support'
            'tk: Legacy Tk-based GUI'
            'tkimg: Thumbnail support in Tcl/Tk GUI'
            'emane: Support for heterogeneous network emulation'
            'mgen: Traffic generation')
backup=('etc/core/core.conf'
        'etc/core/logging.conf')
source=("https://github.com/coreemu/core/archive/release-$pkgver.tar.gz"
        'core-daemon.service')
sha512sums=('781d76c4af539c63a883b0ba696eee492caf8ba4d546adfceb8d97ee52b3f04ad39a2b49d82729d230b25a1555dc151cb358cc2b9bda3a2b4e6d5345412aeefb'
            'e56f65a68804b0c7534d54fa116b53abe6922fb0aae13ee1073f76c0c7972b4832d12665d135159f7241d0f39d070ef510b4a7f05978118b6f00d737fda8dd46')

prepare() {
  cd "$srcdir/core-release-$pkgver/daemon"

  dephell deps convert --from pyproject.toml --to setup.py
}

build() {
  cd "$srcdir/core-release-$pkgver"

  ./bootstrap.sh
  ./configure --prefix=/usr
  make

  cd daemon
  python setup.py build
}

package() {
  cd "$srcdir/core-release-$pkgver"
  make DESTDIR="$pkgdir/" install

  cd daemon
  python setup.py install --root="$pkgdir" --optimize=1 --skip-build

  mkdir -p "$pkgdir/usr/bin"
  cp -r scripts/* "$pkgdir/usr/bin"

  mkdir -p "$pkgdir/usr/share/core"
  cp -r examples "$pkgdir/usr/share/core"

  install -D -m 0644 "data/core.conf" "$pkgdir/etc/core/core.conf"
  install -D -m 0644 "data/logging.conf" "$pkgdir/etc/core/logging.conf"
  install -D -m 0644 "$srcdir/core-daemon.service" "$pkgdir/usr/lib/systemd/system/core-daemon.service"
  install -D -m 0644 "$srcdir/core-release-$pkgver/LICENSE" "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}

# vim:set ts=2 sw=2 et:
