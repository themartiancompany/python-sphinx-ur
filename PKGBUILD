# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer: Pellegrino Prevete <pellegrinoprevete@gmail.com>
# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Johannes Löthberg <johannes@kyriasis.com>
# Maintainer: Daniel M. Capella <polyzen@archlinux.org>
# Contributor: Sébastien Luttringer
# Contributor: Angel Velasquez <angvp@archlinux.org>
# Contributor: Fabio Volpe <volpefabio@gmail.com>

_py="python"
_pyver="$( \
  "${_py}" \
    -V | \
    awk \
      '{print $2}')"
_pymajver="${_pyver%.*}"
_pyminver="${_pymajver#*.}"
_pynextver="${_pymajver%.*}.$(( \
  ${_pyminver} + 1))"
_pkg=sphinx
pkgname="${_py}-${_pkg}"
_name=${pkgname#python-}
pkgver=8.0.2
pkgrel=1
pkgdesc='Python documentation generator'
arch=(
  'any'
)
url="http://www.${_pkg}-doc.org"
license=(
  BSD-2-Clause
)
depends=(
  "${_py}-babel"
  "${_py}-docutils"
  "${_py}-imagesize"
  "${_py}-jinja"
  "${_py}-packaging"
  "${_py}-pygments"
  "${_py}-requests"
  "${_py}-snowballstemmer"
  "${_py}-sphinx-alabaster-theme"
  "${_py}-sphinxcontrib-"{{"apple","dev","html"}"help","jsmath","qthelp","serializinghtml"}
)
makedepends=(
  git
  "${_py}-build"
  "${_py}-flit-core"
  "${_py}-installer"
)
checkdepends=(
  cython
  imagemagick
  librsvg
  "${_py}-defusedxml"
  "${_py}-pytest"
  "${_py}-setuptools"
  "texlive-"{"fontsextra","fontsrecommended","latexextra","luatex","xetex"}
)
optdepends=(
  'imagemagick: for ext.imgconverter'
  'texlive-fontsextra: for the default admonition title icons in PDF output'
  'texlive-latexextra: for generation of PDF documentation'
)
_http="https://github.com"
_ns="${_pkg}-doc"
_url="${_http}/${_ns}/${_pkg}"
source=(
  "${_pkg}-${pkgver}::git+${_url}.git#tag=v${pkgver}"
)
b2sums=(
  'a6ac5e7ec9fc892c60f22cc023a9d713e1f73668c582ac483d8abb2008fc752c133eba27fc631b20466eddffe41be11b5a826511e83eb79bcc621dc95d61af04'
)

build() {
  cd \
    "${_pkg}-${pkgver}"
  "${_py}" \
    -m \
      build \
    --wheel \
    --skip-dependency-check \
    --no-isolation
  mkdir \
    -p \
    tempinstall
  bsdtar \
    -xf  \
    dist/*.whl \
    -C \
      tempinstall
  PYTHONPATH="$PWD/tempinstall" \
  make \
    -C \
    doc \
    man
}

check() {
  cd \
    "${_pkg}-${pkgver}"
  LC_ALL="en_US.UTF-8" \
  "${_py}" \
    -X \
      dev \
    -X \
      warn_default_encoding \
    -m \
      pytest \
        -vx
}

package() {
  local \
    _site_packages
  _site_packages=$( \
   "${_py}" \
     -c \
     "import site; print(site.getsitepackages()[0])")
  cd \
    "${_pkg}-${pkgver}"
  "${_py}" \
    -m \
      installer \
    --destdir="${pkgdir}" \
    dist/*.whl
  install \
    -Dt \
    "${pkgdir}/usr/share/man/man1" \
    "doc/_build/man/${_pkg}"-*.1
  # Symlink license file
  install \
    -d \
      "${pkgdir}/usr/share/licenses/${pkgname}"
  ln \
    -s \
      "${_site_packages}/${_pkg}-${pkgver}.dist-info/LICENSE.rst" \
    "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE.rst"
}
