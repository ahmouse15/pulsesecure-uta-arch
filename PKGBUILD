pkgname="pulse-secure-vpn-uta"
pkgver=22.8.R2
pkgrel=1
pkgdesc="Pulse Secure VPN for UTA network"
arch=("x86_64")
depends=("glib2" "gtkmm3>=3.18" "webkit2gtk" "curl" "nss")
#libglib2.0-0, libgtkmm-3.0-1v5 (>= 3.18), libwebkit2gtk-4.0-37, libcurl4, libnss3-tools``
#optdepends=("")
#conflicts=("")
#license=("custom")

options=("!debug")
install="helper.install"

source=("$pkgname.deb::https://pulse-vpn.uta.edu/dana-na/jam/getComponent.cgi?command=get&component=PulseSecure&platform=deb")
sha256sums=("5cd66b89a1b07b6be4176ce554a6b5df1857b0aa67852e20f330d98d6cbcbe0b")

#Must be defined here since srcdir is only defined in a function
DATA_DIR="data"
CONTROL_DIR="control"

#Fixes directories and stuff
prepare()
{
    #Extract sub-tars
    bsdtar -xf data.tar.* -C $DATA_DIR
    bsdtar -xf control.tar.* -C $CONTROL_DIR

    # /lib symlinks to /usr/lib and pacman requires using /usr/lib directly
    cp -r $DATA_DIR/lib/. $DATA_DIR/usr/lib
    rm -r $DATA_DIR/lib
}

#Copies output files after prepare phase into proper directories
package()
{
    #Copy to output
    cp -r $DATA_DIR/. $pkgdir

    #Create symlink to executable in /usr/bin
    mkdir -p $pkgdir/usr/bin
    ln -s /opt/pulsesecure/bin/pulseUI $pkgdir/usr/bin/pulseUI
}

#Returns package version
pkgver()
{
    #Parse version from .deb "control" manifest file
    sed -z -e 's/.*Version: //' -e 's/\n.*//' "${CONTROL_DIR}/control"
}
