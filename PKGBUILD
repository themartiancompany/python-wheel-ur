# Maintainer: Felix Yan <felixonmars@archlinux.org>
# Contributor: Lance Chen <cyen0312@gmail.com>

_pypiname=wheel
pkgbase=python-wheel
pkgname=('python-wheel' 'python2-wheel')
pkgver=0.28.0
pkgrel=1
pkgdesc="A built-package format for Python"
arch=(any)
url="https://pypi.python.org/pypi/wheel"
license=('MIT')
makedepends=('python-setuptools' 'python2-setuptools')
checkdepends=('python-jsonschema' 'python2-jsonschema' 'python-pytest-cov' 'python2-pytest-cov'
              'python-keyring' 'python2-keyring' 'python-keyrings-alt' 'python2-keyrings-alt')
source=("https://pypi.python.org/packages/source/w/wheel/$_pypiname-$pkgver.tar.gz")
md5sums=('6d6b7e87dd744c6fac77ca9b43479207')

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
  PYTHONPATH="$PWD/tmp_install/usr/lib/python3.5/site-packages:$PYTHONPATH" py.test wheel

  cd "$srcdir/wheel-$pkgver-py2"
  python2 setup.py install --root="$PWD/tmp_install" --optimize=1
  PYTHONPATH="$PWD/tmp_install/usr/lib/python2.7/site-packages:$PYTHONPATH" py.test2 wheel
}

package_python-wheel() {
  depends=('python')
  optdepends=('python-keyring: for wheel.signatures')

  cd "$srcdir/$_pypiname-$pkgver"
  python setup.py install --root="$pkgdir/" --optimize=1
  install -D -m644 LICENSE.txt "$pkgdir/usr/share/licenses/$pkgname/LICENSE.txt"
}

package_python2-wheel() {
  depends=('python2')
  optdepends=('python2-keyring: for wheel.signatures')

  cd "$srcdir/$_pypiname-$pkgver"
  python2 setup.py install --root="$pkgdir/" --optimize=1
  install -D -m644 LICENSE.txt "$pkgdir/usr/share/licenses/$pkgname/LICENSE.txt"
  mv "$pkgdir/usr/bin/wheel" "$pkgdir/usr/bin/wheel2"
}

# vim:set ts=2 sw=2 et:
