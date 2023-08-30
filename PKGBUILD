# Maintainer: Johannes Löthberg <johannes@kyriasis.com>
# Maintainer: Daniel M. Capella <polyzen@archlinux.org>
# Contributor: Sébastien Luttringer
# Contributor: Angel Velasquez <angvp@archlinux.org>
# Contributor: Fabio Volpe <volpefabio@gmail.com>

pkgname=python-sphinx
_name=${pkgname#python-}
pkgver=7.2.5
pkgrel=1
pkgdesc='Python documentation generator'
arch=('any')
url=http://www.sphinx-doc.org/
license=('BSD')
depends=(
  'python-babel'
  'python-docutils'
  'python-imagesize'
  'python-jinja'
  'python-packaging'
  'python-pygments'
  'python-requests'
  'python-snowballstemmer'
  'python-sphinx-alabaster-theme'
  'python-sphinxcontrib-'{{apple,dev,html}help,jsmath,qthelp,serializinghtml}
)
makedepends=('python-build' 'python-flit-core' 'python-installer')
checkdepends=(
  'cython'
  'imagemagick' 'librsvg'
  'python-filelock'
  'python-html5lib'
  'python-pytest'
  'python-setuptools'
  'texlive-'{fontsextra,fontsrecommended,latexextra,luatex,xetex}
)
optdepends=(
  'imagemagick: for ext.imgconverter'
  'texlive-latexextra: for generation of PDF documentation'
)
source=("https://github.com/$_name-doc/$_name/archive/v$pkgver/$_name-$pkgver.tar.gz")
b2sums=('27e0c7f8916e36990e214f4c5013434ff0e1e0045513efdf4b16eb56f0865d4cb52db802a41e7f62cc18aae58457d03be6720c129854cc63f35a7537b361ba2d')

build() {
  cd "$_name"-$pkgver
  python -m build --wheel --skip-dependency-check --no-isolation

  mkdir -p tempinstall
  bsdtar -xf dist/*.whl -C tempinstall
  PYTHONPATH="$PWD/tempinstall" make -C doc man
}

check() {
  cd "$_name"-$pkgver
  LC_ALL="en_US.UTF-8" python -X dev -X warn_default_encoding -m pytest -v
}

package() {
  cd "$_name"-$pkgver
  python -m installer --destdir="$pkgdir" dist/*.whl
  install -Dt "$pkgdir"/usr/share/man/man1 doc/_build/man/"$_name"-*.1

  # Symlink license file
  local site_packages=$(python -c "import site; print(site.getsitepackages()[0])")
  install -d "$pkgdir"/usr/share/licenses/$pkgname
  ln -s "$site_packages"/"$_name"-$pkgver.dist-info/LICENSE \
    "$pkgdir"/usr/share/licenses/$pkgname/LICENSE
}

# vim:set ts=2 sw=2 et:
