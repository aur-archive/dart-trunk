# Maintainer: Kenneth Endfinger <kaendfinger@gmail.com>

pkgname=dart-trunk
pkgver=41921
pkgrel=1
pkgdesc='Bleeding Edge (Trunk Build) of the Dart SDK'
arch=('x86_64' 'i686')
url='http://www.dartlang.org/'
license=('BSD')
makedepends=('setconf')
options=('!strip')
provides=('dart')
conflicts=('dart')

if [[ $CARCH == x86_64 ]]; then
  source=("$pkgname-x64.zip::http://storage.googleapis.com/dart-archive/channels/be/raw/latest/sdk/dartsdk-linux-x64-release.zip")
  sha256sums=('SKIP')
else
  source=("$pkgname-x32.zip::http://storage.googleapis.com/dart-archive/channels/be/raw/latest/sdk/dartsdk-linux-ia32-release.zip")
  sha256sums=('SKIP')
fi

prepare() {
  # Fix permissions
  find "dart-sdk" -type d -exec chmod 0755 '{}' + \
    -or -type f -exec chmod 0644 '{}' +
  chmod +x "dart-sdk/bin/"*

  cd "dart-sdk/bin"

  # Configure paths
  setconf dart2js BIN_DIR "/opt/dart-sdk/bin"
  setconf dart2js PROG_NAME "/opt/dart-sdk/bin/dart2js"
  setconf dartanalyzer SCRIPT_DIR "/opt/dart-sdk/bin"
  setconf docgen BIN_DIR "/opt/dart-sdk/bin"
  setconf pub BIN_DIR "/opt/dart-sdk/bin"
  setconf pub SDK_DIR "/opt/dart-sdk/"
  setconf dartfmt BIN_DIR "/opt/dart-sdk/bin"
  setconf dartfmt SDK_DIR "/opt/dart-sdk/"

  # Extract license (AUTHORS and LICENSE files are missing)
  head -n5 "../include/dart_api.h" > ../../LICENSE
}

pkgver() {
  cat "dart-sdk/revision"
}

package() {
  # Create directories
  install -d "$pkgdir"{"/opt/dart-sdk",/usr/{bin,"share/doc/dart-sdk"}}

  # Package the files
  cp -a "dart-sdk/"* "$pkgdir/opt/dart-sdk/"

  # Set up symbolic links for the executables
  for f in dart dart2js dartanalyzer docgen pub dartfmt; do
    ln -s "/opt/dart-sdk/bin/$f" "$pkgdir/usr/bin/$f"
  done

  # BSD License
  install -Dm644 LICENSE \
    "$pkgdir/usr/share/licenses/dart-sdk/LICENSE"
}
