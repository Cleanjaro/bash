# Maintainer:  David Spink <yorper_protonmail.com>
# Contributor: Bernhard Landauer <oberon@manjaro.org>
# Contributor: Bartłomiej Piotrowski <bpiotrowski@archlinux.org>
# Contributor: Allan McRae <allan@archlinux.org>
# Contributor: Aaron Griffin <aaron@archlinux.org>

pkgbase=bash
pkgname=('bash' 'bashrc-cleanjaro')
_basever=5.0
_patchlevel=016
pkgver=${_basever}.${_patchlevel}
pkgrel=1
arch=('x86_64')
license=('GPL')
url='http://www.gnu.org/software/bash/bash.html'
source=(https://ftp.gnu.org/gnu/bash/bash-$_basever.tar.gz{,.sig}
    'dot.bashrc'
    'dot.bash_profile'
    'dot.bash_logout'
    'system.bashrc'
    'system.bash_logout')
sha256sums=('b4a80f2ac66170b2913efbfb9f2594f1f76c7b1afd11f799e22035d63077fb4d'
            'SKIP'
            'b8b6fc5a617e1c0ee5e91e9f35e3d6d2ea81d09ec060bef2bc4b86c91a2794e9'
            'e149407c2bee17779caec70a7edd3d0000d172e7e4347429b80cb4d55bcec9c2'
            '4330edf340394d0dae50afb04ac2a621f106fe67fb634ec81c4bfb98be2a1eb5'
            '5fdc20c44bc9058f728d11111327f4dbb5598fec4d948dd5265211598667f9f0'
            '025bccfb374a3edce0ff8154d990689f30976b78f7a932dc9a6fcef81821811e'
            'f2fe9e1f0faddf14ab9bfa88d450a75e5d028fedafad23b88716bd657c737289'
            'SKIP'
            '87e87d3542e598799adb3e7e01c8165bc743e136a400ed0de015845f7ff68707'
            'SKIP'
            '4eebcdc37b13793a232c5f2f498a5fcbf7da0ecb3da2059391c096db620ec85b'
            'SKIP'
            '14447ad832add8ecfafdce5384badd933697b559c4688d6b9e3d36ff36c62f08'
            'SKIP'
            '5bf54dd9bd2c211d2bfb34a49e2c741f2ed5e338767e9ce9f4d41254bf9f8276'
            'SKIP'
            'd68529a6ff201b6ff5915318ab12fc16b8a0ebb77fda3308303fcc1e13398420'
            'SKIP'
            '17b41e7ee3673d8887dd25992417a398677533ab8827938aa41fad70df19af9b'
            'SKIP'
            'eec64588622a82a5029b2776e218a75a3640bef4953f09d6ee1f4199670ad7e3'
            'SKIP'
            'ed3ca21767303fc3de93934aa524c2e920787c506b601cc40a4897d4b094d903'
            'SKIP'
            'd6fbc325f0b5dc54ddbe8ee43020bced8bd589ddffea59d128db14b2e52a8a11'
            'SKIP'
            '2c4de332b91eaf797abbbd6c79709690b5cbd48b12e8dfe748096dbd7bf474ea'
            'SKIP'
            '2943ee19688018296f2a04dbfe30b7138b889700efa8ff1c0524af271e0ee233'
            'SKIP'
            'f5d7178d8da30799e01b83a0802018d913d6aa972dd2ddad3b927f3f3eb7099a'
            'SKIP'
            '5d6eee6514ee6e22a87bba8d22be0a8621a0ae119246f1c5a9a35db1f72af589'
            'SKIP'
            'a517df2dda93b26d5cbf00effefea93e3a4ccd6652f152f4109170544ebfa05e'
            'SKIP'
            'ffd1d7a54a99fa7f5b1825e4f7e95d8c8876bc2ca151f150e751d429c650b06d'
            'SKIP')
            
validpgpkeys=('7C0135FB088AAF6C66C650B9BB5869F064EA74AB') # Chet Ramey

if [[ $((10#${_patchlevel})) -gt 0 ]]; then
    for (( _p=1; _p<=$((10#${_patchlevel})); _p++ )); do
    source=(${source[@]} https://ftp.gnu.org/gnu/bash/bash-$_basever-patches/bash${_basever//.}-$(printf "%03d" $_p){,.sig})
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
    conflicts=('bashrc-cleanjaro-kde')
    install -Dm644 system.bashrc "$pkgdir"/etc/bash.bashrc
    install -Dm644 dot.bashrc "$pkgdir"/etc/skel/.bashrc
}

