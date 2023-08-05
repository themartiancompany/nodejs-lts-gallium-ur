# Maintainer: Felix Yan <felixonmars@archlinux.org>
# Contributor  Bartłomiej Piotrowski <bpiotrowski@archlinux.org>
# Contributor: Axel Navarro < navarroaxel at gmail >
# Contributor: Thomas Dziedzic < gostrc at gmail >
# Contributor: James Campos <james.r.campos@gmail.com>
# Contributor: BlackEagle < ike DOT devolder AT gmail DOT com >
# Contributor: Dongsheng Cai <dongsheng at moodle dot com>
# Contributor: Masutu Subric <masutu.arch at googlemail dot com>
# Contributor: TIanyi Cui <tianyicui@gmail.com>
# Contributor: Pellegrino Prevete <pellegrinoprevete@gmail.com>

_pkg="nodejs"
_pkgname="${_pkg}js"
pkgname="${_pkgname}-lts-gallium"
pkgver=16.13.0
pkgrel=1
pkgdesc='Evented I/O for V8 javascript'
arch=('x86_64')
url="https://${_pkgname}.org"
license=('MIT')
depends=(
  'openssl'
  'zlib'
  'icu'
  'libuv'
  'libnghttp2'
  'c-ares'
  # 'http-parser'
  # 'v8'
)
makedepends=(
  "python<=3.10"
  'procps-ng'
)
_corepack=(
  "corepack: zero-runtime-dependency package acting"
  "as bridge between Node projects and their package managers.")
optdepends=(
  'npm: nodejs package manager'
  "${_corepack[*]}")
provides=("${_pkgname}=${pkgver}")
conflicts=(nodejs)
source=(
  "https://github.com/${_pkgname}/${_pkg}/archive/v${pkgver}/${_pkgname}-${pkgver}.tar.gz"
  system-c-ares.patch
)
sha512sums=(
  '0be8e2d427567910bfc986a6d98cff2351811ef9fc22f42a4625d58df958ed5b19d13fd09fbceb997d46582206e5f6ae4b79153870bb151c72062b9aaadb5a1c'
  'bca85b5a4622b38f20f3787bdb2784db76cd4b213fadf8a5813a375080e7481c4aff7556712d59df3e19849a7eb6b2f048a69b4e2162cb717ab1b3a791d2558e'
)

prepare() {
  patch -d "${_pkg}-${pkgver}" -Rp1 < \
    system-c-ares.patch
}

build() {
  local _configure_options=()
  _configure_options=(
    --prefix=/usr
    --with-intl=system-icu
    --without-npm
    --shared-openssl
    --shared-zlib
    --shared-libuv
    --experimental-http-parser
    --shared-nghttp2
    --shared-cares
    # --shared-v8
    # --shared-http-parser
  )
  cd "${_pkg}-${pkgver}"
  ./configure "${_configure_opts[@]}"
  make
}

check() {
  cd "${_pkg}-${pkgver}"
  make test || :
}

package() {
  cd "${_pkg}-${pkgver}"

  make DESTDIR="${pkgdir}" \
       install

  install -D \
          -m644 LICENSE \
          "${pkgdir}/usr/share/licenses/${_pkgname}/LICENSE"
}

# vim:set ts=2 sw=2 et:
