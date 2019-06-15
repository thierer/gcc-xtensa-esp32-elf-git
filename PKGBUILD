# Maintainer: Alexei Karpenko <alexei@karpenko.ca>
pkgname=gcc-xtensa-esp32-elf-git
_pkgname=crosstool-NG
pkgver=1.22.0.r81.g2411e6ac
pkgrel=1
pkgdesc='ESP32 GCC Cross-compiler Toolchain'
arch=(x86_64)
url='https://esp-idf.readthedocs.io/en/latest/get-started/linux-setup.html'
license=(Apache)
depends=(glibc python ncurses python2-pyserial)
makedepends=(gcc git make flex bison gperf)
provides=(gcc-xtensa-esp32-elf)
conflicts=(gcc-xtensa-esp32-elf-bin)
options=(!libtool !buildflags)
source=(
	git+https://github.com/thierer/crosstool-NG.git#branch=xtensa-1.22.x
	0015-fix-too-many-template-parameters.patch::https://github.com/staticfloat/gcc/commit/94801184df727b94bf7b8d64b1f98a22f51325d7.patch
	1000-gdb-python-3-7.patch::https://raw.githubusercontent.com/thierer/esp-open-sdk/9755038a67305047660dbf844f1d1f19bdd2f1e9/1000-gdb-python-3-7.patch)
md5sums=('SKIP'
         '8493778dc92ea5231c94ade075f524ac'
         '133effcb81ac2ac8190388af00fe631f')

pkgver() {
  cd "$srcdir/${_pkgname}"
  # cutting off 'crosstool.ng.' prefix that present in the git tag
  git describe --long | sed 's/^crosstool.ng.//;s/\([^-]*-g\)/r\1/;s/-/./g'
}

prepare() {
  cp -f 0015-fix-too-many-template-parameters.patch $srcdir/${_pkgname}/local-patches/gcc/5.2.0/
  mkdir -p $srcdir/${_pkgname}/local-patches/gdb/7.10/
  cp -f 1000-gdb-python-3-7.patch $srcdir/${_pkgname}/local-patches/gdb/7.10/
}

build() {
  cd "$srcdir/${_pkgname}"
  
  echo Building crosstool-ng...
  ./bootstrap
  ./configure --enable-local
  make

  echo Building xtensa-esp32-elf...
  ./ct-ng xtensa-esp32-elf 
  sed -i 's/^CT_GDB_CROSS_EXTRA_CONFIG_ARRAY.*/CT_GDB_CROSS_EXTRA_CONFIG_ARRAY="--with-guile=guile-2.0"/g' .config
  ./ct-ng build
}

package() {
  cd "$srcdir/${_pkgname}"
  for file in builds/xtensa-esp32-elf/bin/xtensa-esp32-elf*; do
    install -m 755 -D -t "$pkgdir/usr/bin" "${file}"
  done
}
