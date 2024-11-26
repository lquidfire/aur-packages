# Maintainer: Edmund Lodewijks <e.lodewijks @ gmail.com>
# Contributor: George Rawlinson <grawlinson@archlinux.org>
# Contributor: Gaetan Bisson <bisson@archlinux.org>
# Contributor: Sergej Pupykin <pupykin.s+arch@gmail.com>
# Contributor: mutantmonkey <mutantmonkey@gmail.com>

_origname=sendmail # libmilter is bundled with the sendmail source
_pkgname=libmilter
pkgname=libmilter-sharedlib
pkgver=8.18.1
pkgrel=1
pkgdesc='Implementation of the sendmail Mail Filter API'
url='https://www.proofpoint.com/us/sendmail-open-source'
arch=('x86_64')
license=('Sendmail')
provides=('libmilter')
conflicts=('libmilter')
# Signing keys sourced from upstream. Current version is signed with the 2024 keys.
# https://www.proofpoint.com/us/products/email-protection/open-source-email-solution
validpgpkeys=('8AB063D7A4C5939DA9C01E38C4065A87C71F6844')
source=(
  "https://ftp.sendmail.org/$_origname.$pkgver.tar.gz"{,.sig}
  'fd-passing-libmilter.patch'
  'site.config.m4'
  'Patch002-dot-a-to-dot-so-in-devtools-m4-unix-sharedlibrary-m4.patch'
  'Patch003-libsharedmilter-Makefile-m4.patch'
)
sha512sums=('9ce713b44439d4de6faa9e3cdfa2226b44b4fbeb352a5f81584c062570e9472da244158287e489aabe258d28fe54ca4964565c7b0adc7e1763d212be42f98061'
            'SKIP'
            '438dfb94d4b884a08abbb20849a8b309b251b9d48c098575e67603d9d4d23d8ac799287cedd975b8aae61c550b987a9bf8dd7c9343ed289185b7e2ca72cbc82a'
            'da943d9c5b5a1c1eef91ede1f5c0e669ef493413e14a8c69655f9cc3bfe569c3c519ca1de823001521e755cbe678ca91cd7a5decbafaf257a7c58cf470de9b77'
            '18636b24e9fcb0d09b17e823203c5f0f89f50e6d95a03f4d1d0870910a2fe292a61676ec53e28a8b59cb49a7e0e6167ca98963231b89cef69f83db3cf2f75640'
            '3e6d5b31a586370a4ff7637f4366ae84998945c095ab7505acc7e5e4f5957c51d19a612ad7a6bac3970db011377c344e96ec788e1d73f1ea983612901fa9b71a')
b2sums=('3afa36073fd611c7fdb43ef0ab9f02d5fb8ae388e9471bdc7275c6c9dcee0a654f46ddef505b70e978cb1b818b0da375250678e501676d8bace534d59ee40d90'
        'SKIP'
        'ea2f1811666ce1b2c7532794845de9ec1f1e72d6c58a02c4c5800e93359c1c1cd4a0353fee572c258c378b0fea776d03ba19d794da7ed3295d9432b47ceb2481'
        'b03b731571a91d59744a651370ed8db9f035974bb559d1ed1a618356eef244462f1c922db343512cea6021475b872b03cb07e4a63ce5e0a4125b47c064134fcf'
        '507e897570e2c55424e27467b50adcad569e649a8fe5005ccdeea70697941ef6b0977fbb3975b401055e4b774e89862a5855b03998aa8229845056704b3591a9'
        'aea14561fb41c1e4e714ac13e10632150d013e21eaecba2de6dc0b82839fdde889edf45d4f1f25bb2c7563a3c66aedea621ddbe22d0d3c18a5f6725c382bd8a6')

prepare() {
  cd "$_origname-$pkgver"
  patch -p1 -i ../fd-passing-libmilter.patch # FS#49421

  # Enable local Archlinux CFLAGS and LDFLAGS.
  cp ../site.config.m4 devtools/Site
  sed -i "s/@@CCOPTS@@/${CFLAGS}/g;s/@@LDOPTS@@/${LDFLAGS}/g" devtools/Site/site.config.m4

  # Files for libmilter shared object (.so)
  cp -r libmilter libsharedmilter
  cp devtools/M4/UNIX/{,shared}library.m4
  patch -p1 -i ../Patch002-dot-a-to-dot-so-in-devtools-m4-unix-sharedlibrary-m4.patch
  sed -i "s/library/sharedlibrary/g" libsharedmilter/Makefile.m4
  patch -p1 -i ../Patch003-libsharedmilter-Makefile-m4.patch
}

build() {
# This builds `libmilter.so', not `libmilter.a'.
# If you want to (also) build `libmilter.a', (also) use the following
# directory in the "./Build" functions in build() and package():
#
#   "$_origname-$pkgver/$_pkgname"
#
  cd "$_origname-$pkgver/libsharedmilter"
  ./Build
}

package() {
  cd "$_origname-$pkgver/libsharedmilter"

  # create install directory
  install -vd "$pkgdir/usr/lib"
  ./Build DESTDIR="$pkgdir" install

  # license
  install -vDm644 -t "$pkgdir/usr/share/licenses/$_pkgname" ../LICENSE

  # documentation
  install -vDm644 -t "$pkgdir/usr/share/doc/$_pkgname" ../README
  install -vDm644 -t "$pkgdir/usr/share/doc/$_pkgname" docs/*

  # correct permissions
  chown -R root:root "$pkgdir"
}
