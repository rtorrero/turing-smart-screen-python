# Maintainer: swweetp <swweetp@outlook.com>
pkgname=turing-smart-screen-python
pkgver=3.9.7
pkgrel=1
epoch=
pkgdesc="A Python system monitor program and an abstraction library for small IPS USB-C (UART) displays (Unofficial open-source alternative version)"
arch=('any')
url="https://github.com/mathoudebine/turing-smart-screen-python"
license=('GPL-3.0-or-later')
groups=()
depends=(
	python
	python-pyserial
	python-yaml
	python-psutil
	python-pystray
	python-babel
	python-ruamel-yaml
	python-sv-ttk
	python-tkinter-tooltip
	python-uptime
	python-requests
	python-ping3
	python-pillow
	python-numpy
	python-gputil
	bash
	tk
)
makedepends=(
	python-virtualenv
)
checkdepends=()
optdepends=(
	'python-pyamdgpuinfo: Support for AMD GPUs'
)
provides=()
conflicts=()
replaces=()
backup=(
	"opt/$pkgname/config.yaml"
)
options=()
install="${pkgname}.install"
changelog=
source=(
	"$pkgname-$pkgver.tar.gz::https://github.com/mathoudebine/${pkgname}/archive/refs/tags/${pkgver}.tar.gz"
	"${pkgname%-python}"
	"sysusers.conf"
	"tmpfiles.conf"
	"udev.rules"
	"python3.14-support.patch"
	"subprocess-venv-python.patch"
)
noextract=()
sha256sums=('88c0780e09dad6e1ec38914f780ad49f3da1679795de3434e97276c1844fe5ba'
            'cb274a8de9b87f4af204cba32367d5b8f713e283785ab88025a301bcac196fb5'
            'e648b026686611231538e1e67d32c1d9879da47d427f0d34c13e870b154506cf'
            'fa172b5ab1fbcaaf8b6f21e9080d12e27333a99863680fd768789ba7bafb1ae2'
            '3d3749981af15fcdacda784a159c4970ca8c6316dedd2eab477939ac97071f2c'
            'SKIP'
            'SKIP')
validpgpkeys=()

prepare() {
	cd "$srcdir/turing-smart-screen-python-$pkgver"
	patch -p1 < "$srcdir/python3.14-support.patch"
	patch -p1 < "$srcdir/subprocess-venv-python.patch"
}

package() {
	install -Dm755 "${pkgname%-python}" -t "$pkgdir/usr/bin/"
	
	install -dm755 "$pkgdir/usr/lib/sysusers.d/"
	install -Dm644 "sysusers.conf" "$pkgdir/usr/lib/sysusers.d/${pkgname%-python}.conf"
	install -dm755 "$pkgdir/usr/lib/tmpfiles.d/"
	install -Dm644 "tmpfiles.conf" "$pkgdir/usr/lib/tmpfiles.d/${pkgname%-python}.conf"
	install -Dm644 "udev.rules" "$pkgdir/usr/lib/udev/rules.d/65-${pkgname%-python}.rules"

	cd "$pkgname-$pkgver"
	install -dm755 "$pkgdir/opt/"
	cp -a . "$pkgdir/opt/$pkgname/"

	chmod 664 "$pkgdir/opt/$pkgname/config.yaml"

	# Create venv and install Python dependencies
	cd "$pkgdir/opt/$pkgname"
	python -m venv --system-site-packages venv
	
	# Install pip dependencies (excluding pyinstaller which is only for building)
	"$pkgdir/opt/$pkgname/venv/bin/pip" install --no-cache-dir \
		pyserial~=3.5 \
		PyYAML~=6.0.3 \
		psutil~=7.2.1 \
		pystray~=0.19.5 \
		babel~=2.17.0 \
		ruamel.yaml~=0.19.1 \
		sv-ttk~=2.6.1 \
		tkinter-tooltip~=3.1.2 \
		uptime~=3.0.1 \
		requests~=2.32.5 \
		ping3~=5.1.5 \
		"pillow~=12.1.0" \
		"numpy~=2.4.1" \
		"GPUtil @ git+https://github.com/mathoudebine/gputil.git@1.4.1-py3.13"
}
