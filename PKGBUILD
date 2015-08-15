# $Id: PKGBUILD 117239 2014-08-12 03:15:49Z fyan $
# Maintainer:  Bartłomiej Piotrowski <bpiotrowski@archlinux.org>
# Contributor: Lukas Fleischer <archlinux at cryptocrack dot de>
# Contributor: Angel Velasquez <angvp@archlinux.org>
# Contributor: judd <jvinet@zeroflux.org>

_pkgname=exim
pkgname=${_pkgname}_spf
pkgver=4.84
pkgrel=1
pkgdesc='Message Transfer Agent'
arch=('x86_64' 'i686')
url='http://www.exim.org/'
license=('GPL')
backup=(etc/mail/aliases etc/mail/exim.conf etc/logrotate.d/exim)
install=exim.install
depends=('gdbm' 'pcre' 'pam' 'openssl' 'libldap' 'libspf2')
provides=('smtp-server' 'smtp-forwarder')
conflicts=('smtp-server' 'smtp-forwarder')
options=('!makeflags')
source=(http://mirror.switch.ch/ftp/mirror/exim/exim/exim4/exim-$pkgver.tar.bz2
        aliases
        exim.logrotate
        exim.Makefile
        exim-submission@.service
        exim.service
        exim@.service
        exim.socket
        exim-submission.socket)
md5sums=('3d14522e604b687b9e515f5aa739b2c0'
         '4874006f0585253ddab027d441009757'
         'e18a535218718c5eb394ed5c9296fe06'
         'c24d009ae8587aed96a2e8e57596546c'
         '88cf76405367a6ed371d05db1d3c9565'
         'be8fdcf7b2d6b6368a21705e69743265'
         '6f5a5c2b0835b33f59d2513cce18c65b'
         '101a726e4d3f4d0074710273f9808022'
         '11b319ba0bd84911fa70e9bef2190715')

build() {
  cd $_pkgname-$pkgver

  cp ../$_pkgname.Makefile Local/Makefile
  make
}

package() {
  cd $_pkgname-$pkgver

  install -Dm0644 ../exim.logrotate "$pkgdir"/etc/logrotate.d/exim
  install -Dm0644 doc/exim.8 "$pkgdir"/usr/share/man/man8/exim.8

  mkdir -p "$pkgdir"/var/spool/exim/db "$pkgdir"/etc/mail \
    "$pkgdir"/var/log/exim "$pkgdir"/usr/{lib,bin}

  chmod 770 "$pkgdir"/var/spool/exim{,/db} "$pkgdir"/var/log/exim

  cd build-Linux-*
  for i in exicyclog exim_checkaccess exim_dumpdb exim_lock exim_tidydb \
    exipick exiqsumm exigrep exim_dbmbuild exim exim_fixdb eximstats exinext \
    exiqgrep exiwhat; do
    install -m0755 "$i" "$pkgdir"/usr/bin
  done

  cd ../src
  sed -e "s|/etc/aliases|/etc/mail/aliases|g" \
    -e "s|SYSTEM_ALIASES_FILE|/etc/mail/aliases|g" \
    configure.default > "$pkgdir"/etc/mail/exim.conf

  cp "$srcdir"/aliases "$pkgdir"/etc/mail

  cd "$pkgdir"/usr/bin
  for i in mailq newaliases rmail rsmtp runq sendmail; do
    ln -s exim "$i"
  done

  # fhs compliancy
  ln -s ../bin/exim ../lib/sendmail

  install -Dm0644 "$srcdir"/exim-submission@.service \
    "$pkgdir"/usr/lib/systemd/system/exim-submission@.service
  install -Dm0644 "$srcdir"/exim.service \
    "$pkgdir"/usr/lib/systemd/system/exim.service
  install -Dm0644 "$srcdir"/exim@.service \
    "$pkgdir"/usr/lib/systemd/system/exim@.service
  install -Dm0644 "$srcdir"/exim.socket \
    "$pkgdir"/usr/lib/systemd/system/exim.socket
  install -Dm0644 "$srcdir"/exim-submission.socket \
    "$pkgdir"/usr/lib/systemd/system/exim-submission.socket
}
