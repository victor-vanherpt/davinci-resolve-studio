# Maintainer: Victor van Herpt <victor@codibit.es>
# Contributor: Alex S. <shantanna_at_hotmail_dot_com>
# Contributor: Jonathon Fernyhough <jonathon_at_manjaro_dot_org>

# You'll need to download the package archive from
# https://www.blackmagicdesign.com/products/davinciresolve

# Hardware support is limited. Nvidia cards should work fine.
# If you're running a hybrid setup, try with primusrun/optirun.

pkgname=davinci-resolve
_pkgname=resolve
pkgver=15.2.2
pkgrel=4
pkgdesc='Professional A/V post-production software suite'
arch=('x86_64')
url="https://www.blackmagicdesign.com/"
license=('Commercial')
depends=('glu' 'gtk2' 'gstreamer' 'libpng12' 'lib32-libpng12' 'ocl-icd' 'openssl-1.0'
         'opencl-driver' 'qt4' 'qt5-base' 'qt5-svg' 'qt5-webkit'
         'qt5-webengine' 'qt5-websockets')
makedepends=('xdg-user-dirs' 'unzip' 'libisoburn')
options=('!strip')
conflicts=('davinci-resolve-beta' 'davinci-resolve-studio' 'davinci-resolve-studio-beta')
install='davinci-resolve.install'
source=("davinci-resolve.install" "davinci-resolve.desktop")
sha256sums=('be3ed61881c2a74b60237e4003c8147423916b1555140b5b02b7bc64975b4bbe' '1552d5921113cdd23bc162159468c4eb908c54c0eb91f447cc3e55264880204b' 
'5190c0c42d3c84ae4691c73b6fe28e7f471da6a247e7400e7b5181a6c0c81bee' '9e6471ed9e7ef8dbc70ae8f67dd21a2003768d71c566e83bd46719da4ae1e224' )

prepare(){
	_archive="DaVinci_Resolve_${pkgver}_Linux.zip"
    	_archive_sha256sum='4330673cbe62f1ce2292d0357e20503233124bbb5a1b7752ce83b4befcf29497'

	DOWNLOADS_DIR=`xdg-user-dir DOWNLOAD`

	if [ ! -f ${srcdir}/${_archive} ]; then
		if [ -f $DOWNLOADS_DIR/${_archive} ]; then
		    ln -sfn $DOWNLOADS_DIR/${_archive} ${srcdir}
		else
		    msg2 "The package archive can be downloaded here: https://www.blackmagicdesign.com/products/davinciresolve/"
		    msg2 "Please remember to put a downloaded package ${_archive} into the build directory or $DOWNLOADS_DIR"
		    exit 1
		fi
	fi

# check integrity
	if ! echo "${_archive_sha256sum} ${srcdir}/${_archive}" | sha256sum -c --quiet; then
	echo "Invalid checksum for ${_archive}"
	return 1
	fi

	    
# extract package
	    unzip ${srcdir}/${_archive}
}

package() {


# Extract DaVinci Resolve Archive
	mkdir -p "${srcdir}/unpack"

# Create directories
	mkdir -p "${pkgdir}/opt/${_pkgname}/"{configs,easyDCP,logs,scripts,.LUT,.license,.crashreport,DolbyVision,Fairlight,Media,"Resolve Disk Database"}

# Extract DaVinci Resolve Archive
	xorriso -osirrox on -indev "${srcdir}/DaVinci_Resolve_${pkgver}_Linux.run" -extract / "${srcdir}/unpack"

# Copy objects

	cp -rp "${srcdir}/unpack/bin" "${pkgdir}/opt/${_pkgname}/"
	cp -rp "${srcdir}/unpack/Control" "${pkgdir}/opt/${_pkgname}/"
	cp -rp "${srcdir}/unpack/DaVinci Resolve Panels Setup" "${pkgdir}/opt/${_pkgname}/"
	cp -rp "${srcdir}/unpack/Developer" "${pkgdir}/opt/${_pkgname}/"
	cp -rp "${srcdir}/unpack/docs" "${pkgdir}/opt/${_pkgname}/"
	cp -rp "${srcdir}/unpack/Fusion" "${pkgdir}/opt/${_pkgname}/"
	cp -rp "${srcdir}/unpack/graphics" "${pkgdir}/opt/${_pkgname}/"
	cp -rp "${srcdir}/unpack/libs" "${pkgdir}/opt/${_pkgname}/"
	cp -rp "${srcdir}/unpack/LUT" "${pkgdir}/opt/${_pkgname}/"
	cp -rp "${srcdir}/unpack/Onboarding" "${pkgdir}/opt/${_pkgname}/"
	cp -rp "${srcdir}/unpack/plugins" "${pkgdir}/opt/${_pkgname}/"
	cp -rp "${srcdir}/unpack/UI_Resource" "${pkgdir}/opt/${_pkgname}/"
    #scripts
	cp -p "${srcdir}/unpack/scripts/script.checkfirmware" "${pkgdir}/opt/${_pkgname}/scripts"
	cp -p "${srcdir}/unpack/scripts/script.getlogs.v4" "${pkgdir}/opt/${_pkgname}/scripts"
	cp -p "${srcdir}/unpack/scripts/script.start" "${pkgdir}/opt/${_pkgname}/scripts"
    #configs
	cp -rp "${srcdir}/unpack/share/default-config-linux.dat" "${pkgdir}/opt/${_pkgname}/configs/config.dat-pkg-default"
	cp -rp "${srcdir}/unpack/share/log-conf.xml" "${pkgdir}/opt/${_pkgname}/configs/log-conf.xml-pkg-default"
	cp -rp "${srcdir}/unpack/share/default_cm_config.bin" "${pkgdir}/opt/${_pkgname}/DolbyVision/config.bin-pkg-default"

#Add lib symlinks
	cd "${pkgdir}/opt/${_pkgname}/" || exit
	ln -s /usr/lib/libcrypto.so.1.0.0  libs/libcrypto.so.10
	ln -s /usr/lib/libssl.so.1.0.0     libs/libssl.so.10
	ln -s /usr/lib/libgstbase-1.0.so   libs/libgstbase-0.10.so.0
	ln -s /usr/lib/libgstreamer-1.0.so libs/libgstreamer-0.10.so.0

#Set proper permissions
	chmod -R a+rw "${pkgdir}/opt/resolve/configs"
	chmod -R a+rw "${pkgdir}/opt/resolve/easyDCP"
	chmod -R a+rw "${pkgdir}/opt/resolve/logs"
	chmod -R a+rw "${pkgdir}/opt/resolve/Developer"
	chmod -R a+rw "${pkgdir}/opt/resolve/DolbyVision"
	chmod -R a+rw "${pkgdir}/opt/resolve/LUT"
	chmod -R a+rw "${pkgdir}/opt/resolve/.LUT"
	chmod -R a+rw "${pkgdir}/opt/resolve/.license"
	chmod -R a+rw "${pkgdir}/opt/resolve/.crashreport"
	chmod -R a+rw "${pkgdir}/opt/resolve/Resolve Disk Database"
	chmod -R a+rw "${pkgdir}/opt/resolve/Fairlight"
	chmod -R a+rw "${pkgdir}/opt/resolve/Media"


#Install .desktop launcher
	install -Dm644 "${srcdir}/davinci-resolve.desktop" "${pkgdir}/usr/share/applications/DaVinci Resolve.desktop"

}
