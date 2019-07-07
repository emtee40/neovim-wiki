You can install Neovim from [download](#install-from-download), [package](#install-from-package), or [source](#install-from-source) in just a few seconds.

---

- To start Neovim, run `nvim` (not `neovim`).
- Before upgrading to a new version, **check [Following HEAD](Following-HEAD).**
- For config (vimrc) see [the FAQ](https://github.com/neovim/neovim/wiki/FAQ#where-should-i-put-my-config-vimrc).

---

Install from download
=====================

Downloads are available on the [Releases](https://github.com/neovim/neovim/releases) page.

* Latest [stable release](https://github.com/neovim/neovim/releases/latest)
* Latest [development prerelease](https://github.com/neovim/neovim/releases/nightly)


Install from package
====================

Packages are listed below. (You can also [build Neovim from source](#install-from-source).)

## Windows

### [Chocolatey](https://chocolatey.org)

- **Release (v0.3):** `choco install neovim` (use -y for automatically skipping confirmation messages)
- **Development (pre-release):** `choco install neovim --pre`

### [Scoop](http://scoop.sh/)

- **Release (v0.3.4):**`scoop install neovim`
- **Development (pre-release):**

```
scoop bucket add neovim-dev https://github.com/wsdjeg/scoop-neovim-dev.git
scoop install neovim-dev
```

### Pre-built archives

0. If you are missing `VCRUNTIME140.dll`, install the [Visual Studio 2015 C++ redistributable](https://support.microsoft.com/en-us/kb/2977003) (choose x86_64 or x86 depending on your system).
1. Choose a package (**nvim-winXX.zip**) from the [releases page](https://github.com/neovim/neovim/releases).
2. Unzip the package. Any location is fine, administrator privileges are _not_ required.
    - `$VIMRUNTIME` will be set to that location automatically.
3. Double-click `nvim-qt.exe`.

**Optional** steps:

- Add the `bin` folder (e.g. `C:\Program Files\nvim\bin`) to your PATH.
    - This makes it easy to run `nvim` and `nvim-qt` from anywhere.
- If `:set spell` does not work, create the `C:/Users/foo/AppData/Local/nvim/site/spell` folder. 
  You can then copy your spell files over (for English, located 
  [here](https://github.com/vim/vim/blob/master/runtime/spell/en.utf-8.spl) and 
  [here](https://github.com/vim/vim/blob/master/runtime/spell/en.utf-8.sug));
- For Python plugins you need the `pynvim` module. "Virtual envs" are recommended. After activating the virtual env do `pip install pynvim` (in *both*). Edit your `init.vim` so that it contains the path to the env's Python executable:
    ```vim
    let g:python3_host_prog='C:/Users/foo/Envs/neovim3/Scripts/python.exe'
    let g:python_host_prog='C:/Users/foo/Envs/neovim/Scripts/python.exe'
    ```
    - Run `:checkhealth` and read `:help provider-python`.
- **init.vim ("vimrc"):** If you already have Vim installed you can copy `%userprofile%\_vimrc` to `%userprofile%\AppData\Local\nvim\init.vim` to use your Vim config with Neovim.


## macOS / OS X

### Pre-built archives

The [Releases](https://github.com/neovim/neovim/releases) page provides pre-built binaries for macOS 10.11+.

    curl -LO https://github.com/neovim/neovim/releases/download/nightly/nvim-macos.tar.gz
    tar xzf nvim-macos.tar.gz
    ./nvim-osx64/bin/nvim

### [Homebrew](http://brew.sh) (macOS) / [Linuxbrew](http://linuxbrew.sh/) (Linux)

    brew install neovim

Or install the development version of Nvim:

    brew install --HEAD neovim

### [Macports](https://www.macports.org/)

    sudo port selfupdate
    sudo port install neovim

## Linux

### AppImage ("universal" Linux package)

The [Releases](https://github.com/neovim/neovim/releases) page provides an [AppImage](http://appimage.org) that runs on most Linux systems. No installation is needed, just download `nvim.appimage` and run it. (It might not work if your Linux distribution is more than 4 years old.)

    curl -LO https://github.com/neovim/neovim/releases/download/nightly/nvim.appimage
    chmod u+x nvim.appimage
    ./nvim.appimage

### Arch Linux

Neovim can be installed from the community repository:

    sudo pacman -S neovim

Alternatively, Neovim can be also installed using the PKGBUILD [`neovim-git`](https://aur.archlinux.org/packages/neovim-git), available on the [AUR](https://wiki.archlinux.org/index.php/Arch_User_Repository).

Alternatively, Neovim Nightly builds can be also installed using the PKGBUILD [`neovim-nightly`](https://aur.archlinux.org/packages/neovim-nightly), available on the [AUR](https://wiki.archlinux.org/index.php/Arch_User_Repository).

The Python module is available from the community repository:

    sudo pacman -S python-neovim

Python 2 and Ruby modules (currently only supported in `neovim-git`) are available from the AUR as [`python2-neovim`](https://aur.archlinux.org/packages/python2-neovim-git) and [`ruby-neovim`](https://aur.archlinux.org/packages/ruby-neovim) respectively.

### CentOS 7 / RHEL 7

Neovim is available through [EPEL (Extra Packages for Enterprise Linux)](https://fedoraproject.org/wiki/EPEL)

    yum install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
    yum install -y neovim python{2,3}-neovim

### CRUX

A CRUX port is available under [`6c37/neovim`](https://github.com/6c37/crux-ports), along with ports for other dependencies of Neovim.

For instructions on how to install the Python modules, see [`:help provider-python`].


### Debian

Neovim is in [Debian](https://packages.debian.org/search?keywords=neovim).

    sudo apt-get install neovim

Python (`:python`) support is installable via the package manager on Debian unstable.

    sudo apt-get install python-neovim
    sudo apt-get install python3-neovim

### Exherbo Linux

Exhereses for scm and released versions are currently available in repository `::medvid`. Python client (with GTK+ GUI included) and Qt5 GUI are also available as suggestions:

    cave resolve app-editors/neovim --take dev-python/neovim-python --take app-editors/neovim-qt

### Fedora

Neovim is in [Fedora](https://admin.fedoraproject.org/pkgdb/package/rpms/neovim/) starting with Fedora 25:

    dnf install -y neovim python{2,3}-neovim

You can also get nightly builds of git master from the [Copr automated build system](https://copr.fedoraproject.org/coprs/agriffis/neovim-nightly/):

    dnf copr enable agriffis/neovim-nightly
    dnf install -y neovim python{2,3}-neovim

See the [blog post](https://arongriffis.com/2019/03/02/neovim-nightly-builds) for information on how these are built.

### Flatpak

You can find Neovim on [Flathub](https://flathub.org/apps/details/io.neovim.nvim). Providing you have Flatpak [set up](https://flatpak.org/setup/):

    flatpak install flathub io.neovim.nvim
    flatpak run io.neovim.nvim

You can add `/var/lib/flatpak/exports/bin` (or `~/.local/share/flatpak/exports/bin` if you used `--user`) to the `$PATH` and run it with `io.neovim.nvim`.

Note that Flatpak'ed Neovim will look for `init.vim` in `~/.var/app/io.neovim.nvim/config/nvim` instead of `~/.config/nvim`. 

### Gentoo Linux

An ebuild is available in Gentoo's official portage repository:

    emerge -a app-editors/neovim

### GNU Guix

Neovim can be installed with:

    guix install neovim

### Nix / NixOS

Neovim can be installed with:

    nix-env -iA nixpkgs.neovim

To install the Python modules:

    nix-env -iA nixpkgs.python3Packages.pynvim

Replace python3 with python2 for the python 2 packages.

### Mageia 7

    urpmi neovim

To install the Python modules:

    urpmi python2-pynvim python3-pynvim

### OpenSUSE

Neovim can be installed with:

    sudo zypper in neovim

To install the Python modules:
    
    sudo zypper in python-neovim python3-neovim

### PLD Linux

Neovim is in [PLD Linux](https://github.com/pld-linux/neovim):

    poldek -u neovim
    poldek -u python-neovim python3-neovim
    poldek -u python-neovim-gui python3-neovim-gui

### Slackware

See [neovim on SlackBuilds](http://slackbuilds.org/apps/neovim/).

For instructions on how to install the Python modules, see [`:help provider-python`].


### Source Mage

Neovim can be installed using the Sorcery package manager:

    cast neovim

### Solus

Neovim can be installed using the default package manager in Solus (eopkg):

    sudo eopkg install neovim

### Ubuntu

Neovim has been added to a "Personal Package Archive" (PPA). This allows you to install it with `apt-get`. Follow the links to the PPAs to see which versions of Ubuntu are currently available via the PPA. Choose **stable** or **unstable**:

- [https://launchpad.net/~neovim-ppa/+archive/ubuntu/**stable**](https://launchpad.net/~neovim-ppa/+archive/ubuntu/stable) (Xenial 16.04 or newer only).
- [https://launchpad.net/~neovim-ppa/+archive/ubuntu/**unstable**](https://launchpad.net/~neovim-ppa/+archive/ubuntu/unstable)

To be able to use **add-apt-repository** you may need to install software-properties-common:

    sudo apt-get install software-properties-common

If you're using an older version Ubuntu you must use:

    sudo apt-get install python-software-properties

Run the following commands:

    sudo add-apt-repository ppa:neovim-ppa/stable
    sudo apt-get update
    sudo apt-get install neovim

Prerequisites for the Python modules:

    sudo apt-get install python-dev python-pip python3-dev python3-pip

If you're using an older version Ubuntu you must use:

    sudo apt-get install python-dev python-pip python3-dev
    sudo apt-get install python3-setuptools
    sudo easy_install3 pip

For instructions to install the Python modules, see [`:help provider-python`].

If you want to use Neovim for some (or all) of the editor alternatives, use the following commands:

    sudo update-alternatives --install /usr/bin/vi vi /usr/bin/nvim 60
    sudo update-alternatives --config vi
    sudo update-alternatives --install /usr/bin/vim vim /usr/bin/nvim 60
    sudo update-alternatives --config vim
    sudo update-alternatives --install /usr/bin/editor editor /usr/bin/nvim 60
    sudo update-alternatives --config editor

Note, however, that special interfaces, like `view` for `nvim -R`, are not supported.  (See [#1646](https://github.com/neovim/neovim/issues/1646) and [#2008](https://github.com/neovim/neovim/pull/2008).)

### Void-Linux

Neovim can be installed using the xbps package manager

    sudo xbps-install -S neovim

## BSD

### FreeBSD

Neovim can be installed using [`pkg(8)`](http://man.freebsd.org/pkg/8):

    pkg install neovim

or [from the ports tree](https://www.freshports.org/editors/neovim/):

    cd /usr/ports/editors/neovim/ && make install clean

To install the pynvim Python modules using [`pkg(8)`](http://man.freebsd.org/pkg/8) run:

    pkg install py27-pynvim py36-pynvim

### OpenBSD

Neovim can be installed using [`pkg_add(1)`](https://man.openbsd.org/pkg_add):

    pkg_add neovim

or [from the ports tree](https://cvsweb.openbsd.org/cgi-bin/cvsweb/ports/editors/neovim/):

    cd /usr/ports/editors/neovim/ && make install

## Android

[Termux on the Google Play store](https://play.google.com/store/apps/details?id=com.termux) offers a Neovim package.


Install from source
===================

If a package is not provided for your platform, you can build Neovim from source. See [Building-Neovim](https://github.com/neovim/neovim/wiki/Building-Neovim) for details.  If you have the [prerequisites](https://github.com/neovim/neovim/wiki/Building-Neovim#build-prerequisites) then building is easy:

    make CMAKE_BUILD_TYPE=Release
    sudo make install

For Unix-like systems this installs Neovim to `/usr/local`, while for Windows to `C:\Program Files`. Note, however, that this can complicate uninstallation. The following example avoids this by isolating an installation under `$HOME/neovim`:

    rm -r build/  # clear the CMake cache
    make CMAKE_EXTRA_FLAGS="-DCMAKE_INSTALL_PREFIX=$HOME/neovim"
    make install
    export PATH="$HOME/neovim/bin:$PATH"

## Uninstall

To _uninstall_ after `make install`, just delete the `CMAKE_INSTALL_PREFIX` artifacts:

```sh
sudo rm /usr/local/bin/nvim
sudo rm -r /usr/local/share/nvim/
```



[`:help provider-python`]: https://neovim.io/doc/user/provider.html#provider-python