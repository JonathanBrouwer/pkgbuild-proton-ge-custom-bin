## Maintainer:     barfin <barfin@protonmail.com>
## Co-Maintainer:  Jaja <jaja@mailbox.org>
## Co-Maintainer:  floriplum <floriplum@mailbox.org>

## pkginfo
pkgdesc="A fancy custom distribution of Valves Proton with various patches"
pkgname=proton-ge-custom-bin
pkgver=GE_Proton7_5
pkgrel=1
epoch=1
arch=('x86_64')
license=('BSD' 'LGPL' 'zlib' 'MIT' 'MPL' 'custom')
changelog=changelog.md
provides=('proton' "proton-ge-custom=${pkgver/_/.}")
conflicts=('proton-ge-custom')

## dependencies
depends=('python'
         'vulkan-icd-loader'
         'lib32-libusb'
         'lib32-openal'
#         # libav support #
         'lib32-libva'
         'ffmpeg4.4'
         'lib32-speex'
         'lib32-libtheora'
         'lib32-libvdpau'
#         # gstreamer support #
         'gst-plugins-bad-libs'
         'lib32-gst-plugins-base-libs'
         'libjpeg6-turbo'
         'lib32-libjpeg6-turbo'
         'lib32-libgudev'
         'lib32-flac'
         'lib32-mpg123')
optdepends=('kdialog: KDE splash dialog support'
            'zenity: GNOME splash dialog support'
            'python-kivy: splash dialog support (big picture mode)'
            'steam: use proton with steam like intended'
            'lib32-vulkan-icd-loader: dxvk dependency for 32bit prefixes'
            'vulkan-driver: driver to be used by dxvk'
            'winetricks: protonfixes backend - highly recommended'
            'wine: support for 32bit prefixes'
            'xboxdrv: gamepad driver service')

## makepkg options
options=('!strip')

## fix naming conventions, matching upstream
_pkgname=${pkgname//-bin/}
_pkgver=${pkgver//_/-}
_srcdir=${_pkgver}

## paths and files
_protondir=usr/share/steam/compatibilitytools.d/${_pkgname}
_licensedir=usr/share/licenses/${pkgname}
_execfile=usr/bin/proton
_protoncfg=${_protondir}/user_settings.py

## user edited files to backup
backup=("${_protoncfg}")

## sources
url='https://github.com/GloriousEggroll/proton-ge-custom'
source=(${_pkgver}_${pkgrel}.tar.gz::"${url}/releases/download/${_pkgver}/${_pkgver}.tar.gz"
        "supplementary.tar.zst")
sha512sums=('bec8eca24029c4e56c7fe3a77a8f08be172f54c377d1a92cce2b8be973e7406801eb79995a173a49b3a350229f45c8e90d2c7eaf13440287905b6066f9a51a4f'
            'a484c4cd2003057cf0cbbd32ca5d0106e97c75434e7bef34b35be8239ad98a482358852e41e85abedf5b24ac4d0375c8fffc7deee81a9b08c7799a398f23773b')

build() {
## patches
sed -i "s|_proton=echo|_proton=/${_protondir}/proton|" ${srcdir}/launchers/proton.sh
sed -i -r 's|"GE-Proton.*"|"Proton-GE"|' ${_srcdir}/compatibilitytool.vdf
## remove artifacts
rm "${_srcdir}"/protonfixes/*.tar.xz
rm -rf "${_srcdir}"/protonfixes/.git*
## fixes from namcap inspection
strip --preserve-dates --strip-unneeded "${_srcdir}"/files/bin/wine*
}

package() {
## create paths
install -d ${pkgdir}/${_protondir}/
install -d ${pkgdir}/${_licensedir}/
install -d ${pkgdir}/$(dirname ${_execfile})/
## licenses
mv ${_srcdir}/LICENSE ${pkgdir}/${_licensedir}/license
mv ${_srcdir}/LICENSE.OFL ${pkgdir}/${_licensedir}/license_OFL
mv ${_srcdir}/PATENTS.AV1 ${pkgdir}/${_licensedir}/license_AV1
mv ${_srcdir}/protonfixes/LICENSE ${pkgdir}/${_licensedir}/license_protonfixes
## config files
install --mode=0775 --group=50 ${srcdir}/configs/user_settings.py ${pkgdir}/${_protoncfg}
## executables
mv ${_srcdir}/* ${pkgdir}/${_protondir}
install --mode=0755 ${srcdir}/launchers/proton.sh ${pkgdir}/${_execfile}
}
