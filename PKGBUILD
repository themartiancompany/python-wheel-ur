# Maintainer: Felix Yan <felixonmars@archlinux.org>
# Maintainer: Morten Linderud <foxboron@archlinux.org>
# Contributor: Lance Chen <cyen0312@gmail.com>

_pypiname=wheel
pkgbase=python-wheel
pkgname=('python-wheel' 'python2-wheel')
pkgver=0.33.2
pkgrel=1
pkgdesc="A built-package format for Python"
arch=(any)
url="https://pypi.python.org/pypi/wheel"
license=('MIT')
makedepends=('python' 'python-setuptools'
             'python2' 'python2-setuptools')
checkdepends=('python-jsonschema' 'python2-jsonschema' 'python-pytest-cov' 'python2-pytest-cov'
              'python-keyring' 'python-keyrings-alt' 'python-xdg' 'python2-xdg')
source=("https://pypi.io/packages/source/w/wheel/$_pypiname-$pkgver.tar.gz")
source=("$pkgname-$pkgver.tar.gz::https://github.com/pypa/wheel/archive/$pkgver.tar.gz")
sha512sums=('6f0a294b10e75edb27aaf926877db870fe1d9faddbb7517e6ddb587fd1e388709cafdd8685d514a31ae46a19d91472d828024fe286c5733c6cc17aec10674b9e')

prepare() {
  cp -a wheel-$pkgver{,-py2}
}

build() {
  cd "$srcdir/wheel-$pkgver"
  python setup.py build

  cd "$srcdir/wheel-$pkgver-py2"
  python2 setup.py build
}

check() {
  # Hack entry points by installing it

  cd "$srcdir/wheel-$pkgver"
  python setup.py install --root="$PWD/tmp_install" --optimize=1
  PYTHONPATH="$PWD/tmp_install/usr/lib/python3.7/site-packages:$PYTHONPATH" py.test

  cd "$srcdir/wheel-$pkgver-py2"
  python2 setup.py install --root="$PWD/tmp_install" --optimize=1
  PYTHONPATH="$PWD/tmp_install/usr/lib/python2.7/site-packages:$PYTHONPATH" py.test2 -k 'not test_keygen'
}

package_python-wheel() {
  depends=('python')
  optdepends=('python-keyring: for wheel.signatures')
  optdepends=('python-xdg: for wheel.signatures')

  cd "$srcdir/$_pypiname-$pkgver"
  python setup.py install --root="$pkgdir/" --optimize=1 --skip-build
  install -D -m644 LICENSE.txt "$pkgdir/usr/share/licenses/$pkgname/LICENSE.txt"
}

package_python2-wheel() {
  depends=('python2')
  optdepends=('python2-keyring: for wheel.signatures')
  optdepends=('python2-xdg: for wheel.signatures')

  cd "$srcdir/$_pypiname-$pkgver"
  python2 setup.py install --root="$pkgdir/" --optimize=1 --skip-build
  install -D -m644 LICENSE.txt "$pkgdir/usr/share/licenses/$pkgname/LICENSE.txt"
  mv "$pkgdir/usr/bin/wheel" "$pkgdir/usr/bin/wheel2"
}

# vim:set ts=2 sw=2 et:
