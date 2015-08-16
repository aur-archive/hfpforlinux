# Contributor: Jonas Heinrich <onny@project-insanity.org>
# Maintainer: Jonas Heinrich <onny@project-insanity.org>

pkgname=hfpforlinux
pkgver=2.04
pkgrel=7
pkgdesc="HFP for Linux is a Bluetooth Hands-Free Profile server"
url="http://nohands.sourceforge.net/"
arch=('i686' 'x86_64')
license=("GPL2")
depends=('bluez')
optdepends=('libtool' 'libsalsa' 'audiofile' 'dbus' 'qt3')
makedepends=('svn' 'speex')
conflicts=()
replaces=()
backup=()
build () {
	svn co https://nohands.svn.sourceforge.net/svnroot/nohands/trunk
	cd $srcdir/trunk
	./autogen.sh
	./configure
	sed -i 's/-D_FORTIFY_SOURCE=2 -Wall/-D_FORTIFY_SOURCE=2 -Wall -fpermissive/' libhfp/Makefile
	sed -i 's/-laudiofile   -lspeexdsp/-laudiofile   -lspeexdsp -lpthread/' test/Makefile
	sed -i 's/-laudiofile   -lspeexdsp/-laudiofile   -lspeexdsp -lpthread/' include/Makefile
	sed -i 's/-laudiofile   -lspeexdsp/-laudiofile   -lspeexdsp -lpthread/' qt/Makefile
	sed -i 's/-laudiofile   -lspeexdsp/-laudiofile   -lspeexdsp -lpthread/' hfpd/Makefile
	sed -i 's/-Wshadow -fno-exceptions/-Wshadow -fno-exceptions -fpermissive/' hfpd/Makefile
	sed -i 's/CXXFLAGS = -march=x86-64/CXXFLAGS = -fstrict-aliasing -fpermissive -march=x86-64/' qt/Makefile
	sed -i '810s/RegisterDirect/this->RegisterDirect/' include/libhfp/events.h
	sed -i '859s/Invoke/BaseT::Invoke/' include/libhfp/events.h
        sed -i 's/^LDFLAGS =/LDFLAGS = -lpthread/' hfpd/Makefile
	sed -i 's/^libhfp_LIBS =/libhfp_LIBS = -lpthread/' test/Makefile
	sed -i '1s/python/python2/' data/hfconsole.in
	make
}
package() {
	cd $srcdir/trunk
	make DESTDIR="$pkgdir" install || return 1
}
