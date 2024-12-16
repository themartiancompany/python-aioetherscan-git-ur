# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Pellegrino Prevete (tallero) <pellegrinoprevete@gmail.com>

_poetry="true"
_setuptools="false"
_build="false"
_os="$( \
  uname \
    -o)"
_git="false"
_offline="false"
_proj="hip"
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
_pkg=aioetherscan
_pkgname="${_py}-${_pkg}"
pkgname="${_pkgname}-git"
pkgver="0.9.6.1".r3.g"c3041033e3dea1ba4ce7748fa1c7093e5d35f94a"
_commit="hijess"
pkgrel=1
_pkgdesc=(
  'Etherscan API async Python wrapper.'
)
pkgdesc="${_pkgdesc[*]}"
arch=(
  any
)
_gl="gitlab.com"
_gh="github.com"
_http="https://${_gh}"
_ns="themartiancompany"
_local="file://${HOME}/${_pkg}"
url="${_http}/${_ns}/${_pkg}"
_gh_api="https://api.${_gh}/repos/${_ns}/${_pkg}"
license=(
  AGPL3
)
depends=(
  "${_py}>=${_pymajver}"
  "${_py}<${_pynextver}"
  "${_py}>=3.9"
  "${_py}-aiohttp>=3.4"
  "${_py}-asyncio-throttle>=1.0.1"
  "${_py}-aiohttp-retry>=2.8.3"
)
makedepends=(
  "cython"
  "${_py}-wheel"
  "${_py}-setuptools"
)
if [[ "${_build}" == "true" ]] || \
   [[ "${_poetry}" == "true" ]]; then
  makedepends+=(
    "${_py}-build"
    "${_py}-poetry-core"
    "${_py}-installer"
  )
fi
if [[ "${_poetry}" == "true" ]]; then
  makedepends+=(
    "${_py}-poetry"
  )
fi
checkdepends=(
  "${_py}-pytest>=8.2.2"
  "${_py}-pytest-asyncio>=0.23.7"
)
optdepends=(
)
[[ "${_os}" == 'Android' ]] && \
  optdepends+=(
  )
provides=(
  "${_pkgname}=${pkgver}"
  "${_pkg}=${pkgver}"
)
conflicts=(
  "${_pkgname}"
  "${_pkg}"
)
groups=(
 "${_proj}"
 "${_proj}-git"
)
_url="${url}"
if [[ "${_offline}" == "true" ]]; then
  _url="${_local}"
fi
_tag="master"
_tag_name="branch"
_tarname="${_pkg}-${_tag}"
if [[ "${_git}" == true ]]; then
  echo "hi"
  makedepends+=(
    "git"
  )
  _src="${_tarname}::git+${_url}#${_tag_name}=${_tag}?signed"
  _sum="SKIP"
elif [[ "${_git}" == false ]]; then
  makedepends+=(
    curl
    jq
  )
  if [[ "${_tag_name}" == 'branch' ]]; then
    _src="${_tarname}.tar.gz::${_url}/archive/refs/heads/${_tag}.tar.gz"
    _sum="SKIP"
  elif [[ "${_tag_name}" == 'pkgver' ]]; then
    _src="${_tarname}.tar.gz::${_url}/archive/refs/tags/${_tag}.tar.gz"
    _sum="d4f4179c6e4ce1702c5fe6af132669e8ec4d0378428f69518f2926b969663a91"
  elif [[ "${_tag_name}" == "commit" ]]; then
    _src="${_tarname}.zip::${_url}/archive/${_commit}.zip"
    _sum='31ac0b8014076f8c7cafa1c92c311359dc7f6f931e17c0a718c406cae77272f7'
  fi
fi
source=(
  "${_src}"
)
sha256sums=(
  "${_sum}"
)
validpgpkeys=(
  # Truocolo <truocolo@aol.com>
  '97E989E6CF1D2C7F7A41FF9F95684DBE23D6A3E9'
  # Pellegrino Prevete <pellegrinoprevete@gmail.com>
  'E30813C918EDD358EFCDCA34D3104DE92EB34492'
)

_nth() {
  local \
    _str="${1}" \
    _n="${2}"
  echo \
    "${_str}" | \
    awk \
      -F '+' \
      '{print $'"${_n}"'}'
}

_jq_pkgver() {
  local \
    _version \
    _rev \
    _commit
  _version="$( \
    curl \
      --silent \
      "${_gh_api}/tags" | \
      jq \
        '.[0].name')"
  _version_commit="$( \
    curl \
      --silent \
      "${_gh_api}/tags" | \
      jq \
        '.[0].commit.sha')"
  _rev="$( \
    curl \
      --silent \
      "${_gh_api}/commits" | \
      jq \
        'map(.sha == '${_version_commit}' ) | index(true)')"
  _commit="$( \
    curl \
      --silent \
      "${_gh_api}/commits" | \
      jq \
        '.[0].sha')"
  printf \
    "%s.r%s.g%s" \
    "${_version}" \
    "${_rev}" \
    "${_commit}"
}

_parse_ver() {
  local \
    _pkgver="${1}" \
    _out="" \
    _ver \
    _rev \
    _commit
  _ver="$( \
    _nth \
      "${_pkgver}" \
      "1")"
  _rev="$( \
    _nth \
      "${_pkgver}" \
      "2")"
  _commit="$( \
    _nth \
      "${_pkgver}" \
      "3")"
  _out=${_ver}
  [[ "${_rev}" != "" ]] && \
    _out+=".r${_rev}"
  [[ "${_commit}" != "" ]] && \
    _out+=".${_commit}"
  echo \
    "${_out}"
}

_git_pkgver() {
  local \
    _pkgver
  _pkgver="$( \
    git \
      describe \
      --tags \
      --long | \
      sed \
        's/-/+/g')"
  _parse_ver \
    "${_pkgver}"
}

pkgver() {
  cd \
    "${_tarname}"
  if [[ "${_git}" == true ]]; then
    _git_pkgver
  elif [[ "${_git}" == false ]]; then
    _jq_pkgver
  fi
}

_poetry_build() {
  export \
    POETRY_VIRTUALENVS_OPTIONS_NO_PIP=true \
    POETRY_VIRTUALENVS_OPTIONS_SYSTEM_SITE_PACKAGES=true
    # POETRY_VIRTUALENVS_CREATE=false
  # POETRY_VIRTUALENVS_CREATE=false \
  POETRY_VIRTUALENVS_OPTIONS_NO_PIP=true \
  POETRY_VIRTUALENVS_OPTIONS_SYSTEM_SITE_PACKAGES=true \
  poetry \
    -vvv \
    build
}

_build_build() {
  # this currently gives no output at all
  "${_py}" \
    -m \
      build \
    --verbose \
    --wheel \
    --no-isolation
}

build() {
  cd \
    "${_tarname}"
  if [[ "${_poetry}" == "true" ]]; then
    _poetry_build
  fi
  if [[ "${_build}" == "true" ]]; then
    _build_build
  fi
}

_setuptools_package() {
  "${_py}" \
    setup.py \
      install \
        --root="${pkgdir}" \
        --optimize=1
}

_installer_package() {
  "${_py}" \
    -m \
      installer \
      --destdir="${pkgdir}" \
      dist/*.whl
}

# shellcheck disable=SC2154
package() {
  cd \
    "${_tarname}"
  if [[ "${_setuptools}" == "true" ]]; then
    _setuptools_package
  elif [[ "${_poetry}" == "true" ]] || \
       [[ "${_build}" == "true" ]]; then
    _installer_package
  fi
  install \
    -Dm644 \
    LICENSE \
    "${pkgdir}"/usr/share/licenses/${pkgname}/LICENSE
}

# vim: ft=sh syn=sh et
