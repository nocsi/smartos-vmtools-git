# Maintainer: Loc Nguyen <l@nocsi.com>

pkgname=smartos-vmtools
_pkgname=smartos-vmtools
_branch=master
pkgver=20161123
pkgrel=1
pkgdesc="Joyent SmartOS vmtools"
arch=('any')
url="https://github.com/joyent/smartos-vmtools"
license=('Joyent')
depends=('pacman')
makedepends=('git' 'perl')
provides=('smartos-vmtools')

md5sums=('SKIP')
source=("smartos-vmtools::https://github.com/nocsigroup/smartos-vmtools.git")


_gitroot="https://github.com/nocsigroup/smartos-vmtools.git"
_gitname="smartos-vmtools"

build() {
 	cd "$srcdir"
	if [ -d $_gitname ] ; then
		cd $_gitname && git pull origin
		msg "The local files are updated."
	else
		git clone --depth=1 $_gitroot $_gitname
	fi
	msg "GIT checkout done or server timeout"
	rm -rf "$srcdir/$_gitname-build" 
	mkdir "$srcdir/$_gitname-build"
	cd "$srcdir/$_gitname" && ls -A | grep -v .git | xargs -d '\n' cp -r -t ../$_gitname-build # do not copy over the .git folder
	cd "$srcdir/$_gitname-build"
}

pkgver() {
	cd ${srcdir}/${pkgname}
	printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}
package() {
	cd "$srcdir/$_gitname-build/src/linux/lib/smartdc"
	for file in *
	do
		#Correct the paths in the script files
		sed 's|/lib/smartdc|/usr/lib/smartdc|' $file > $file.tmp
		mv  $file.tmp $file
		install -D $file $pkgdir/usr/lib/smartdc/$file 
	done
  
	cd "$srcdir/$_gitname-build/src/linux/lib/systemd/system"
	for file in *.service
	do
		#Correct the paths in the script files
		sed 's|/lib/smartdc|/usr/lib/smartdc|' $file > $file.tmp
		mv  $file.tmp $file
		install -D $file $pkgdir/usr/lib/systemd/system/$file
	done
}