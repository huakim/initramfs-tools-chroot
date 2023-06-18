# Maintainer bunnylo1 <bunnylo12@yahoo.com>

pkgbase='initramfs-tools-chroot'
_pkgmain='initramfs-tools'
_pkgcore="${_pkgmain}-core"
_pkgbin="${_pkgmain}-bin"
pkgname=(
"${_pkgmain}"
"${_pkgcore}"
"${_pkgbin}"
)
pkgver='0.144'
pkgrel='mpr'
arch=(any)
depends=()
makedepends=(
    'patch'
    "make"
    "c-compiler"
    'coreutils'
    'pkg-config' 
    'libudev-dev'
)


source=(
    git+"https://git.launchpad.net/ubuntu/+source/initramfs-tools"
    'chroot_patch.patch'
)

sha256sums=( 
    'SKIP'
    'b2fecc07c9b8a4b49bd997faf8e99bd97c5d62ecf64492607860f662e80f85e8'
)

prepare(){
    cd initramfs-tools
    patch -p1 -t -i ../chroot_patch.patch || :
}

shopt -sq expand_aliases

alias _pkg_main="package_${_pkgmain}"

alias _pkg_bin="package_${_pkgbin}"

alias _pkg_core="package_${_pkgcore}"

_install(){
    local base dest src
    if [[ -z "${2}" ]]; then
        dest="${1}"
    else 
        base="$(basename "${1}")"
        dest="${2}/${base}"
    fi
    src="${3}/${1}"
    dest="${4}/${dest}"
    echo 'src'  $src 'dest' $dest
    mkdir -p "$(dirname ${dest})" || :
    cp -RTfvp "${src}" "${dest}" || :
}

_copy(){
    local src line base dest
    local -a arr
    for i in postinst preinst postrm prerm triggers; do
        src="${1}/debian/${2}.$i"
        if [[ -f "${src}" ]]; then
            install -Dm755 "${src}" "${3}/DEBIAN/$i" || :
        fi
    done
    
    src="${1}/debian/${2}.install"
    
    if [[ -f "${src}" ]]; then
    sed -n '/^[[:space:]]*#/!p' "${src}" | while read line 
    do
        arr=($line)
        echo "${arr[0]}" 
        echo "${arr[1]}" 
        _install "${arr[0]}" "${arr[1]}" "${1}" "${3}"
        #arr=($line)
        #if [[ -z "${arr[1]}" ]]; then
            #dest="${arr[0]}"
        #else 
            #base="$(basename "${arr[0]}")"
            #dest="${arr[1]}/${base}"
        #fi
        #src="${1}/${arr[0]}"
        #dest="${3}/${dest}"
        #mkdir -p "$(dirname ${dest})"
        #cp -RTfvp "${src}" "${dest}"
    done
    fi
    
    src="${1}/debian/${2}.dirs"
    
    if [[ -f "${src}" ]]; then
    sed -n '/^[[:space:]]*#/!p' "${src}" | while read line 
    do
        dest="${3}/${line}"
        if ! [[ -e "${dest}" ]]; then
            mkdir -p "${dest}" || :
        fi
    done
    fi
}

build(){
    cd "${srcdir}/${_pkgmain}/src"
    make 
}

_pkg_bin(){
    _copy "${srcdir}/${_pkgmain}" "${pkgname}" "${pkgdir}" 
    
    pkgdesc="binaries used by initramfs-tools
This package contains binaries used inside the initramfs images generated
by initramfs-tools."
    depends=(
    "libc6"
    "libgcc-s1" 
    "libudev1"
    )
 #   _copy "${src}/src" "/wait-for-root" "${pkgdir}/usr/lib/initramfs-tools/bin"
 #   _copy "${src}/src" "/gcc_s1-stub" "${pkgdir}/usr/lib/initramfs-tools/bin"
}

_pkg_core(){
    _copy "${srcdir}/${_pkgmain}" "${pkgname}" "${pkgdir}" 
    _install mkinitramfs "sbin" "${srcdir}/${_pkgmain}" "${pkgdir}"
    pkgarch=all
    multiarch=foreign
    pkgdesc="generic modular initramfs generator (core tools)
This package contains the mkinitramfs program that can be used to
create a bootable initramfs for a Linux kernel.  The initramfs should
be loaded along with the kernel and is then responsible for mounting
the root filesystem and starting the main init system."
    
    depends=(
    "klibc-utils" 
    "cpio" 
    "kmod" 
    "udev" 
    "coreutils" 
    "logsave|e2fsprogs" 
    )
# _copy "${src}" "/lsinitramfs" "${pkgdir}/usr/bin"
# _copy "${src}" "/unmkinitramfs" "${pkgdir}/usr/bin"	
# _copy "${src}" "/init"	"${pkgdir}/usr/share/initramfs-tools"
# _copy "${src}" "/scripts" "${pkgdir}/usr/share/initramfs-tools"
# _copy "${src}" "/conf/initramfs.conf" "${pkgdir}/etc/initramfs-tools"
# _copy "${src}" "/hooks" "${pkgdir}/usr/share/initramfs-tools"
# _copy "${src}" "/hook-functions" "${pkgdir}/usr/share/initramfs-tools"
# _copy "${src}" "/conf/modules" "${pkgdir}/usr/share/initramfs-tools"
    
}

_pkg_main(){
    _copy "${srcdir}/${_pkgmain}" "${pkgname}" "${pkgdir}" 

    #/debian/${pkgname}.$i" "${_pkgmain}/DEBIAN/$i" || :
        
    pkgarch=all
    multiarch=foreign
    #cp "${srcdir}/${pkgbase}/debian/${pkgbase}".* "${pkgdir}/DEBIAN/"
    depends=(
    "initramfs-tools-core=${pkgver}-${pkgrel}"
    "linux-base"
    )

    pkgdesc="generic modular initramfs generator (automation)
This package builds a bootable initramfs for Linux kernel packages. The
initramfs is loaded along with the kernel and is responsible for
mounting the root filesystem and starting the main init system."

# _copy "${src}" "/conf/update-initramfs.conf" "${pkgdir}/etc/initramfs-tools"
# _copy "${src}" "/update-initramfs" "${pkgdir}/usr/sbin"
# _copy "${src}" "/debian/script" "${pkgdir}/usr/share/bug/initramfs-tools"
# _copy "${src}" "/kernel" "${pkgdir}/etc"

}
