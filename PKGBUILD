# Maintainer: Felix Yan <felixonmars@archlinux.org>
# Contributor: Andrea Scarpino <andrea@archlinux.org>
# Contributor: Gavin Power <gjpower@mun.ca>

pkgname=qt5-wayland
pkgver=5.15.2+kde+r36
pkgrel=1
_commit=992833ca741efe8f533c61abfaf129a1d8bfcfee
arch=('x86_64')
url='https://www.qt.io'
license=('GPL3' 'LGPL3' 'FDL' 'custom')
pkgdesc='Provides APIs for Wayland'
depends=('qt5-declarative' 'libxcomposite')
makedepends=('vulkan-headers' 'git')
groups=('qt' 'qt5')
_pkgfqn=qtwayland
source=("git+https://invent.kde.org/qt/qt/$_pkgfqn#commit=$_commit" "373473.patch")
sha256sums=('SKIP'
            'fe7378cbd70fd7919662aeae3be0763d8983b9a792c2ba6c6a47ae207ecc3388')

pkgver() {
  cd $_pkgfqn
  echo "5.15.2+kde+r"`git rev-list --count origin/5.15.2..$_commit`
}

prepare() {
  mkdir -p build

  cd $_pkgfqn
  git revert -n 30cb2a87fcc6265232cb5a3ffce9836da6e531d6 # Revert version bump
  
  patch --forward --strip=2 --input="${srcdir}/373473.patch"
}

build() {
  cd build

  qmake ../${_pkgfqn}
  make
}

package() {
  cd build

  make INSTALL_ROOT="$pkgdir" install

  # Drop QMAKE_PRL_BUILD_DIR because reference the build dir
  find "$pkgdir/usr/lib" -type f -name '*.prl' \
    -exec sed -i -e '/^QMAKE_PRL_BUILD_DIR/d' {} \;

  install -d "$pkgdir"/usr/share/licenses
  ln -s /usr/share/licenses/qt5-base "$pkgdir"/usr/share/licenses/${pkgname}
}
