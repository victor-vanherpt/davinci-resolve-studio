# Maintainer: Victor van Herpt <victor@codibit.es>
# Contributor: Alex S. <shantanna_at_hotmail_dot_com>
# Contributor: Jonathon Fernyhough <jonathon_at_manjaro_dot_org>

# You'll need to download the package archive from
# https://www.blackmagicdesign.com/products/davinciresolve

# Hardware support is limited. Nvidia cards should work fine.
# If you're running a hybrid setup, try with primusrun/optirun.

pkgname=davinci-resolve-studio
_pkgname=resolve-studio
pkgver=14.2
pkgrel=0
pkgdesc='Professional A/V post-production software suite'
arch=('x86_64')
url="https://www.blackmagicdesign.com/"
license=('Commercial')
depends=('glu' 'gtk2' 'gstreamer' 'libpng12' 'lib32-libpng12' 'ocl-icd' 'openssl-1.0'
         'opencl-driver' 'qt4' 'qt5-base' 'qt5-svg' 'qt5-webkit'
         'qt5-webengine' 'qt5-websockets')
options=('!strip')

if [ ! -f ${pkgdir}/DaVinci_Resolve_Studio_${pkgver}_Linux.zip ]; then
  if [ -f $HOME/Downloads/DaVinci_Resolve_Studio_${pkgver}_Linux.zip ]; then
    ln -sf $HOME/Downloads/DaVinci_Resolve_Studio_${pkgver}_Linux.zip ${pkgdir}
  else
    msg2 "Please remember to put a package archive DaVinci_Resolve_Studio_${pkgver}_Linux.zip in your Downloads directory"
    sleep 3
  fi
fi

source=("local://DaVinci_Resolve_Studio_${pkgver}_Linux.zip")
sha256sums=('dd17a333fcaaa26a4c32c1e57ec0aa904a860d0674a51fc80097042d0aacb209')

package() {


#	msg2 "Did you download the archive manually? If not, this will fail."
	mkdir -p "${pkgdir}/opt/${_pkgname}/"{bin,configs,Media}

	msg2 "Extracting from bundle..."
	cd "${srcdir}" || exit
	./DaVinci_Resolve_Studio_${pkgver}_Linux.sh --tar xf -C "${pkgdir}/opt/${_pkgname}"

	msg2 "Extracting library archives..."
	cd "${pkgdir}/opt/${_pkgname}/" || exit
	gunzip -f LUT/trim_lut0.dpx.gz
	for archive in libs/*tgz; do
		tar xf "${archive}" -C libs/
		rm -f "${archive}"
	done

	msg2 "Relocate binaries and config..."
	cd "${pkgdir}/opt/${_pkgname}/" || exit
	local _binaries=(
		BMDPanelFirmware
		DPDecoder
		ShowDpxHeader
		TestIO
		bandwidthTest
		bmdpaneld
		deviceQuery
		oclBandwidthTest
		oclDeviceQuery
		qt.conf
	)
	for binary in "${_binaries[@]}"; do
		mv "rsf/${binary}" "bin/${binary}"
	done
	mv resolve bin/resolve
	mv rsf/Control Control
	install -Dm666 rsf/default-config-linux.dat "${pkgdir}/opt/${_pkgname}/configs/config.dat"

	msg2 "Add lib symlinks..."
	cd "${pkgdir}/opt/${_pkgname}/" || exit
	ln -s /usr/lib/libcrypto.so.1.0.0  libs/libcrypto.so.10
	ln -s /usr/lib/libssl.so.1.0.0     libs/libssl.so.10
	ln -s /usr/lib/libgstbase-1.0.so   libs/libgstbase-0.10.so.0
	ln -s /usr/lib/libgstreamer-1.0.so libs/libgstreamer-0.10.so.0
	
	msg2 "Creating udev rules"
	# Ensure dongle permissions are correctly set
	SDX_RULES="${srcdir}/75-sdx.rules"
	echo 'SUBSYSTEM=="usb", ENV{DEVTYPE}=="usb_device", ATTRS{idVendor}=="096e", MODE="0666"' > ${SDX_RULES}
	
	install -Dm644 ${SDX_RULES} "${pkgdir}/usr/lib/udev/rules.d/75-sdx.rules"
	
	msg2 "Creating launchers..."
	cd "${srcdir}" || exit
	cat > "${srcdir}/DaVinci Resolve Studio.desktop" << EOF
#!/usr/bin/env xdg-open
[Desktop Entry]
Type=Application
Name=DaVinci Resolve Studio
Comment=Professional non-linear editing
Exec=/opt/${_pkgname}/bin/start-resolve
Icon=/opt/${_pkgname}/rsf/DV_Resolve.png
Terminal=false
Categories=Multimedia;AudioVideo;Application;
EOF
	install -Dm644 DaVinci\ Resolve\ Studio.desktop "${pkgdir}/usr/share/applications/DaVinci Resolve\ Studio.desktop"

	cat > "${srcdir}/start-resolve" << EOF
#!/bin/sh
mkdir -p /tmp/${_pkgname}/{logs,GPUCache}
cd /opt/${_pkgname}
exec bin/resolve "\$@"
EOF
	install -Dm755 start-resolve "${pkgdir}/opt/${_pkgname}/bin/start-resolve"

	msg2 "Making sure file ownership is 'correct'..."
	chown -R root:root "${pkgdir}/opt"
	chmod 0777 "${pkgdir}/opt/${_pkgname}/configs"
	chmod 0777 "${pkgdir}/opt/${_pkgname}/Media"

	msg2 "Any final tweaks..."
	ln -s "/tmp/${_pkgname}/logs" "${pkgdir}/opt/${_pkgname}/logs"
	ln -s "/tmp/${_pkgname}/GPUCache" "${pkgdir}/opt/${_pkgname}/GPUCache"

	msg2 "Done!"
}

# vim: fileencoding=utf-8 sts=4 sw=4 noet