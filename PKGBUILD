# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer: Pellegrino Prevete <pellegrinoprevete@gmail.com>
# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Felix Yan <felixonmars@archlinux.org>
# Maintainer: Morten Linderud <foxboron@archlinux.org>
# Contributor: Lance Chen <cyen0312@gmail.com>

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
_pkg=wheel
pkgname="${_py}-${_pkg}"
pkgver=0.42.0
pkgrel=1
pkgdesc="A built-package format for Python"
arch=(
  any
)
url="https://pypi.python.org/pypi/${_pkg}"
license=('MIT')
depends=(
  "${_py}<${_pynextver}"
  "${_py}-packaging"
)
optdepends=(
  "${_py}-keyring: for wheel.signatures"
  "${_py}-xdg: for wheel.signatures"
)
makedepends=(
  "${_py}-setuptools"
  "${_py}-build"
  "${_py}-flit-core"
  "${_py}-installer"
)
checkdepends=(
  "${_py}-jsonschema"
  "${_py}-pytest"
  "${_py}-keyring"
  "${_py}-keyrings-alt"
  "${_py}-xdg"
  "${_py}-pytest-cov"
)
_http="https://github.com"
_ns="pypa"
_url="${_http}/${_ns}/${_pkg}"
_tarname="${_pkg}-${pkgver}"
source=(
  "${_tarname}.tar.gz::${_url}/archive/${pkgver}.tar.gz"
)
sha512sums=(
  '022aebdf077d32673cbbd9a41b96001324c026e2ccfc4eb42648a2a29c25da031f8b14c38ae6143b6d3720d57f269df6087c2025009f6d1296579513de214bd1'
)

prepare() {
  local \
    _metadata_cmd \ 
    _metadata_pattern \
    _metadata_repl \
    _tags_cmd \ 
    _tags_pattern \
    _tags_repl \
    _version_cmd \ 
    _version_pattern \
    _version_repl \
    _other_tags_cmd \ 
    _other_tags_pattern \
    _other_tags_repl
  _metadata_pattern="from .vendored.packaging.requirements import Requirement"
  _metadata_repl="from packaging.requirements import Requirement"
  _metadata_cmd= "s/${_metadata_pattern}/${_metadata_repl}/"
  _tags_pattern="from .vendored.packaging import tags"
  _tags_repl="from packaging import tags"
  _tags_cmd="s/${_tags_pattern}/${_tags_repl}/"
  _version_pattern="from .vendored.packaging import version as _packaging_version"
  _version_repl="from packaging import version as _packaging_version"
  _version_cmd="s/${_version_pattern}/${_version_repl}/"
  _other_tags_pattern="from wheel.vendored.packaging import tags"
  _other_tags_repl="from packaging import tags"
  _other_tags_cmd="s/${_other_tags_pattern}/${_other_tags_repl}/"
  cd \
    "${_tarname}"
  # https://github.com/pypa/wheel/pull/365 but why?
  rm \
    -r \
    src/wheel/vendored
  sed \
    -i \
    "${_metadata_cmd}" \
    src/wheel/metadata.py
  sed \
    -i \
    "${_tags_cmd}" \
    src/wheel/bdist_wheel.py
  sed \
    -i \
    "${_version_cmd}" \
    src/wheel/bdist_wheel.py
  sed \
    -i \
    "${_other_tags_cmd}" \
    tests/test_bdist_wheel.py
}

build() {
  cd \
    "${_tarname}"
  "${_py}" \
    -m \
      build \
    --wheel \
    --no-isolation
}

check() {
  local \
    site_packages
  site_packages="$( \
    "${_py}" \
      -c \
      "import site; print(site.getsitepackages()[0])")"
  # Hack entry points by installing it
  cd \
    "${_tarname}"
  "${_py}" \
    -m \
      installer \
      --destdir="$PWD/tmp_install" \
      dist/*.whl
  PYTHONPATH="$PWD/tmp_install/${site_packages}" \
  pytest
}

package() {
  cd \
    "${_tarname}"
  "${_py}" \
    -m \
      installer \
    --destdir="${pkgdir}" \
    dist/*.whl
  install \
    -Dm644 \
    LICENSE.txt \
    -t \
    "${pkgdir}/usr/share/licenses/${pkgname}/"
}
