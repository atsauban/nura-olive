maintainer="Barnabas Czeman <barnabas.czeman@postmarketos.org>"
_flavor="postmarketos-qcom-msm89x7"
pkgname=linux-postmarketos-qcom-msm89x7
pkgver=7.1.3
pkgrel=20
pkgdesc="Mainline kernel fork for Qualcomm QM215/MSM8917/MSM8937/MSM8940/SDM429/SDM439 devices"
arch="aarch64"
_carch="arm64"
url="https://github.com/msm89x7-mainline/linux"
license="GPL-2.0-only"
options="!strip !check !tracedeps
	pmb:cross-native
	pmb:kconfigcheck-nftables
"
makedepends="
	bison
	clang
	elfutils-dev
	findutils
	flex
	linux-headers
	lld
	llvm
	openssl-dev
	pahole
	perl
	postmarketos-installkernel
	python3
	zstd
"
_tag="$pkgver-r1"
_config="config-$_flavor.$arch"
source="
	$pkgname-v$_tag.tar.gz::$url/archive/v$_tag.tar.gz
	$_config
	0001-touchscreen-fts-olive.patch
	0002-dts-enable-spi-touch-olive.patch
	0003-generic-adc-battery-overflow-capacity.patch
	0004-backlight-ktd3136.patch
"
builddir="$srcdir/linux-$_tag"

prepare() {
	default_prepare
	cp "$srcdir/config-$_flavor.$arch" .config
}

build() {
	unset LDFLAGS
	make ARCH="$_carch" LLVM=1 \
		KBUILD_BUILD_VERSION="$((pkgrel + 1 ))-postmarketOS"
}

package() {
	mkdir -p "$pkgdir"/boot
	make zinstall modules_install dtbs_install \
		ARCH="$_carch" \
		LLVM=1 \
		INSTALL_PATH="$pkgdir"/boot \
		INSTALL_MOD_PATH="$pkgdir"/usr \
		INSTALL_MOD_STRIP=1 \
		INSTALL_DTBS_PATH="$pkgdir"/boot/dtbs
	rm -f "$pkgdir"/usr/lib/modules/*/build "$pkgdir"/usr/lib/modules/*/source

	install -D "$builddir"/include/config/kernel.release \
		"$pkgdir"/usr/share/kernel/$_flavor/kernel.release
}
sha512sums="
c4bebf0d92f803297004d14573fbf0ccca5fc8927944f0aeecd767472ca21639ec931b6648b5601ec46eb49dd489939ae00e17f36933307bcdd371b87059796a  linux-postmarketos-qcom-msm89x7-v7.1.3-r1.tar.gz
52c15ac54e15a7062ff144c192580e99e1134fd1926c65376b117997b0c69d791c7499e84a38565488ee7b1516fad018c5c04881bcee30cc7cc161c9457da466  config-postmarketos-qcom-msm89x7.aarch64
e3f5cb596838e4fba7c3afcba213a5050c1e315d3f8d72df8c78655cea2ae58b540887aff1efd051e5a07eca3b90d38c8de0cee10adea2c831e731d611ee5dc7  0001-touchscreen-fts-olive.patch
8f9c1f2b13ecfc2ca5190a770b6d4db632fc05c1b816d1d7b9edd5380d785fb4635ea9a7d07f34c456a1e42d2ba4a4ff0aa7654c6105bbefb39f484f957f367e  0002-dts-enable-spi-touch-olive.patch
5da743128e2815607490096755813b4a59ec56bf60aab9da362a8ddfbe6f379779ec1cdeee68df29d602b64b1fdc999cd45285c2ff48f9e7e83c9625a45b7542  0003-generic-adc-battery-overflow-capacity.patch
ce9b4f0c349378105cb1cb45aab8d58042d5167fff29131f64bcfc0490c5ebc9360cf2393dec29a0d76571f4ea4f86d8be10af1a9c140170a9d20b2a7f57bd2f  0004-backlight-ktd3136.patch
"
