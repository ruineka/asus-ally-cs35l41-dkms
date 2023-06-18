# Maintainer: Matthew Anderson<ruinairas1992 at gmail dot com>
pkgname=asus-ally-dkms
_upstreamver='6.4-rc6'
pkgver="${_upstreamver/-/}"
pkgrel=1
pkgdesc="Fix for Cirrus Amp in the Asus Ally"
arch=('x86_64')
url="https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/tree/sound/soc/amd?h=v$_upstreamver"
license=('GPL2')
depends=('dkms')
makedepends=('git')
source=("https://git.kernel.org/torvalds/t/linux-$_upstreamver.tar.gz"
	'asus-ally-fix.patch'
	'dkms.conf')
noextract=("linux-$_upstreamver.tar.gz")
sha256sums=('SKIP'
	    'SKIP'
	    'SKIP')
validpgpkeys=('8A144654AD99CA9E984F4156EC74EAEC196A648E')

prepare() {
	bsdtar -xf "linux-$_upstreamver.tar.gz" "linux-$_upstreamver/sound/pci/hda"
	cd "$srcdir/linux-$_upstreamver"
	for src in "${source[@]}"; do
	    src="${src%%::*}"
            src="${src##*/}"
            [[ $src = *.patch ]] || continue
            echo "Applying patch $src..."
            git apply "../$src"
  	done
}

package() {

	local DEST="$pkgdir/usr/src/${pkgname%-dkms}-$pkgver"
	local SRC="linux-$_upstreamver/sound/pci/hda/"

	install -dm755 "$DEST"

	for f in "$SRC/"*.[hc] "$SRC/Makefile"; do
		install -m644 "$f" "$DEST"
	done

	install -m644 dkms.conf "$DEST"

	# substitution in dkms.conf
	sed -e 's/PACKAGE_NAME=""/PACKAGE_NAME="'"${pkgname%-dkms}"'"/' \
		  -e 's/PACKAGE_VERSION=""/PACKAGE_VERSION="'"$pkgver"'"/' \
			-i "$DEST/dkms.conf"
}
