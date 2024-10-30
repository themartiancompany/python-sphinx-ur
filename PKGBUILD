# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer: Pellegrino Prevete <pellegrinoprevete@gmail.com>
# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Johannes Löthberg <johannes@kyriasis.com>
# Maintainer: Daniel M. Capella <polyzen@archlinux.org>
# Contributor: Sébastien Luttringer
# Contributor: Angel Velasquez <angvp@archlinux.org>
# Contributor: Fabio Volpe <volpefabio@gmail.com>

_git="true"
_os="$( \
  uname \
    -o)"
if [[ "${_os}" == "Android" ]]; then
  _git="false"
fi
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
pkgver=8.0.2
pkgrel=1
pkgdesc='Python documentation generator'
arch=(
  'any'
)
url="http://www.${_pkg}-doc.org"
license=(
  "BSD-2-Clause"
)
depends=(
  "${_py}>=${_pymajver}"
  "${_py}<${_pynextver}"
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
if [[ "${_git}" == "true" ]]; then
  _src="${_pkg}-${pkgver}::$git+${_url}.git#tag=v${pkgver}"
  _sum="a6ac5e7ec9fc892c60f22cc023a9d713e1f73668c582ac483d8abb2008fc752c133eba27fc631b20466eddffe41be11b5a826511e83eb79bcc621dc95d61af04"
elif [[ "${_git}" == "false" ]]; then
  _src="${_pkg}-${pkgver}.tar.gz::${_url}/archive/refs/tags/v${pkgver}.tar.gz"
  _sum="ed6e321a1e58341609d88993c418ec1a0a580683ed28895077322fdba839d5c158007d65d5349d4d53c5e3b49ae823142cc6eb0203812580ebbb5b95247bf157"
fi
source=(
  "${_src}"
)
b2sums=(
  "${_sum}"
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
