# Maintainer: Johannes Löthberg <johannes@kyriasis.com>
# Maintainer: Daniel M. Capella <polyzen@archlinux.org>
# Contributor: Sébastien Luttringer
# Contributor: Angel Velasquez <angvp@archlinux.org>
# Contributor: Fabio Volpe <volpefabio@gmail.com>

pkgname=python-sphinx
_name=${pkgname#python-}
pkgver=8.0.0
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
b2sums=('f16933e125b3bda6a727ad958e9a7641a1fcf1df8d0c7dbe53496641aab3dbb6a1386667af12d2b201d2945c14c0d4b94821ea06fe0434bff24975a2ee58f2b2')

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
