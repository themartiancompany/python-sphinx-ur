# Maintainer: Angel 'angvp' Velasquez <angvp[at]archlinux.com.ve>
# Contributor: Fabio Volpe <volpefabio@gmail.com>

pkgname=python-sphinx
pkgver=0.6.4
pkgrel=2
pkgdesc="Python documentation generator"
arch=('any')
url="http://sphinx.pocoo.org/"
license=('GPL')
depends=('setuptools' 'pygments' 'docutils' 'python-jinja')
source=(http://pypi.python.org/packages/source/S/Sphinx/Sphinx-$pkgver.tar.gz)
md5sums=('a65e0bcff6f79a7c013220d00ea137ad')

build() {
    cd "$srcdir/Sphinx-$pkgver"
    python setup.py install --root="$pkgdir" -O1 || return 1
}
