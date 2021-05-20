## Package no longer mantained. Please check latest versions at:
https://aur.archlinux.org/packages/davinci-resolve-studio

# davinci-resolve-studio for Arch Linux
Archlinux Package to install Blackmagic's Davinci Resolve Studio, based on https://aur.archlinux.org/packages/davinci-resolve/

This package adds the package name and sha256 from the studio version zip file, and also adds an udev rule for the dongle. Please let me know of any [issues here](https://github.com/codibit/davinci-resolve-studio/issues).

## ATTENTION:
A new packaging structure has been released since resolve 15.2.2, so this package might need some testing. Please report any issues or improvements.

## ACTIVATION:

If you're using a dongle, remember to have it plugged in before starting the application.

If you're using an activation/serial number, remember to run the app as root in order to activate.
## INSTALLATION:

To install, simply download the Davinci Resolve Studio installer from [Blackmagic's 
website](https://www.blackmagicdesign.com/products/davinciresolve/) to your Downloads directory. The DaVinci_Resolve_Studio_X.Y.Z_Linux.zip 
file (where X.Y.Z is te current package version) should be placed into /home/$youruser/Downloads or inside the package directory.

### Automatic installation:
You should be able to automatically install from the AUR repository with a tool such as [yay](https://github.com/Jguer/yay):

`yay -S davinci-resolve-studio`

### Manual installation:
* If you want to manually install, clone this repository from [GitHub](https://github.com/codibit/davinci-resolve-studio) _or_ [AUR](https://aur.archlinux.org/packages/davinci-resolve-studio/):

`git clone https://github.com/codibit/davinci-resolve-studio.git`

_or_

`git clone https://aur.archlinux.org/davinci-resolve-studio.git`

* run makepkg inside the directory (as your regular user):

`cd davinci-resolve-studio`

`makepkg`

* and finally install the created package with pacman (as root):

`pacman -U davinci-resolve-studio-X.Y.Z-0-x86_64.pkg.tar.xz`

## CREDITS:
All the package credits go to the [davinci-resolve](https://aur.archlinux.org/packages/davinci-resolve/) package mantainer and contributors in [https://aur.archlinux.org/packages/davinci-resolve/](https://aur.archlinux.org/packages/davinci-resolve/) and [Daniel Tufvesson](http://www.danieltufvesson.com)'s [makeresolvedeb script](http://www.danieltufvesson.com/makeresolvedeb)
