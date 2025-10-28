pkgname="pulse-secure-uta"
pkgver=22.8.R2
pkgrel=1
pkgdesc="Pulse Secure VPN for UTA network"
arch=("x86_64")
depends=("glib2" "gtkmm3>=3.18" "webkit2gtk" "curl" "nss")
makedepends=("patch")
#libglib2.0-0, libgtkmm-3.0-1v5 (>= 3.18), libwebkit2gtk-4.0-37, libcurl4, libnss3-tools``
#optdepends=("")
#conflicts=("")

url="https://pulse-vpn.uta.edu/split-tunnel"
license=("unknown")

options=("!debug")
install="helper.install"

source=("$pkgname-$pkgver.deb::https://pulse-vpn.uta.edu/dana-na/jam/getComponent.cgi?command=get&component=PulseSecure&platform=deb")
sha256sums=("5cd66b89a1b07b6be4176ce554a6b5df1857b0aa67852e20f330d98d6cbcbe0b")

# Relative to $srcdir (which is the default working directory for all functions below)
DATA_DIR="data"
CONTROL_DIR="control"

# Fixes directories and stuff
prepare()
{
    # Create needed directories
    mkdir -p "$srcdir"/"$DATA_DIR"
    mkdir -p "$srcdir"/"$CONTROL_DIR"

    # Extract sub-tars (compression type may change in future, don't rely on ending)
    bsdtar -xf data.tar.* -C "$DATA_DIR"
    bsdtar -xf control.tar.* -C "$CONTROL_DIR"

    # /lib symlinks to /usr/lib and pacman requires using /usr/lib directly
    cp -r "$DATA_DIR"/lib/. "$DATA_DIR"/usr/lib
    rm -r "$DATA_DIR"/lib

    # Patch to fix CEF installer script. Credit: chiwanpark (See https://aur.archlinux.org/cgit/aur.git/tree/setup_cef.sh.patch?h=pulse-secure)
    patch "$DATA_DIR"/opt/pulsesecure/bin/setup_cef.sh setup_cef.sh.patch

    # Fix certificate path (pulse expects it to be in /etc/pki) Credit: earendilfr / chiwanpark (See https://aur.archlinux.org/packages/pulse-secure?O=50#comment-879232)
    mkdir -p "$pkgdir"/etc/pki/ca-trust/extracted/openssl/
    ln -sf /etc/ca-certificates/extracted/ca-bundle.trust.crt "$pkgdir"/etc/pki/ca-trust/extracted/openssl/ca-bundle.trust.crt
}

# Returns package version (makepkg uses this to autoupdate pkgver variable above)
pkgver()
{
    # Parse version from .deb "control" manifest file
    sed -z -e 's/.*Version: //' -e 's/\n.*//' "$CONTROL_DIR"/control
}


# Copies output files after prepare() into proper directories
package()
{
    # Copy to output
    cp -r "$DATA_DIR"/. "$pkgdir"

    # Create symlink to executable in /usr/bin
    mkdir -p "$pkgdir"/usr/bin
    ln -s /opt/pulsesecure/bin/pulseUI "$pkgdir"/usr/bin/pulseUI
}
