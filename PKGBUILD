# Maintainer: laloch <laloch@atlas.cz>
pkgname=kvpm-svn
_pkgname=kvpm
pkgver=1196
pkgrel=1
pkgdesc='KDE4 front end for Linux LVM and Gnu parted'
url='http://kvpm.sourceforge.net'
arch=('i686' 'x86_64')
license=('GPL3')
depends=('kdebase-runtime' 'lvm2')
makedepends=('cmake' 'automoc4' 'subversion')
conflicts=('kvpm')
provides=('kvpm')
source=()
md5sums=()
install=${pkgname}.install

_svntrunk='svn://svn.code.sf.net/p/kvpm/code/trunk'
_svnmod='kvpm-code'

build() {
  cd ${srcdir}

  if [ -d ${_svnmod} ]; then
    svn up -r ${pkgver}
  else
    svn co ${_svntrunk} -r ${pkgver} ${_svnmod}
  fi
  msg2 'SVN checkout done or server timeout'

  rm -rf ${srcdir}/${_svnmod}-build
  mkdir ${srcdir}/${_svnmod}-build
  cd ${srcdir}/${_svnmod}-build
  
  msg2 'Starting make...'
  cmake ${srcdir}/${_svnmod} \
    -DKDE4_ENABLE_HTMLHANDBOOK=OFF \
    -DCMAKE_INSTALL_PREFIX=`kde4-config --prefix` \
    -DQT_QMAKE_EXECUTABLE=/usr/bin/qmake-qt4 \
    -DCMAKE_BUILD_TYPE=Release
  make
  echo "[Desktop Entry]
GenericName=LVM Frontend
Name=KVPM
Comment=KDE Volume Partition Manager
Exec=kdesu kvpm
Icon=kvpm
X-KDE-SubstituteUID=true
Type=Application
Categories=System;KDE;" > ${srcdir}/${_svnmod}-build/${_pkgname}.desktop
}

package() {
  cd ${srcdir}/${_svnmod}-build
  make DESTDIR=${pkgdir} install
  install -Dm 644 ${_pkgname}.desktop ${pkgdir}/usr/share/applications/${_pkgname}.desktop  
  mkdir -p ${pkgdir}/usr/bin
  mv ${pkgdir}/usr/sbin/* ${pkgdir}/usr/bin
  rmdir ${pkgdir}/usr/sbin
}
