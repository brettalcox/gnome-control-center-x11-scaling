# Maintainer: puxplaying (Georg Wagner) <puxplaying@gmail.com>

# Archlinux credits:
# Maintainer: Jan Alexander Steffens (heftig) <heftig@archlinux.org>
# Contributor: Jan de Groot <jgc@archlinux.org>

# Ubuntu credits:
# Sebastien Bacher: <https://salsa.debian.org/gnome-team/gnome-control-center>
# Marco Trevisan: <https://salsa.debian.org/gnome-team/mutter/-/blob/ubuntu/master/debian/patches/x11-Add-support-for-fractional-scaling-using-Randr.patch>

pkgbase=gnome-control-center
pkgname=$pkgbase-x11-scaling
pkgver=40.0
pkgrel=1
pkgdesc="GNOME's main interface to configure various aspects of the desktop"
url="https://gitlab.gnome.org/GNOME/gnome-control-center"
license=(GPL2)
arch=(x86_64)
depends=(accountsservice cups-pk-helper gnome-bluetooth gnome-desktop
         gnome-online-accounts gnome-settings-daemon gsettings-desktop-schemas gtk3
         libgtop nm-connection-editor sound-theme-freedesktop upower libpwquality
         gnome-color-manager smbclient libmm-glib libgnomekbd grilo libibus
         cheese libgudev bolt udisks2 libhandy gsound colord-gtk)
makedepends=(docbook-xsl modemmanager git python meson)
checkdepends=(python-dbusmock python-gobject xorg-server-xvfb)
conflicts=($pkgbase)
provides=($pkgbase)
optdepends=('system-config-printer: Printer settings'
            'gnome-user-share: WebDAV file sharing'
            'gnome-remote-desktop: screen sharing'
            'rygel: media sharing'
            'openssh: remote login')
groups=(gnome)
_commit=49d71c07b5b3ce59e035b785310cba4fcf903868  # tags/40.0^0
source=("git+https://gitlab.gnome.org/GNOME/gnome-control-center.git#commit=$_commit"
        "git+https://gitlab.gnome.org/GNOME/libgnome-volume-control.git"
        "0019-display-Support-UI-scaled-logical-monitor-mode.patch::https://salsa.debian.org/gnome-team/gnome-control-center/-/raw/ubuntu/master/debian/patches/0019-display-Support-UI-scaled-logical-monitor-mode.patch"
        "0024-display-Allow-fractional-scaling-to-be-enabled.patch::https://salsa.debian.org/gnome-team/gnome-control-center/-/raw/ubuntu/master/debian/patches/0024-display-Allow-fractional-scaling-to-be-enabled.patch")
sha256sums=('SKIP'
            'SKIP'
            '29be1d77bfc84b8cdea72df222b0ad1ce65a1452cdab7bec240cda7769ca05f5'
            '5eeba3e2a6b88570dd106bf6b6a9d450b1a6c5484e525deca34990a0ca3bd3c5')

pkgver() {
  cd $pkgbase
  git describe --tags | sed 's/^GNOME_CONTROL_CENTER_//;s/_/./g;s/-/+/g'
}

prepare() {
  cd $pkgbase
  git submodule init
  git submodule set-url subprojects/gvc "$srcdir/libgnome-volume-control"
  git submodule update
  
  # Ubuntu Patches for X11 fractional scaling
  patch -p1 -i "${srcdir}/0019-display-Support-UI-scaled-logical-monitor-mode.patch"
  patch -p1 -i "${srcdir}/0024-display-Allow-fractional-scaling-to-be-enabled.patch"
}


build() {
  arch-meson $pkgbase build -D documentation=true
  meson compile -C build
}

check() {
  meson test -C build --print-errorlogs
}

package() {
  DESTDIR="$pkgdir" meson install -C build
  install -d -o root -g 102 -m 750 "$pkgdir/usr/share/polkit-1/rules.d"
}
