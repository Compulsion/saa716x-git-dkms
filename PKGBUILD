_pkgbase=saa716x-git
pkgname=${_pkgbase}-dkms
pkgver=r1.b62daa4
pkgrel=1
pkgdesc="Trimmed down saa716x driver for tbs6281."
url="https://github.com/ljalves/linux_media/wiki"
arch=('i686' 'x86_64')
license=('GPL')
makedepends=('git')
depends=('dkms')
conflicts=("${_pkgbase}" 'tbs-dvb-drivers' 'tbs-linux_media-git-dkms' 'tbs-linux_media-git')
provides=("${_pkgbase}")

source=('git+https://github.com/Compulsion/saa716x.git'
	'dvb-demod-si2168-b40-01.fw'
	'dvb-tuner-si2158-a20-01.fw'
	'dkms.conf')

md5sums=('SKIP'
	'8dfc2483d90282bbb05817fbbc282376'
	'0cba7ce61c1411cbe7f22c0746e24e33'
	'6c3707d9c554a0d6e7d02ddfbeb23888')

pkgver() {
	cd "$srcdir/saa716x"
	( set -o pipefail
		git describe --long 2>/dev/null | sed 's/\([^-]*-g\)/r\1/;s/-/./g' ||
		printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
	)
}

package() {
	install -Dm0644 "$srcdir"/dvb-demod-si2168-b40-01.fw "$pkgdir"/usr/lib/firmware/dvb-demod-si2168-b40-01.fw
	install -Dm0644 "$srcdir"/dvb-tuner-si2158-a20-01.fw "$pkgdir"/usr/lib/firmware/dvb-tuner-si2158-a20-01.fw
	install -Dm0644 "$srcdir/dkms.conf" "$pkgdir/usr/src/$_pkgbase-$pkgver/dkms.conf"
	echo ""
	msg "Compressing modules, this will take awhile..."
	echo ""

	# Set name and version
	sed -e "s/@_PKGBASE@/${_pkgbase}/" \
	-e "s/@PKGVER@/${pkgver}/" \
	-i "${pkgdir}/usr/src/${_pkgbase}-${pkgver}/dkms.conf"

	# Copy sources
	mkdir -p "${pkgdir}/usr/src/${_pkgbase}-${pkgver}/"
	cp -r "${srcdir}/saa716x"/* "${pkgdir}/usr/src/${_pkgbase}-${pkgver}/"
}
