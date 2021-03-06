# Maintainer: Albert I <kras@raphielgang.org>

pkgname=megacmd-dynamic
pkgver=1.4.0
_sdkver=3.7.3c
pkgrel=2
pkgdesc='MEGA Command Line Interactive and Scriptable Application (dynamically linked version)'
arch=(x86_64)
url='https://mega.nz/cmd'
license=(BSD-2-Clause GPL3)
depends=(c-ares crypto++ ffmpeg freeimage libsodium libuv mediainfo sqlite zlib)
optdepends=('bash-completion: for completion script')
provides=("megacmd=$pkgver" "mega-sdk=$_sdkver")
conflicts=(megacmd mega-sdk)
source=(
    git+https://github.com/meganz/MEGAcmd.git#commit=4fc0787c44d1894a476355c147ad8c207333fe94
    git+https://github.com/meganz/sdk.git#commit=2337aca38daaca6deedd04d8ea400293503f00ff
)
b2sums=(
    SKIP
    SKIP
)

prepare() {
    cd MEGAcmd

    # init SDK
    git submodule init sdk
    git config submodule.sdk.url ../sdk
    git submodule update sdk

    ./autogen.sh
}

build() {
    cd MEGAcmd

    CPPFLAGS="$CPPFLAGS -DREQUIRE_HAVE_FFMPEG -DREQUIRE_HAVE_LIBUV -DREQUIRE_USE_MEDIAINFO -DREQUIRE_USE_PCRE" \
    ./configure --prefix=/usr \
        --disable-examples \
        --without-libraw \
        --with-cares \
        --with-cryptopp \
        --with-curl \
        --with-freeimage \
        --with-libmediainfo \
        --with-libsodium \
        --with-libuv \
        --with-libzen \
        --with-pcre \
        --with-readline

    make
}

package() {
    make -C MEGAcmd DESTDIR="$pkgdir" install
    # the Makefile installs bash completion scripts at wrong path
    mv "$pkgdir"/{usr/,}etc
    install -Dm644 MEGAcmd/LICENSE "$pkgdir"/usr/share/licenses/$pkgname/LICENSE
}
