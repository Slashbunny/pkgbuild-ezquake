# Maintainer: Slash <demodevil5[at]yahoo[dot]com>

pkgname=ezquake
pkgver=3.6.9
pkgrel=1
pkgdesc="One of the most Popular QuakeWorld clients for Linux/BSD/OSX/Win32. You need the retail pak files to play."
url="https://ezquake.com/"
license=('GPL-2.0-only')
depends=('curl' 'expat' 'jansson' 'libjpeg-turbo' 'libpng' 'minizip' 'openssl' 'sdl2' 'speex' 'speexdsp' 'libsndfile' 'pcre2' 'freetype2')
makedepends=('cmake' 'ninja')
conflicts=('ezquake-git' 'fuhquake')
provides=('quake' 'fuhquake')
arch=('x86_64')
install=ezquake.install
source=("https://github.com/QW-Group/ezquake-source/releases/download/${pkgver}/ezquake-source-${pkgver}.tar.gz"
'https://github.com/QW-Group/ezquake-source/releases/download/3.2.3/ezquake-ubuntu-3.2.3-full.tar.gz'
'ezquake.launcher' 'ezquake.desktop' 'ezquake.ico')
noextract=("ezquake-ubuntu-3.2.3-full.tar.gz")
b2sums=('295bab572c927cf5819203f800320b15fe7a77511355b3c12b4c50f0396620a5e7249941078bc663500bc999a943bcc811015fe6be96da7e1b96a8d11c06a569'
        '98840842ea783d6fe99081425fddc69cb5a1009ac43bbe8815f6ee6fe3365a8ea08b75dff9e48f6eda9d47c50b45bba53e879c8b0f63a949170c0b1e419710ae'
        '2913e1a5cd3beed9858ba5762b6da170fc4ddc7ce429d78d029d8b45d17b9860144a15835742822f955d45329679ec1e9d99543dd130148c6d1abc46f7321d5c'
        '1e1519b43c9ff3d71c0e71f679a02201848e0ffae7d668e20917ecd71c3007fe4b2b0b17edcb65e9d6f7780a97257fafcf3363e4d576249dd3afd592fb053a78'
        '45640963258bd9abc7230229b2bbd2fb62ebc09650bd58371f2bb74168f6346d8ead508b8c7d92bc1960b8ed9bf6f922c2036e93ad9a2cd1551ce9d32dcbb425')

build() {
    cd "${srcdir}/ezquake-source-${pkgver}/"

    # Commands from upstream build-linux.sh
    cmake --preset dynamic -DCMAKE_C_FLAGS="-I/usr/include/minizip"
    cmake --build build-dynamic --config Release
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
    mv -v "${srcdir}/ezquake-source-${pkgver}/build-dynamic/Release/ezquake-linux-x86_64" \
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
