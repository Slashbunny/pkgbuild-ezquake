# Maintainer: Slash <demodevil5[at]yahoo[dot]com>

pkgname=ezquake
pkgver=3.6.4
pkgrel=2
pkgdesc="One of the most Popular QuakeWorld clients for Linux/BSD/OSX/Win32. You need the retail pak files to play."
url="https://www.ezquake.com/"
license=('GPL-2.0-only')
depends=('curl' 'expat' 'jansson' 'libjpeg-turbo' 'libpng' 'minizip' 'openssl' 'sdl2' 'speex')
makedepends=('unzip' 'vim')
conflicts=('ezquake-git' 'fuhquake')
provides=('quake' 'fuhquake')
arch=('x86_64')
install=ezquake.install
source=("https://github.com/QW-Group/ezquake-source/releases/download/${pkgver}/ezquake-source_with-submodules-${pkgver}.zip"
'https://github.com/QW-Group/ezquake-source/releases/download/3.2.3/ezquake-ubuntu-3.2.3-full.tar.gz'
'ezquake.launcher' 'ezquake.desktop' 'ezquake.ico')
noextract=("ezquake-ubuntu-3.2.3-full.tar.gz")
sha256sums=('925d26b6441dc2bdb69307b9616a6b4a4647aa1c9443134daabf20433e848848'
            'd58f26ed912166615420f0d0b208a10fd2539a84b90e85edfcb1aedc94615af5'
            'aa59da4a296a43af8ea8c5670cef5980a15407124b3e53f3cf805ceb6126e6ed'
            'e92b9cdeac5eadced50a6167eb53b1343b0772d3bf8afa310eb281b88bf7e677'
            '2a6a5484ddb4cfaf8518b51df39ffd1fa8ce768402eab6401415bececb8e8ab2')

prepare() {
    cd "${srcdir}/ezquake-source-${pkgver}/"

    # pcre2 fix
    # @see https://github.com/QW-Group/ezquake-source/issues/916
    /usr/bin/sed -i '181s/const //' src/sv_mod_frags.c
}

build() {
    cd "${srcdir}/ezquake-source-${pkgver}/"

    # Compile ezquake
    make
}

package() {
    cd "${srcdir}"

    # Create Destination Directories
    install -d "${pkgdir}/opt/quake"

    # Unpack ezQuake assets package (base)
    bsdtar -x -o -C "${pkgdir}/opt/quake" -f "${srcdir}/ezquake-ubuntu-3.2.3-full.tar.gz"

    # Clean up permissions in assets package
    find "${pkgdir}/opt/quake" -type d -exec chmod 0755 "{}" \;
    find "${pkgdir}/opt/quake" -type f -exec chmod 0644 "{}" \;

    # Overwrite packaged binary with compiled one
    mv -v "${srcdir}/ezquake-source-${pkgver}/ezquake-linux-x86_64" \
        "${pkgdir}/opt/quake/"

    # Make id1 Directory for pak0.pak and pak1.pak files
    install -d "${pkgdir}/opt/quake/id1/"

    # Make save game Directory with user group permissions
    install -d -m775 -g users "${pkgdir}/opt/quake/qw/save/"

    # Install Icon
    install -D -m644 "${srcdir}/ezquake.ico" \
        "${pkgdir}/usr/share/pixmaps/ezquake.ico"

    # Install Launcher
    install -D -m755 "${srcdir}/ezquake.launcher" \
        "${pkgdir}/usr/bin/ezquake"

    # Install Desktop
    install -D -m644 "${srcdir}/ezquake.desktop" \
        "${pkgdir}/usr/share/applications/ezquake.desktop"
}

