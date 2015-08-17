# build using 'makepkg --holdver'

# Maintainer: Christoph Brill <egore911@egore911.de>
pkgname=ogreopcode-svn
pkgver=417
pkgrel=2
pkgdesc="collision detection library for Ogre based on OPCODE"
arch=('i686' 'x86_64')
url="http://www.jacmoe.dk/forum/"
license=('LGPL2.1')
depends=('ogre>=1.8.0')
makedepends=('subversion')
provides=('ogreopcode')
conflicts=('ogreopcode-git')
options=('!libtool')
source=('ogreopcode-417-ogre-1.8.0.patch' 'ogreopcode-417-autotools.patch')
md5sums=('d7dd5ae0d183031a9428f572e30592ba' 'a30bea9ce07cbe5c0463f266b1518496')

_svntrunk=https://ogreconglo.svn.sourceforge.net/svnroot/ogreconglo/ogreopcode/trunk/
_svnmod=trunk

build() {
  cd "$srcdir"
  msg "Connecting to SVN server...."

  if [[ -d "$_svnmod/.svn" ]]; then
    (cd "$_svnmod" && svn up -r "$pkgver")
  else
    svn co "$_svntrunk" --config-dir ./ -r "$pkgver" "$_svnmod"
  fi

  msg "SVN checkout done or server timeout"
  msg "Starting build..."

  rm -rf "$srcdir/$_svnmod-build"
  svn export "$srcdir/$_svnmod" "$srcdir/$_svnmod-build"
  cd "$srcdir/$_svnmod-build"

  #
  # BUILD HERE
  #
  msg "Bootstrapping"
  touch NEWS README AUTHORS ChangeLog
  patch -p0 < ${srcdir}/ogreopcode-${pkgver}-autotools.patch || exit 1
  msg "Patching for OGRE 1.8 compatibility"
  patch -p0 --binary < ${srcdir}/ogreopcode-${pkgver}-ogre-1.8.0.patch || exit 1
  autoreconf -i
  ./configure --prefix=/usr
  make
}

package() {
  cd "$srcdir/$_svnmod-build"
  make DESTDIR="$pkgdir/" install
}

# vim:set ts=2 sw=2 et:
