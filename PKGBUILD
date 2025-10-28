pkgname="pulse-secure-vpn-uta"
pkgver="22.8.2"
pkgrel="1"
pkgdesc="Pulse Secure VPN for UTA network"
arch=("x86_64")
depends=("glib2" "gtkmm3>=3.18" "webkit2gtk" "curl" "nss")
#libglib2.0-0, libgtkmm-3.0-1v5 (>= 3.18), libwebkit2gtk-4.0-37, libcurl4, libnss3-tools``
#optdepends=("")
#conflicts=("")
#license=("custom")

options=("!debug")
install="helper.install"

source=("pulsesecure-${pkgver}.deb::https://pulse-vpn.uta.edu/dana-na/jam/getComponent.cgi?command=get&component=PulseSecure&platform=deb")
sha256sums=("5cd66b89a1b07b6be4176ce554a6b5df1857b0aa67852e20f330d98d6cbcbe0b")
package()
{
    #Must be defined here since srcdir is only defined in this function
    DATA_DIR="${srcdir}/data"
    CONTROL_DIR="${srcdir}/control"

    mkdir -p "${DATA_DIR}" "${CONTROL_DIR}"
    
    #Remove archive
    rm "pulsesecure-${pkgver}.deb"

    #Extract sub-tars
    bsdtar -xf ${srcdir}/data.tar.* -C "${DATA_DIR}"
    bsdtar -xf ${srcdir}/control.tar.* -C "${CONTROL_DIR}"

    # /lib symlinks to /usr/lib and pacman requires using /usr/lib directly
    cp -r "${DATA_DIR}/lib/." "${DATA_DIR}/usr/lib"
    rm -r "${DATA_DIR}/lib"

    #Copy to output
    cp -r "${DATA_DIR}/." "${pkgdir}"

    #Create symlink to executable in /usr/bin
    mkdir -p "${pkgdir}/usr/bin"
    ln -s "/opt/pulsesecure/bin/pulseUI" "${pkgdir}/usr/bin/pulsevpn"

    #TODO: Build .install script from Pulse source (post and pre install hooks)
}