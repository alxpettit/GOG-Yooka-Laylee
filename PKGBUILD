# Maintainer: Alexandria Pettit <alexandria@achu.xyz>
# Contributer: Dan Beste <dan.ray.beste@gmail.com>

pkgname='gog-yooka-laylee'
pkgver=2.x
pkgrel=2
pkgdesc="Yooka-Laylee is an all-new open-world platformer from key creative talent behind the Banjo-Kazooie and Donkey Kong Country games!"
url="http://www.playtonicgames.com/games/yooka-laylee/"
license=('custom')
arch=('i686' 'x86_64')
makedepends=('p7zip')
optdepends=(
  'firejail: Automatically sandbox this application from your OS'
)
source=(
  "${pkgname}"
  "${pkgname}.desktop"
  "${pkgname}.profile"
)
sha256sums=('4e4c5428a1d929007fea2204b688a4dcd97a13c992b5f92a0c7866f11adc8adc'
            '55509b743de096883a55992e2361219f7e0a3305833fe1b281c2fa08f70d0b80'
            '45d542985620e05d6e60f52e3e9b348870f79b328cc118ff9bd5769e9fed5585')

prepare() {
  success=false
  for file_path in "$(pwd)" "${HOME}/Downloads" "${HOME}/Desktop" "${HOME}"; do
    # Iterate over common-sense directories looking for yooka laylee installer...
    echo "Searching in ${file_path}..."
    target_path=(${file_path}/yooka_laylee_*_*.sh)
    test -f "${target_path}" && {
      echo "File found: ${target_path}"
      7z x "${target_path}" -tzip -y && {
        success=true
        break
      }
    } || echo "Couldn't find under ${target_path}"
  done
  test "$success" = true && {
    cd "data/noarch/game"
    for lib_path in "${srcdir}/data/noarch/game/lib"{32,64}; do
      cd "${lib_path}"
      # Fix for someone at GOG messing up the symlinks
      rm libSDL2.so libSDL2-2.0.so.0
      ln -s libSDL2-2.0.so.0.4.1 libSDL2-2.0.so.0
      ln -s libSDL2-2.0.so.0.4.1 libSDL2.so
    done

  } || {
    echo "Failure!"
    echo "Please ensure that this file is in one of the directories listed above!"
    exit 1
  }

}

package() {
  install -d "${pkgdir}/opt/${pkgname}/"
  install -d "${pkgdir}/opt/${pkgname}/support/"
  install -d "${pkgdir}/usr/bin/"
  install -d "${pkgdir}/usr/share/applications/"
  install -d "${pkgdir}/usr/share/licenses/${pkgname}/"
  install -d "${pkgdir}/usr/share/pixmaps/"

  cp -r data/noarch/game "${pkgdir}/opt/${pkgname}/"
  find "${pkgdir}/opt/${pkgname}" -type d -exec chmod 755 {} \;

  install -m 755           \
    "${srcdir}/${pkgname}" \
    "${pkgdir}/usr/bin/${pkgname}"
  install -m 755         \
    data/noarch/start.sh \
    "${pkgdir}/opt/${pkgname}/"
  install -m 755                     \
    data/noarch/support/*.{sh,shlib} \
    "${pkgdir}/opt/${pkgname}/support/"
  install -m 644                                      \
    'data/noarch/docs/End User License Agreement.txt' \
    "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
  install -m 644                   \
    "data/noarch/support/icon.png" \
    "${pkgdir}/usr/share/pixmaps/${pkgname}.png"
  install -m 644                   \
    "${srcdir}/${pkgname}.desktop" \
    "${pkgdir}/usr/share/applications/${pkgname}.desktop"
}

# vim: ts=2 sw=2 et:
