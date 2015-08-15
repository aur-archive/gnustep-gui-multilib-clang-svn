# Maintainer: X0rg

_svnname=gnustep-gui
pkgname=$_svnname-multilib-clang-svn
pkgver=38274
pkgrel=6
pkgdesc="The GNUstep GUI class library for multilib, using Clang"
arch=('x86_64')
url="http://www.gnustep.org/"
license=('GPL3' 'LGPL2.1')
groups=('gnustep-multilib-clang-svn')
depends=('libtiff' 'libjpeg-turbo' 'libpng' 'gnustep-base-multilib-clang-svn' \
         'lib32-libtiff' 'lib32-libjpeg-turbo' 'lib32-libpng')
makedepends=('svn' 'clang' 'gnustep-make-multilib-clang-svn')
optdepends=('lib32-giflib: enable libgif-based GIF support'
	'lib32-aspell: enable aspell for spellchecker'
	'flite: enable speech server'
	'lib32-libsndfile: enable sound (need lib32-libao to work)'
	'lib32-libao: enable sound (need lib32-libsndfile to work)'
	'lib32-libcups: enable cups printing support')
provides=('gnustep-gui-clang-svn')
conflicts=('gnustep-gui' 'gnustep-gui-svn' 'gnustep-gui-clang-svn')
options=('!emptydirs')
install=merge.install
source=("$_svnname::svn://svn.gna.org/svn/gnustep/libs/gui/trunk/")
md5sums=('SKIP')

pkgver() {
  cd "$srcdir/$_svnname"
  svnversion | tr -d [A-z]
}

prepare() {
  msg2 "Patch 'NSBitmapImageRep+PNG.m' file..."
  sed -i 's|png_sizeof|sizeof|g' "$srcdir/$_svnname/Source/NSBitmapImageRep+PNG.m"

  msg2 "Make a clone of $_svnname"
  cp -Rv "$srcdir/$_svnname" "$srcdir/$_svnname-32"
}

build() {
  # 64-bit build
  cd "$srcdir/$_svnname"
  msg2 "Run 'configure' (64-bit)..."
  OBJCFLAGS="-fblocks" CC="clang" CXX="clang++" ./configure --prefix=/usr --sysconfdir=/etc/GNUstep --enable-libgif

  msg2 "Run 'make' (64-bit)..."
  make

  # 32-bit build
  cd "$srcdir/$_svnname-32"
  source "/usr/share/GNUstep32/Makefiles/GNUstep.sh"
  export PKG_CONFIG_PATH="/usr/lib32/pkgconfig"

  msg2 "Run 'configure' (32-bit)..."
  OBJCFLAGS="-fblocks" CC="clang -m32" CXX="clang++ -m32" ./configure --prefix=/usr --libdir=/usr/lib32 --sysconfdir=/etc/GNUstep --enable-libgif

  msg2 "Run 'make' (32-bit)..."
  make
}

# check() {
#   cd "$srcdir/$_svnname"
#   make check
#
#   cd "$srcdir/$_svnname-32"
#   make check
# }

package() {
  # 64-bit install
  cd "$srcdir/$_svnname"
  msg2 "Install (64-bit)..."
  GNUSTEP_CONFIG_FILE="/etc/GNUstep/GNUstep.conf" make DESTDIR="$pkgdir" install

  # 32-bit install
  cd "$srcdir/$_svnname-32"
  msg2 "Install (32-bit)..."
  GNUSTEP_CONFIG_FILE="/etc/GNUstep/GNUstep32.conf" make DESTDIR="$pkgdir" install
}
