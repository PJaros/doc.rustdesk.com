---
title: Linux
weight: 10
---

### Abhängigkeiten

Desktop-Versionen nutzen [Sciter](https://sciter.com/) für die Benutzeroberfläche, bitte laden Sie die dynamische Bibliothek Sciter selbst herunter.

[Windows](https://raw.githubusercontent.com/c-smile/sciter-sdk/master/bin.win/x64/sciter.dll) |
[Linux](https://raw.githubusercontent.com/c-smile/sciter-sdk/master/bin.lnx/x64/libsciter-gtk.so) |
[macOS](https://raw.githubusercontent.com/c-smile/sciter-sdk/master/bin.osx/libsciter.dylib)

### Grobe Schritte zum Erstellen

- Bereiten Sie Ihre Rust-Entwicklungsumgebung und Ihre C++-Build-Umgebung vor

- Installieren Sie [vcpkg](https://github.com/microsoft/vcpkg) und setzen Sie die Umgebungsvariable `VCPKG_ROOT` korrekt

  - Windows: `vcpkg install libvpx:x64-windows-static libyuv:x64-windows-static opus:x64-windows-static aom:x64-windows-static`
  - Linux/macOS: `vcpkg install libvpx libyuv opus aom`

- Nutzen Sie `cargo run`

### Auf Linux erstellen

#### Ubuntu 18.04 (Debian 10)

```sh
sudo apt install -y g++ gcc git curl wget nasm yasm libgtk-3-dev clang libxcb-randr0-dev libxdo-dev libxfixes-dev libxcb-shape0-dev libxcb-xfixes0-dev libasound2-dev libpulse-dev cmake  libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev
```

#### Fedora 28 (CentOS 8)

```sh
sudo yum -y install gcc-c++ git curl wget nasm yasm gcc gtk3-devel clang libxcb-devel libxdo-devel libXfixes-devel pulseaudio-libs-devel cmake alsa-lib-devel
```

#### Arch Linux (Manjaro)

```sh
sudo pacman -Syu --needed unzip git cmake gcc curl wget yasm nasm zip make pkg-config clang gtk3 xdotool libxcb libxfixes alsa-lib pulseaudio
```

#### vcpkg installieren

```sh
git clone https://github.com/microsoft/vcpkg
cd vcpkg
git checkout 2023.04.15
cd ..
vcpkg/bootstrap-vcpkg.sh
export VCPKG_ROOT=$HOME/vcpkg
vcpkg/vcpkg install libvpx libyuv opus aom
```

#### libvpx reparieren (für Fedora)

```sh
cd vcpkg/buildtrees/libvpx/src
cd *
./configure
sed -i 's/CFLAGS+=-I/CFLAGS+=-fPIC -I/g' Makefile
sed -i 's/CXXFLAGS+=-I/CXXFLAGS+=-fPIC -I/g' Makefile
make
cp libvpx.a $HOME/vcpkg/installed/x64-linux/lib/
cd
```

#### Erstellen

```sh
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source $HOME/.cargo/env
git clone https://github.com/rustdesk/rustdesk
cd rustdesk
mkdir -p target/debug
wget https://raw.githubusercontent.com/c-smile/sciter-sdk/master/bin.lnx/x64/libsciter-gtk.so
mv libsciter-gtk.so target/debug
VCPKG_ROOT=$HOME/vcpkg cargo run
```

#### Wayland zu X11 ändern (Xorg)

~~RustDesk unterstützt Wayland nicht.~~ Lesen Sie diese [Anleitung](https://docs.fedoraproject.org/en-US/quick-docs/configuring-xorg-as-default-gnome-session/), um Xorg als Standard-GNOME-Sitzung zu konfigurieren.

RustDesk unterstützt jetzt experimentell Wayland. Um dieses Feature zu aktivieren, müssen Sie möglicherweise die [Nightly-Version](https://github.com/rustdesk/rustdesk/releases/tag/nightly) herunterladen.
