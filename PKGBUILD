# Maintainer: Johannes Löthberg <johannes@kyriasis.com>
# Maintainer: Daniel M. Capella <polyzen@archlinux.org>
# Contributor: Sébastien Luttringer
# Contributor: Angel Velasquez <angvp@archlinux.org>
# Contributor: Fabio Volpe <volpefabio@gmail.com>

pkgname=python-sphinx
_name=${pkgname#python-}
pkgver=7.2.6
pkgrel=5
pkgdesc='Python documentation generator'
arch=('any')
url=http://www.sphinx-doc.org/
license=('BSD-2-Clause')
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
makedepends=('git' 'python-build' 'python-flit-core' 'python-installer')
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
source=("git+https://github.com/$_name-doc/$_name.git#tag=v$pkgver"
         python-sphinx-7.2.6-SOURCE_DATE_EPOCH-fix.patch)
b2sums=('ff04f896e1707375a462e863f4abba9f1bfa2802d0bdfd30c5b602da44357a53f369489ace8e4cb417dc8a95a5f894e030ad47ff8c56d73d02578f0d17a2b063'
        '73afe7aad40e1ec581ba7a2b8a6c8abb28cb8a14950d158843841fb2faeffee160ded10796617bae69a62009ee36c33cac284737c76415059b9959d42ac5a055')

prepare() {
  cd "$_name"
  # Fix autodoc tests for Python 3.11.7 and later
  git cherry-pick -n 7d4ca9cb3eb415084cb288ff0d8d7565932be5be
  # Fix processing copyright dates when SOURCE_DATE_EPOCH is set
  patch -Np1 -i ../python-sphinx-7.2.6-SOURCE_DATE_EPOCH-fix.patch
  # Support docutils 0.21
  sed -e 's|<0.21|<0.22|' -i pyproject.toml
}

build() {
  cd "$_name"
  python -m build --wheel --skip-dependency-check --no-isolation

  mkdir -p tempinstall
  bsdtar -xf dist/*.whl -C tempinstall
  PYTHONPATH="$PWD/tempinstall" make -C doc man
}

check() {
  cd "$_name"
  LC_ALL="en_US.UTF-8" python -X dev -X warn_default_encoding -m pytest -v \
    --deselect tests/test_build_linkcheck.py::test_defaults \
    --deselect tests/test_build_html.py::test_html_scaled_image_link # fail in chroot
}

package() {
  cd "$_name"
  python -m installer --destdir="$pkgdir" dist/*.whl
  install -Dt "$pkgdir"/usr/share/man/man1 doc/_build/man/"$_name"-*.1

  # Symlink license file
  local site_packages=$(python -c "import site; print(site.getsitepackages()[0])")
  install -d "$pkgdir"/usr/share/licenses/$pkgname
  ln -s "$site_packages"/"$_name"-$pkgver.dist-info/LICENSE \
    "$pkgdir"/usr/share/licenses/$pkgname/LICENSE
}

# vim:set ts=2 sw=2 et:
