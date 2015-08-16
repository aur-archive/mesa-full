# Maintainer: LEW21 <lew21@xtreeme.org>

pkgname=mesa-full
pkgver=9.2.0_devel.57103
_realver=9.2
pkgrel=1
pkgdesc="Full Mesa 3D graphics library with all its components, built from the git master branch. Built with Wayland support."
arch=('i686' 'x86_64')
url="http://mesa3d.org/"
license=('LGPL')
depends=('libdrm' 'libvdpau' 'wayland' 'libxxf86vm' 'libxdamage' 'systemd' 'llvm>=3.3' 'elfutils')
makedepends=('git' 'python2' 'libxml2' 'libx11' 'glproto' 'dri2proto' 'libxvmc')
optdepends=('libtxc_dxtn: S3TC support'
'mesa-demos: glxinfo and glxgears'
'opengl-man-pages: for the OpenGL API man pages')
replaces=('mesa-full-wayland')
provides=('mesa-full-wayland' "mesa=${_realver}" "mesa-libgl=${_realver}" "libgl" 'libglapi' 'osmesa' 'libgbm' 'libgles' 'libegl' 'khrplatform-devel'
"ati-dri=${_realver}" "intel-dri=${_realver}" "nouveau-dri=${_realver}" "svga-dri=${_realver}")
conflicts=('mesa' 'mesa-libgl' 'libgl' 'libglapi' 'osmesa' 'libgbm' 'libgles' 'libegl' 'khrplatform-devel' 'ati-dri' 'intel-dri' 'nouveau-dri' 'svga-dri')

source=(git://anongit.freedesktop.org/git/mesa/mesa)
sha1sums=('SKIP')

pkgver() {
	cd mesa
	echo $(grep --max-count=1 -F "AC_INIT([Mesa], [" configure.ac | cut -f2 -d " " | cut -f2 -d "[" | cut -f1 -d "]" | tr "-" "_").$(git rev-list --count HEAD)
}

build() {
	cd mesa

	export PYTHON=/usr/bin/python2

	./autogen.sh --prefix=/usr \
		--sysconfdir=/etc \
		--with-dri-driverdir=/usr/lib/xorg/modules/dri \
		--with-gallium-drivers=r300,r600,radeonsi,nouveau,svga,swrast \
		--with-dri-drivers=i915,i965,radeon,r200,nouveau \
		--disable-radeon-llvm \
		--enable-gallium-llvm \
		--enable-egl \
		--enable-gallium-egl \
		--with-egl-platforms=drm,x11,wayland \
		--enable-shared-glapi \
		--enable-gbm \
		--enable-glx-tls \
		--enable-osmesa \
		--enable-gles1 \
		--enable-gles2 \
		--enable-texture-float \
		--enable-xa \
		--enable-vdpau \

	make
}

package() {
	cd mesa

	make DESTDIR="${pkgdir}" install

	# See FS#26284
	install -m755 -d "${pkgdir}/usr/lib/xorg/modules/extensions"
	ln -s libglx.xorg "${pkgdir}/usr/lib/xorg/modules/extensions/libglx.so"
}
