# Maintainer: Simon-Friedrich Boettger <email at simonboettger dot de>
pkgname=garminfit-git
_project=garminfit
pkgver=21.195.0.r0.g8ad0406
pkgrel=1
pkgdesc="GarminFIT C++ API for handling Garmin FIT files."
arch=('x86_64')
url="https://github.com/garmin/fit-cpp-sdk"
license=('FIT Protocol License')
makedepends=('git' 'cmake' 'ninja' 'llvm')
source=("git+${url}.git#branch=main"
"https://github.com/garmin/fit-sdk-tools/raw/refs/heads/main/FitCSVTool/FitCSVTool.jar"
"https://raw.githubusercontent.com/petrus82/GarminFit/refs/heads/master/CMakeLists.txt"
)
sha256sums=('SKIP'
    '2aa5c2871dfd6517f231b6d43f69871793ecd32c0a4e0f46d63dff249da35eb7'
    'SKIP'
)

pkgver() {
  cd "${srcdir}/fit-cpp-sdk"
  git describe --tags --long --abbrev=7 | sed 's/\([^-]*-g\)/r\1/;s/-/./g'
}

build() {
  cd "${srcdir}/fit-cpp-sdk"
  cmake -S . -B build \
    -G Ninja \
    -DCMAKE_CXX_COMPILER=/usr/bin/clang++ \
    -DCMAKE_AR=/usr/bin/llvm-ar \
    -DCMAKE_RANLIB=/usr/bin/llvm-ranlib \
    -DCMAKE_STRIP=/usr/bin/llvm-strip \
    -DCMAKE_LINKER=/usr/bin/ld.lld \
    -DCMAKE_ADDR2LINE=/usr/bin/llvm-addr2line \
    -DCMAKE_BUILD_TYPE=Release \
    -DCMAKE_INSTALL_PREFIX="/usr/" \
    -Wno-dev
  cmake --build build -j $(nproc)
}

package() {
  cd "${srcdir}/fit-cpp-sdk"
  DESTDIR="${pkgdir}" cmake --install build
  install -Dm644 "${srcdir}/${_project}/FitCSVTool.jar" "${pkgdir}/usr/lib/${_project}/FitCSVTool.jar"
}
