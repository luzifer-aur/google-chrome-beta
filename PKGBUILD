# Maintainer: Det <nimetonmaili g-mail>
# Contributors: t3ddy, Lex Rivera aka x-demon, ruario

# Check for new Linux releases in: http://googlechromereleases.blogspot.com/search/label/Beta%20updates
# or use: $ curl -s https://dl.google.com/linux/chrome/rpm/stable/x86_64/repodata/other.xml.gz | gzip -df | awk -F\" '/pkgid/{ sub(".*-","",$4); print $4": "$10 }'

pkgname=google-chrome-beta
pkgver=55.0.2883.35
pkgrel=1
pkgdesc="An attempt at creating a safer, faster, and more stable browser (Beta Channel)"
arch=('x86_64')
url="https://www.google.com/chrome"
license=('custom:chrome')
depends=('alsa-lib' 'desktop-file-utils' 'flac' 'gconf' 'gnome-themes-standard' 'gtk2' 'harfbuzz' 'harfbuzz-icu'
         'hicolor-icon-theme' 'icu' 'libpng' 'libxss' 'libxtst' 'nss' 'opus' 'snappy' 'speech-dispatcher'
         'ttf-font' 'xdg-utils')
optdepends=('kdebase-kdialog: needed for file dialogs in KDE'
            'ttf-liberation: fix fonts for some PDFs')
provides=('google-chrome')
options=('!emptydirs' '!strip')
install=$pkgname.install
_channel=beta
source=("google-chrome-${_channel}_${pkgver}_amd64.deb::https://dl.google.com/linux/direct/google-chrome-${_channel}_current_amd64.deb"
        "$url/browser/privacy/eula_text.html"
        'google-chrome-beta.sh'
        'StartupWMClass-and-GNOME3-context-menu.patch')
md5sums=('977f745835f963c1922c7f20760f139f'
         'SKIP'
         'ca16d5162eed85b1ba4e6b9fc37f9e35'
         '1cc8a4f5310d51234973ca5bbb8d75ae')

package() {
  msg2 "Extracting the data.tar.xz..."
  bsdtar -xf data.tar.xz -C "$pkgdir/"

  msg2 "Moving stuff in place..."
  # Launcher
  install -m755 google-chrome-$_channel.sh "$pkgdir"/usr/bin/google-chrome-$_channel

  # Icons
  for i in 16x16 22x22 24x24 32x32 48x48 64x64 128x128 256x256; do
    install -Dm644 "$pkgdir"/opt/google/chrome-$_channel/product_logo_${i/x*}.png \
                   "$pkgdir"/usr/share/icons/hicolor/$i/apps/google-chrome-$_channel.png
  done

  # Man page
  gzip "$pkgdir"/usr/share/man/man1/google-chrome-$_channel.1

  # License
  install -Dm644 eula_text.html "$pkgdir"/usr/share/licenses/google-chrome-$_channel/eula_text.html

  #msg2 "Fixing Chrome icon resolution..."
  #sed -i "/Exec=/i\StartupWMClass=Google-chrome-$_channel" "$pkgdir"/usr/share/applications/google-chrome-$_channel.desktop

  msg2 "Fixing Chrome icon resolution and GNOME 3 context menu..."
  cd "$pkgdir"
  patch -p0 -i "$srcdir"/StartupWMClass-and-GNOME3-context-menu.patch

  msg2 "Removing Debian Cron job and duplicate product logos..."
  rm -r "$pkgdir"/etc/cron.daily/ "$pkgdir"/opt/google/chrome-$_channel/cron/
  rm "$pkgdir"/opt/google/chrome-$_channel/product_logo_*.png
}
 