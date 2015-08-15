# Maintainer: modprobe

pkgname=bitshares
pkgver=0.9.2
pkgrel=1
pkgdesc="A decentralized, P2P bank and exchange"
arch=('i686' 'x86_64')
url="https://github.com/BitShares/bitshares"
depends=('qt5-base' 'qt5-imageformats' 'qt5-webkit' 'qt5-tools' 'boost' 'zlib')
makedepends=('git' 'cmake' 'clang' 'nodejs' 'python2' 'npm')
conflicts=('bitshares-git')
source=("git://github.com/BitShares/bitshares.git" "BitShares.desktop")
sha256sums=('SKIP'
            'a51a3cc68105189304c45ad87fb8e30cdc399639ce48156e74be771abe447d4f')
_gitname="bitshares"
license=("Public Domain")

build() {
  cd "$srcdir/$_gitname"
  
  #Fetch submodules
  git checkout "bts/$pkgver"
  git submodule update --init --recursive
  
  #Prepare NodeJS web GUI dependencies
  cd programs/web_wallet
  npm install lineman
  npm install

  #Do the actual build
  cd -

  cd libraries/fc; git checkout master; git pull; cd -

  #Due to a bug in GCC 4.9.1 which prevents it from building BitShares, we must use clang instead.
  cmake -DCMAKE_C_COMPILER=/usr/bin/clang -DCMAKE_CXX_COMPILER=/usr/bin/clang++ -DCMAKE_BUILD_TYPE=Release -DINCLUDE_QT_WALLET=ON .
  make forcebuildweb
  make bitshares_client BitShares
}

package() {
  cd "$srcdir/$_gitname"

  #Do the actual install; it's really simple, just 4 files
  mkdir -p $pkgdir/usr/bin $pkgdir/usr/share/icons $pkgdir/usr/share/applications
  install -D -m644 $startdir/BitShares.desktop $pkgdir/usr/share/applications/BitShares.desktop
  install -D -m644 ./programs/qt_wallet/images/qtapp80.png $pkgdir/usr/share/icons/BitShares.png
  install -D -m755 ./programs/client/bitshares_client $pkgdir/usr/bin/bitshares-cli
  install -D -m755 ./programs/qt_wallet/bin/BitShares $pkgdir/usr/bin/bitshares-gui
}
