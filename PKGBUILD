pkgname=zapret2
pkgver=0.9.4.5
pkgrel=1.1
pkgdesc="Anti-DPI software"
arch=('x86_64')
url="https://github.com/bol-van/$pkgname"
license=('MIT')
depends=(
  'luajit'
  'libnetfilter_queue'
)
makedepends=('git')
provides=($pkgname)
source=($pkgname::git+$url.git#tag=v$pkgver)
sha256sums=('5b4b64ac29b27fc147e5980362926799b7f5a9ed476c0ca0faa16ec8c18e5c52')

prepare() {
  sed -i '/^\[Service\]/ s|$|\nEnvironmentFile=-/tmp/zapret2/envvars|' "$srcdir"/$pkgname/init.d/systemd/zapret2.service
}

build() {
  cd $pkgname
  make
}

package() {
  cd $pkgname

  install -Dm644 /dev/stdin "$pkgdir"/usr/lib/sysusers.d/$pkgname.conf << END
u $pkgname - "$pkgname ${pkgdesc,}"
END

  install -Dm755 ipset/*  -t "$pkgdir"/opt/$pkgname/ipset/
  install -Dm644 common/* -t "$pkgdir"/opt/$pkgname/common/
  install -Dm644 lua/*    -t "$pkgdir"/opt/$pkgname/lua/
  mkdir -p -m 664 "$pkgdir"/opt/$pkgname/files/fake

  install -Dm755 init.d/sysv/{functions,$pkgname} -t "$pkgdir"/opt/$pkgname/init.d/sysv/
  install -Dm644 init.d/systemd/zapret2.service -t "$pkgdir"/usr/lib/systemd/system/

  install -Dm664 config.default -g zapret2 -T "$pkgdir"/opt/$pkgname/config

  install -Dm755 binaries/my/nfqws2 -T "$pkgdir"/opt/$pkgname/nfq2/nfqws2
}
