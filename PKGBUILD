# Maintainer: Krzysztof Wloch <wloszekk [at] gmail [dot] com>
pkgname=wiibafu-hg
pkgver=301
pkgrel=3
pkgdesc="The complete and simple to use backup solution for your Wii games"
arch=(any)
url="http://sourceforge.net/p/wiibafu/"
license=('GPLv3')
groups=()
depends=('qt4' 'wit')
makedepends=('mercurial')
provides=()
conflicts=()
replaces=()
backup=()
options=()
install=
source=()
noextract=()
md5sums=() #generate with 'makepkg -g'

__hgroot=http://hg.code.sf.net/p/wiibafu/code
__hgrepo=wiibafu-code

build() {
  cd "$srcdir"
  msg "Connecting to Mercurial server...."

  if [[ -d "$__hgrepo" ]]; then
    cd "$__hgrepo"
    hg pull -u
    msg "The local files are updated."
  else
    hg clone "$__hgroot" "$__hgrepo"
  fi

  msg "Mercurial checkout done or server timeout"
  msg "Starting build..."

  rm -rf "$srcdir/$__hgrepo-build"
  cp -r "$srcdir/$__hgrepo" "$srcdir/$__hgrepo-build"
  cd "$srcdir/$__hgrepo-build"
  sed -i 's/Exec=WiiBaFu/Exec=wiibafu/g' $srcdir/$__hgrepo-build/resources/WiiBaFu.desktop
  qmake-qt4
  make
}

package() {
  cd "$srcdir/$__hgrepo-build"
  mkdir -p $pkgdir/usr/bin
  install -m 755 bin/WiiBaFu $pkgdir/usr/bin/
  mkdir -p $pkgdir/usr/share/applications
  install -Dm644 "$srcdir/$__hgrepo-build/resources/WiiBaFu.desktop" "$pkgdir/usr/share/applications/WiiBaFu.desktop"
  mkdir -p $pkgdir/usr/share/icons
  install -Dm644 "$srcdir/$__hgrepo-build/resources/images/appicon.png" "$pkgdir/usr/share/icons/WiiBaFu.png"
  echo -e '#!'"/bin/bash\n\nPATH=/usr/share/wit:$PATH WiiBaFu\n" > wiibafu
  chmod +x wiibafu
  install -m 755 wiibafu $pkgdir/usr/bin/
}

# vim:set ts=2 sw=2 et:
