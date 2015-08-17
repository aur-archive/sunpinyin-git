# Maintainer: Felix Yan <felixonmars@gmail.com>
# Contributor: Jekyll Wu <adaptee [at] gmail [dot] com>
# Contributor: Liu Chang <goduck777@gmail.com>
 
pkgname=sunpinyin-git
pkgver=20121025
pkgrel=1
pkgdesc='A statistical language model based pinyin IME by Sun.'
url='http://code.google.com/p/sunpinyin/'
arch=('i686' 'x86_64')
license=('LGPL' 'APACHE')
depends=('sqlite3')
makedepends=('git' 'scons' 'intltool')
provides=('sunpinyin')
conflicts=('sunpinyin')
source=('http://open-gram.googlecode.com/files/lm_sc.t3g.arpa-20121025.tar.bz2'
	'http://open-gram.googlecode.com/files/dict.utf8-20120830.tar.bz2')

_gitroot="git://github.com/sunpinyin/sunpinyin.git"
_gitname="sunpinyin"

build() {
  cd $srcdir
  msg "Connecting to the GIT server...."
  
  if [[ -d $srcdir/$_gitname ]] ; then
    cd $_gitname
    git pull origin
    msg "The local files are updated."
  else
    git clone $_gitroot
  fi
  
  msg "GIT checkout done"
  msg "Starting make..."

  rm -rf $srcdir/$_gitname-build
  git clone $srcdir/$_gitname $srcdir/$_gitname-build
  cd $srcdir/$_gitname-build
  
  mkdir -p raw
  ln -sf ${srcdir}/lm_sc.t3g.arpa raw/lm_sc.t3g.arpa
  ln -sf ${srcdir}/dict.utf8 raw/dict.utf8

  sed -i -e "1s|python|python2|" python/*.py python/importer/*.py
  scons --prefix=/usr
  
  ln -sf ${srcdir}/$_gitname-build/doc/SLM-inst.mk $srcdir/$_gitname-build/raw/Makefile
  cd $srcdir/$_gitname-build/raw
  PATH=$srcdir/$_gitname-build/src:$PATH
  make
}
package ()
{
  cd $srcdir/$_gitname-build
  scons install --prefix=/usr --install-sandbox=$pkgdir
  
  cd $srcdir/$_gitname-build/raw
  make DESTDIR=$pkgdir install
}

md5sums=('0586241ca33359ad176c842c90bf563e'
         '52b9a47861bef707f65b682d52e8117f')
