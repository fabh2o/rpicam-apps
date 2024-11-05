# Maintainer: Fabrice Monasterio <arch@fabricemonasterio.dev>

pkgname=rpicam-apps
pkgver=1.5.2
pkgrel=1
pkgdesc="Small suite of libcamera-based applications to drive the cameras on a Raspberry Pi platform."
arch=(aarch64 armv7h)
url="https://github.com/raspberrypi/rpicam-apps"
license=(BSD-2-Clause)
makedepends=(
  boost
  git
  libcamera-rpi
  libdrm
  libexif
  libjpeg-turbo
  libpng
  libtiff
  meson
)
# https://github.com/raspberrypi/rpicam-apps/blob/main/apps/meson.build
provides=(
  rpicam-still
  rpicam-vid
  rpicam-hello
  rpicam-raw
  rpicam-jpeg
  libcamera-still
  libcamera-vid
  libcamera-hello
  libcamera-raw
  libcamera-jpeg
)

source=(
  "https://github.com/raspberrypi/rpicam-apps/archive/refs/tags/v${pkgver}.tar.gz"
  "rpicam-apps.rules" # http://git.intranifty.net/pkg/rpicam-apps/tree/rpicam-apps.rules
)
# makepkg -g
sha256sums=(
  "7af81158e04f16af379d0804b939c817fee01e1510c1e11249f998a4f4600c8d"
  "4045e41e24d8c3e247b667a66bc9eee13918f599f464003dde25ca55ed7bb995"
)

prepare() {
  mv $pkgname-$pkgver $pkgname
  cd $pkgname

  # add version, so that utils/gen-version.sh may rely on it
  printf "%s\n" "$pkgver" > .tarball-version
}

build() {
  local meson_options=(
    --buildtype=release
    -D enable_libav=disabled
    -D enable_drm=enabled
    -D enable_egl=disabled
    -D enable_qt=disabled
    -D enable_opencv=disabled
    -D enable_tflite=disabled
    -D enable_hailo=disabled
    -D enable_imx500=false
  )

  arch-meson $pkgname $pkgname/build "${meson_options[@]}"
  meson compile -C $pkgname/build
}

package() {
  depends=(
    libcamera-rpi
    boost
  )

  install --mode=644 -D rpicam-apps.rules "${pkgdir}/etc/udev/rules.d/rpicam-apps.rules"

  meson install -C $pkgname/build --destdir "$pkgdir"
  install -vDm 644 $pkgname/license.txt -t "$pkgdir/usr/share/licenses/$pkgname/"
}
