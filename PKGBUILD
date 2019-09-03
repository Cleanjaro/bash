# Maintainer:  David Spink <yorper_protonmail.com>
# Contributor: Bernhard Landauer <oberon@manjaro.org>
# Contributor: Bartłomiej Piotrowski <bpiotrowski@archlinux.org>
# Contributor: Allan McRae <allan@archlinux.org>
# Contributor: Aaron Griffin <aaron@archlinux.org>

pkgbase=bash
pkgname=('bash' 'bashrc-cleanjaro')
_basever=5.0
_patchlevel=011
pkgver=${_basever}.${_patchlevel}
pkgrel=1
arch=('x86_64')
license=('GPL')
url='http://www.gnu.org/software/bash/bash.html'
groups=('base')
source=(https://ftp.gnu.org/gnu/bash/bash-$_basever.tar.gz
    'dot.bashrc'
    'dot.bash_profile'
    'dot.bash_logout'
    'system.bashrc'
    'system.bash_logout')
md5sums=('2b44b47b905be16f45709648f671820b'
         'd79993186d59c9fffab128eb888c9fcb'
         '2902e0fee7a9168f3a4fd2ccd60ff047'
         '42f4400ed2314bd7519c020d0187edc5'
         'd8f3f334e72c0e30032eae1a1229aef1'
         '472f536d7c9e8250dc4568ec4cfaf294'
         'b026862ab596a5883bb4f0d1077a3819'
         '2f4a7787365790ae57f36b311701ea7e'
         'af7f2dd93fd5429fb5e9a642ff74f87d'
         'b60545b273bfa4e00a760f2c648bed9c'
         '875a0bedf48b74e453e3997c84b5d8a4'
         '4a8ee95adb72c3aba03d9e8c9f96ece6'
         '411560d81fde2dc5b17b83c3f3b58c6f'
         'dd7cf7a784d1838822cad8d419315991'
         'c1b3e937cd6dccbb7fd772f32812a0da'
         '19b41e73b03602d0e261c471b53e670c'
         '414339330a3634137081a97f2c8615a8')

if [[ $((10#${_patchlevel})) -gt 0 ]]; then
    for (( _p=1; _p<=$((10#${_patchlevel})); _p++ )); do
    source=(${source[@]} https://ftp.gnu.org/gnu/bash/bash-$_basever-patches/bash${_basever//.}-$(printf "%03d" $_p))
    done
fi

prepare() {
    cd $pkgbase-$_basever
    for (( _p=1; _p<=$((10#${_patchlevel})); _p++ )); do
    msg "applying patch bash${_basever//.}-$(printf "%03d" $_p)"
    patch -p0 -i ../bash${_basever//.}-$(printf "%03d" $_p)
    done
}

build() {
    cd $pkgbase-$_basever
    _bashconfig=(-DDEFAULT_PATH_VALUE=\'\"/usr/local/sbin:/usr/local/bin:/usr/bin\"\'
               -DSTANDARD_UTILS_PATH=\'\"/usr/bin\"\'
               -DSYS_BASHRC=\'\"/etc/bash.bashrc\"\'
               -DSYS_BASH_LOGOUT=\'\"/etc/bash.bash_logout\"\'
               -DNON_INTERACTIVE_LOGIN_SHELLS)
    export CFLAGS="${CFLAGS} ${_bashconfig[@]}"

    ./configure --prefix=/usr --with-curses --enable-readline \
    --without-bash-malloc --with-installed-readline
    make
}

check() {
    make -C $pkgname-$_basever check
}

package_bash() {
    pkgdesc='The GNU Bourne Again shell'
    backup=(etc/bash.bash_logout etc/skel/.bash{_profile,_logout})
    depends=('bashrc'
        'glibc'
        'ncurses'
        'readline>=7.0'
        'bashrc-cleanjaro')
    optdepends=('bash-completion: for tab completion')
    provides=('sh')
    make -C $pkgname-$_basever DESTDIR="$pkgdir" install
    ln -s bash "$pkgdir"/usr/bin/sh

    # system-wide configuration files
    install -Dm644 system.bash_logout "$pkgdir"/etc/bash.bash_logout

    # user configuration file skeletons
    install -Dm644 dot.bash_profile "$pkgdir"/etc/skel/.bash_profile
    install -m644 dot.bash_logout "$pkgdir"/etc/skel/.bash_logout
}

package_bashrc-cleanjaro() {
    pkgdesc="Cleanjaro's default bashrc"
    arch=('any')
    backup=('etc/bash.bashrc' 'etc/skel/.bashrc')
    depends=('bash')
    provides=('bashrc')
    replaces=('bashrc-manjaro')
    install -Dm644 system.bashrc "$pkgdir"/etc/bash.bashrc
    install -Dm644 dot.bashrc "$pkgdir"/etc/skel/.bashrc
}
