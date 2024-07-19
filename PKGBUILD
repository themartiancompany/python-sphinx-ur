# Maintainer: Johannes Löthberg <johannes@kyriasis.com>
# Maintainer: Daniel M. Capella <polyzen@archlinux.org>
# Contributor: Sébastien Luttringer
# Contributor: Angel Velasquez <angvp@archlinux.org>
# Contributor: Fabio Volpe <volpefabio@gmail.com>

pkgname=python-sphinx
_name=${pkgname#python-}
pkgver=7.4.6
pkgrel=1
pkgdesc='Python documentation generator'
arch=(any)
url=http://www.sphinx-doc.org/
license=(BSD-2-Clause)
depends=(
  python-babel
  python-docutils
  python-imagesize
  python-jinja
  python-packaging
  python-pygments
  python-requests
  python-snowballstemmer
  python-sphinx-alabaster-theme
  python-sphinxcontrib-{{apple,dev,html}help,jsmath,qthelp,serializinghtml}
)
makedepends=(
  git
  python-build
  python-flit-core
  python-installer
)
checkdepends=(
  cython
  imagemagick
  librsvg
  python-defusedxml
  python-pytest
  python-setuptools
  texlive-{fontsextra,fontsrecommended,latexextra,luatex,xetex}
)
optdepends=(
  'imagemagick: for ext.imgconverter'
  'texlive-fontsextra: for the default admonition title icons in PDF output'
  'texlive-latexextra: for generation of PDF documentation'
)
source=("git+https://github.com/$_name-doc/$_name.git#tag=v$pkgver")
b2sums=('3b723f17b490b4245a5be3af0fa9eb85bdb3d25745d5df89e7d5e6430194b03bd20c1a99162f25d55d80030722f3b7b990b4b0600e98a908bf7969670b9a44fa')

build() {
  cd "$_name"
  python -m build --wheel --skip-dependency-check --no-isolation

  mkdir -p tempinstall
  bsdtar -xf dist/*.whl -C tempinstall
  PYTHONPATH="$PWD/tempinstall" make -C doc man
}

check() {
  cd "$_name"
  LC_ALL="en_US.UTF-8" python -X dev -X warn_default_encoding -m pytest -vx
}

package() {
  cd "$_name"
  python -m installer --destdir="$pkgdir" dist/*.whl
  install -Dt "$pkgdir"/usr/share/man/man1 doc/_build/man/"$_name"-*.1

  # Symlink license file
  local site_packages=$(python -c "import site; print(site.getsitepackages()[0])")
  install -d "$pkgdir"/usr/share/licenses/$pkgname
  ln -s "$site_packages"/"$_name"-$pkgver.dist-info/LICENSE.rst \
    "$pkgdir"/usr/share/licenses/$pkgname/LICENSE.rst
}
