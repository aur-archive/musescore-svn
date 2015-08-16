# Contributor: Dr.Egg <hondoheal@gmail.com>
# Maintainer: Stefan Husmann <stefan-husmann@t-online.de>
# Maintainer: Yichao Yu <yyc1992@gmail.com>

pkgname=musescore-svn
pkgver=5599
pkgrel=1
pkgdesc="A music score editor - svn-version"
arch=('i686' 'x86_64')
url="http://www.musescore.org/en/"
license=('GPL')
depends=('qt' 'qtwebkit' 'libsndfile')
makedepends=('portaudio' 'cmake' 'doxygen' 'subversion')
optdepends=('jack')
conflicts=('musescore' 'mscore')
source=('paths1.patch' 'paths2.patch' 'mime.xml')
md5sums=('1998017cd636df6593d674073e2b90d3'
         '9714e4897e0e4a033f84dc8e1aaabe1f'
         '969696178e56de36f9af37d7da61baaa')
install=musescore.install
_svntrunk="https://mscore.svn.sourceforge.net/svnroot/mscore/trunk"
_svnmod=$pkgname

build() {
  cd $srcdir
  msg "Connecting to SVN server...."
  if [ -d $_svnmod/.svn ]; then
    (cd $_svnmod && svn up -r $pkgver)
  else
    svn co $_svntrunk --config-dir ./ -r $pkgver $_svnmod
  fi

  msg "SVN checkout done or server timeout"
  msg "Starting make..."
  rm -rf $_svnmod-build
  svn export $_svnmod $_svnmod-build
  #
  # BUILD
  #
  cd $_svnmod-build
  patch -p1 < $srcdir/paths1.patch
  patch -p1 < $srcdir/paths2.patch
  # patch -p1 < $srcdir/no_qtscriptgen.patch

  cd $srcdir/${_svnmod}-build
  install -d $pkgdir/usr/share/doc/$pkgname
  make PREFIX=/usr release
}
package() {
  cd $srcdir/${_svnmod}-build/build.release
  make DESTDIR="$pkgdir" install
  install -Dm644 $srcdir/mime.xml \
    $pkgdir/usr/share/mime/packages/x-musescore.xml
  cd $pkgdir/usr/bin
  ln -s mscore musescore
}
