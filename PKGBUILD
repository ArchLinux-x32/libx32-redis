# $Id: PKGBUILD 109955 2014-04-22 14:53:51Z spupykin $
# Maintainer: Sergej Pupykin <pupykin.s+arch@gmail.com>
# Maintainer: Jan-Erik Rediger <badboy at archlinux dot us>
# Contributor: nofxx <x@<nick>.com>

pkgname=redis
pkgver=2.8.9
pkgrel=1
pkgdesc="Advanced key-value store"
arch=('i686' 'x86_64')
url="http://redis.io/"
license=('BSD')
depends=('bash')
makedepends=('gcc>=3.1' 'make' 'pkgconfig')
backup=("etc/redis.conf"
	"etc/logrotate.d/redis")
install=redis.install
source=("http://download.redis.io/releases/redis-$pkgver.tar.gz"
	"redis.service"
	"redis.logrotate"
	"redis.tmpfiles.d")
md5sums=('3c106b0f1128dc930684e2da88b2a03d'
         '5320aa6d0f31aadc1d6202ca40425aea'
         '9e2d75b7a9dc421122d673fe520ef17f'
         'dd9ab8022b4d963b2e5899170dfff490')

prepare() {
  cd "$srcdir/${pkgname}-${pkgver}"
  sed -i 's|# bind 127.0.0.1|bind 127.0.0.1|' redis.conf
  sed -i 's|daemonize no|daemonize yes|' redis.conf
  sed -i 's|dir \./|dir /var/lib/redis/|' redis.conf
  sed -i 's|pidfile .*|pidfile /run/redis/redis.pid|' redis.conf
  sed -i 's|logfile stdout|logfile /var/log/redis.log|' redis.conf
}

build() {
  cd "$srcdir/${pkgname}-${pkgver}"
  make
}

package() {
  cd "$srcdir/${pkgname}-${pkgver}"
  mkdir -p $pkgdir/usr/bin
  make INSTALL_BIN="$pkgdir/usr/bin" PREFIX=/usr install

  install -Dm755 "$srcdir/${pkgname}-${pkgver}/COPYING" "$pkgdir/usr/share/licenses/redis/COPYING"
  install -Dm644 "$srcdir"/redis.service "$pkgdir"/usr/lib/systemd/system/redis.service
  install -Dm644 "$srcdir/redis.logrotate" "$pkgdir/etc/logrotate.d/redis"
  install -Dm644 "$srcdir/${pkgname}-${pkgver}/redis.conf" "$pkgdir/etc/redis.conf"
  install -Dm644 "$srcdir/redis.tmpfiles.d" "$pkgdir/usr/lib/tmpfiles.d/redis.conf"
}
