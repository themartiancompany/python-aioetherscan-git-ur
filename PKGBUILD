# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer:  Truocolo <truocolo@aol.com>
# Maintainer:  Pellegrino Prevete <pellegrinoprevete@gmail.com>

_os="$( \
  uname \
    -o)"
_git=false
_offline=false
_proj="hip"
_py="python"
_pkg="aioetherscan"
_pkgname="${_py}-${_pkg}"
pkgname="${_pkgname}-git"
pkgver="0.9.5".r0.g"5bfbe39f92bfd672053699b540cc102ae2f48f0c"
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
_host="https://${_gh}"
_ns='themartiancompany'
_local="file://${HOME}/${_pkg}"
url="${_host}/${_ns}/${_pkg}"
_gh_api="https://api.${_gh}/repos/${_ns}/${_pkg}"
license=(
  AGPL3
)
depends=(
  "${_py}>=3.9"
  "${_py}-aiohttp>=3.4"
  "${_py}-asyncio-throttle>=1.0.1"
  "${_py}-aiohttp-retry>=2.8.3"
)
makedepends=(
  "${_py}-build"
  "${_py}-installer"
  "${_py}-poetry"
  "${_py}-poetry-core"
  "${_py}-wheel"
)
checkdepends=(
  shellcheck
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
)
groups=(
 "${_proj}"
 "${_proj}-git"
)
_url="${url}"
[[ "${_offline}" == true ]] && \
  _url="${_local}"
source=()
_branch="master"
[[ "${_git}" == true ]] && \
  makedepends+=(
    git
  ) && \
  source+=(
    "${_pkg}-${_branch}::git+${_url}#branch=${_branch}"
  )
[[ "${_git}" == false ]] && \
  makedepends+=(
    curl
    jq
  ) && \
  source+=(
    "${_pkg}.tar.gz::${_url}/archive/refs/heads/${_branch}.tar.gz"
  )
sha256sums=(
  SKIP
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
    "${_pkg}-${_branch}"
  if [[ "${_git}" == true ]]; then
    _git_pkgver
  elif [[ "${_git}" == false ]]; then
    _jq_pkgver
  fi
}

build() {
  cd \
    "${_pkg}-${_branch}"
  poetry \
    build
  # python \
  #   -m \
  #     build \
  #   --wheel \
  #   --no-isolation
}

package() {
  cd \
    "${_pkg}-${_branch}"
  "${_py}" \
    -m \
      installer \
      --destdir="${pkgdir}" \
      dist/*.whl
  install \
    -Dm644 \
    LICENSE \
    "${pkgdir}"/usr/share/licenses/${pkgname}/LICENSE
}

# vim: ft=sh syn=sh et
