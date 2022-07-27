# Maintainer: Christian Krause ("wookietreiber") <christian.krause@mailbox.org>
# Maintainer: Alberto Ronzani ("albron") <alberto.ronzani@gmail.com>
# shellcheck disable=2034
# shellcheck disable=2148

pkgname=dfhack
pkgver=0.47.05
_pkgver=$pkgver-r5
pkgrel=7
pkgdesc="memory hacking library for Dwarf Fortress and a set of tools that use it"
arch=('x86_64' 'i686')
url="https://dfhack.readthedocs.io/en/stable/"
license=('custom')
depends=("dwarffortress=$pkgver" lua protobuf libpng12 libxrandr libjpeg6 freetype2 libglvnd libxcursor libxinerama libxcrypt-compat)
makedepends=('cmake' 'git' 'python-sphinx' 'perl-xml-libxml' 'perl-xml-libxslt')
conflicts=('dfhack-bin' 'dfhack-git')

source=("$pkgname::git+https://github.com/DFHack/dfhack#tag=$_pkgver"
        Wuninitialized.patch
        Wrestrict.patch
        dfhack.sh
        dfhack-run.sh)

md5sums=('SKIP'
         '25e320dd43d29f64c746b962b57cc145'
         'd9d015c118e4d7adcbf8b8ab5bf3f4d2'
         '81f5909c1a32391679f968e40f24d5ca'
         '3853c6f890d3541f710f2c4833a9e696')

prepare() {
  # shellcheck disable=2154
  cd "$srcdir"/$pkgname || exit 1

  git remote set-url origin https://github.com/DFHack/dfhack
  git submodule sync
  git submodule update --init

  patch --forward --strip=1 --input="${srcdir}/Wuninitialized.patch"
  patch --forward --strip=1 --input="${srcdir}/Wrestrict.patch"
}

build() {
  cd "$srcdir"/$pkgname/build || exit 1

  cmake \
    -DCMAKE_INSTALL_PREFIX=/opt/dwarffortress \
    -DCMAKE_BUILD_TYPE=Release \
    -DBUILD_DOCS=ON \
    -DBUILD_STONESENSE=ON \
    -DDFHACK_BUILD_ARCH=64 \
    ..

  make
}

package() {
  cd "$srcdir"/$pkgname/build || exit 1

  # shellcheck disable=2154
  make DESTDIR="$pkgdir" install

  install -Dm755 "$srcdir"/dfhack.sh     "$pkgdir"/usr/bin/dfhack
  install -Dm755 "$srcdir"/dfhack-run.sh "$pkgdir"/usr/bin/dfhack-run

  install -Dm644 ../LICENSE.rst "$pkgdir"/usr/share/licenses/$pkgname/LICENSE
}
