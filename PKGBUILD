# Maintainer: Alexander <alexanderypema [at] gmail [dot] com>

#original maintainers who I stole this entire thing from except for my github repo. Thanks guys! I hope you're okay with it <3
## Maintainer: eolianoe <eolianoe [at] gmail [DoT] com>
## Contributor: Edvinas Valatka <edacval@gmail.com>
## Contributor: Aaron Lindsay <aaron@aclindsay.com>

pkgname=seafile
pkgver=7.0.1
pkgrel=1
pkgdesc="Seafile is an online file storage and collaboration tool"
arch=('i686' 'x86_64' 'armv7h' 'armv6h' 'aarch64')
url="https://github.com/xarinatan/seafile-symlink"
license=('GPL2')
depends=("ccnet-server" "libsearpc" "libevent"
         "fuse" "python2" "sqlite")
makedepends=("vala" "intltool")
conflicts=("seafile-server")
source=("${pkgname}-master.tar.gz::${url}/archive/master.tar.gz"
        "libseafile.in.patch"
        "seaf-cli@.service")
sha256sums=('SKIP'
            'a2d7f7cf0c59aba97650af62b3cefd0ceb71a1007c34d9369a88e5769c7f6076'
            'c37510109c1de64c774896df39aece240c056b54414d2119fca01860211156ba')
provides=('seafile-client-cli')

prepare () {
  cd "$srcdir/seafile-symlink-master"

  patch -p1 -i "${srcdir}/libseafile.in.patch"

  # Fix all script's python 2 requirement
  grep -s -l -r '#!/usr/bin/env python' "${srcdir}/seafile-symlink-master" \
    | xargs sed -i -e 's|#!/usr/bin/env python|#!/usr/bin/env python2|g'
}

build() {
  cd "$srcdir/seafile-symlink-master"
  
  ./autogen.sh

  ./configure \
    --enable-console \
    --prefix=/usr \
    PYTHON=/usr/bin/python2

  make
}

package() {
  cd "$srcdir/seafile-symlink-master"
  
  make DESTDIR="${pkgdir}" install
  install -Dm644 "${srcdir}"/seaf-cli@.service "${pkgdir}"/usr/lib/systemd/system/seaf-cli@.service
}
