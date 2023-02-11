# Maintainer: Pellegrino Prevete <pellegrinoprevete@gmail.com>
# Contributor: Felix Golatofski <contact@xdfr.de>
# Contributor: Olivier Mehani <olivier.mehani@inria.fr>

_pkgname="binutils"
target="mipsel-linux"
pkgname="${target}-${_pkgname}"
pkgver=2.39
pkgrel=1
pkgdesc="mipsel-linux binary manipulation programs"
url="https://www.gnu.org/software/${_pkgname}"
depends=(glibc)
makedepends=("libgmp-static"
             "gmp4-static"
             "mpfr-static"
             "libmpc-static")
arch=('x86_64' 'i686')
license=(GPL)
source=("ftp://ftp.gnu.org/gnu/${_pkgname}/${_pkgname}-${pkgver}.tar.bz2")
sha256sums=('da24a84fef220102dd24042df06fdea851c2614a5377f86effa28f33b7b16148')

build() {
  cd "${srcdir}/${_pkgname}-${pkgver}"

  local _cflags=( # -D_FORTIFY_SOURCE=0
                 # -O2
                 -I/usr/include
                 -L/usr/lib
                 -Wno-implicit-function-declaration)

  local _ldflags=( # ${LDFLAGS}
                  -I/usr/include
                  -L/usr/lib
                  -lgmp
                  -lmpfr
                  -lmpc
                  -static
                  /usr/lib/libgmp4.a
                  /usr/lib/libgmpxx4.a
                  /usr/lib/libgmp.a
                  /usr/lib/libmpfr.a
                  /usr/lib/libmpc.a
                  -s)

  local _build_opts=(${_make_opts[@]}
                     CFLAGS="${_cflags[*]}"
                     CPPFLAGS="${_cflags[*]}"
                     CXXFLAGS="${_cflags[*]}"
                     LDFLAGS="${_ldflags[*]}")

  export CFLAGS="${_cflags[*]}"
  export CPPFLAGS="${_cflags[*]}"
  export CXXFLAGS="${_cflags[*]}"
  export LDFLAGS="${_ldflags[*]}"


  for _target in "${target}"; do
    rm -rf "build-${_target}"
    mkdir -p "build-${_target}"
    cd "build-${_target}"
    local _configure_opts=(--prefix="/usr"
                           --target="${_target}"
                           --infodir="/usr/${_target}/share/info"
                           --disable-separate-code
                           --disable-sim
                           --with-gmp
                           --with-mpfr
                           --with-mpc
                           --disable-nls)

    LD_LIBRARY_PATH=/usr/lib \
    CC="/usr/bin/gcc" \
    CXX="/usr/bin/g++" \
    CFLAGS="${_cflags[*]}" \
    CXXFLAGS="${_cxxflags[*]}" \
    LDFLAGS="${_ldflags[*]}" \
    LIBS="${_libs[*]}" \
    "../configure" ${_configure_opts[@]}

    make "${_build_opts[@]}"
    cd ..
  done
}

package() {
  local _target
  cd "${srcdir}/${_pkgname}-${pkgver}"
  for _target in "${target}"; do
    cd "build-${_target}"
    make DESTDIR="${pkgdir}" "${_make_opts[@]}" install-strip
    cd ..
  done
}
