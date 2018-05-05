# Contributor: Patrick Jackson <PatrickSJackson gmail com>
# Maintainer: Christoph Vigano <mail@cvigano.de>

pkgname=st
pkgver=0.8.1
pkgrel=1
pkgdesc='A simple virtual terminal emulator for X.'
arch=('i686' 'x86_64')
license=('MIT')
depends=('libxft' 'libxext' 'xorg-fonts-misc')
makedepends=('ncurses')
url="http://st.suckless.org"
source=(http://dl.suckless.org/st/$pkgname-$pkgver.tar.gz
# config.h
st-scrollback-0.8.diff
st-scrollback-mouse-0.8.diff
st-xresources-20180309-c5ba9c0.diff
st-fix-keyboard-input-20180420.diff
https://st.suckless.org/patches/vertcenter/st-vertcenter-20180320-6ac8c8a.diff
)
sha256sums=('c4fb0fe2b8d2d3bd5e72763e80a8ae05b7d44dbac8f8e3bb18ef0161c7266926'
# 'bed7977c855f02e3968a754e813015e4214b52102e3c54712d8a52245bcceeec'
'SKIP'
'SKIP'
'SKIP'
'SKIP'
'SKIP'
)

prepare() {
  cd $srcdir/$pkgname-$pkgver
  # skip terminfo which conflicts with nsurses
  sed \
    -e '/char font/s/= .*/= "Fixed:pixelsize=13:style=SemiCondensed";/' \
    -e '/char worddelimiters/s/= .*/= " '"'"'`\\\"()[]{}<>|";/' \
    -e '/int defaultcs/s/= .*/= 1;/' \
    -i config.def.h
  sed \
    -e 's/CPPFLAGS =/CPPFLAGS +=/g' \
    -e 's/CFLAGS =/CFLAGS +=/g' \
    -e 's/LDFLAGS =/LDFLAGS +=/g' \
    -e 's/_BSD_SOURCE/_DEFAULT_SOURCE/' \
    -i config.mk
  sed '/@tic/d' -i Makefile
  for file in "${source[@]}"; do
    if [[ "$file" == "config.h" ]]; then
      # add config.h if present in source array
      # Note: this supersedes the above sed to config.def.h
      true
      # cp "$srcdir/$file" .
    elif [[ "$file" == *.diff || "$file" == *.patch ]]; then
      # add all patches present in source array
      patch -Np1 <"$srcdir/$(basename ${file})"
    fi
  done
}

build() {
  cd $srcdir/$pkgname-$pkgver
  make X11INC=/usr/include/X11 X11LIB=/usr/lib/X11
}

package() {
  cd $srcdir/$pkgname-$pkgver
  make PREFIX=/usr DESTDIR="$pkgdir" TERMINFO="$pkgdir/usr/share/terminfo" install
  install -Dm644 LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
  install -Dm644 README "$pkgdir/usr/share/doc/$pkgname/README"
}
