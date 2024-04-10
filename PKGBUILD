pkgname=github-cli
_pkgname=cli
pkgver=2.47.0
pkgrel=1
pkgdesc="The GitHub CLI"
arch=("x86_64")
url="https://github.com/cli/cli"
license=("MIT")
depends=("glibc" "mailcap")
makedepends=("go" "git")
checkdepends=("openssh")
optdepends=("git: To interact with remote repositories")
source=("${pkgname}-${pkgver}.tar.gz::${url}/archive/v${pkgver}.tar.gz")
sha256sums=('SKIP')

build() {
    cd "${srcdir}/${_pkgname}-${pkgver}"

    export CGO_CPPFLAGS="${CPPFLAGS}"
    export CGO_CFLAGS="${CFLAGS}"
    export CGO_CXXFLAGS="${CXXFLAGS}"
    export CGO_LDFLAGS="${LDFLAGS}"
    export CGO_ENABLED="0"
    export GOFLAGS="-buildmode=pie -trimpath -mod=readonly -modcacherw"

    msg2 "Adding Bash, Fish, ZSH SHELL completions"
    make GH_VERSION="v${pkgver}" ./bin/gh manpages
    ./bin/gh completion -s bash | install -v -Dm644 "/dev/stdin" "./share/bash-completion/completions/gh"
    ./bin/gh completion -s zsh | install -v -Dm644 "/dev/stdin" "./share/zsh/site-functions/_gh"
    ./bin/gh completion -s fish | install -v -Dm644 "/dev/stdin" "./share/fish/vendor_completions.d/gh.fish"
}

check(){
    make -C "${srcdir}/${_pkgname}-${pkgver}" test || true
}

package() {
    msg2 "Installing and linking needed files from SOURCE to the system"
    make -C "${srcdir}/${_pkgname}-${pkgver}" DESTDIR="${pkgdir}" prefix="/usr/" install
    cp -v -r "${srcdir}/${_pkgname}-${pkgver}/share/" -t "${pkgdir}/usr/"
    ln -v -f -s "/usr/bin/gh" "${pkgdir}/usr/bin/github-cli"
    ln -v -f -s "/usr/bin/gh" "${pkgdir}/usr/bin/gh-cli"
    install -v -Dm644 "${srcdir}/${_pkgname}-${pkgver}/LICENSE" -t "${pkgdir}/usr/share/licenses/${pkgname}/"
    install -v -Dm644 "${srcdir}/${_pkgname}-${pkgver}/README.md" -t "${pkgdir}/usr/share/doc/${pkgname}/"
}
