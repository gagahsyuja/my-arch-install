# After Reboot
After booting your freshly installed Arch Linux, we'll be downloading some packages and do some configuration. Remember to use `sudo` everytime you need root access, especially `pacman`.
## Connect to the internet
We had `networkmanager` previously installed and enabled the service. To connect to the internet using the tui:
```
$ sudo nmtui
```
## Change hardware clock to localtime
```
# timedatectl set-local-rtc 1
```
By doing this, Windows time will not be messed up. However, it is recommended to configure Windows to use UTC, rather than Linux to use localtime. You can do it by reading [UTC in Microsoft Windows](https://wiki.archlinux.org/title/System_time#UTC_in_Microsoft_Windows).
## Graphical user interface
As I said in the beginning, we'll be using `wayland` as our display server. We also need compositor, `hyprland` is cool but I'll be using `sway` instead. Install the compositor of your choice and its dependencies:
```
$ sudo pacman -S --needed sway polkit xorg-xwayland
```
## User directories
Well-known user directories like Downloads or Documents are created by the `xdg-user-dirs-update.service` user service, provided by the `xdg-user-dirs` package. To install and create the directories:
```
$ sudo pacman -S xdg-user-dirs && xdg-user-dirs-update
```
## Audio drivers
You can use `pulseaudio` but `pipewire` is recommended:
```
$ sudo pacman -S pipewire pipewire-pulse wireplumber
```
Then enable `pipewire-pulse.service`:
```
$ systemctl --user enable --now pipewire-pulse
```
## AUR helpers
I'll be using `paru`. Make sure you've installed `git`:
```
$ sudo pacman -S --needed git
```
Let's change directory to `Downloads` and instal `paru`:
```
$ cd Downloads
$ git clone https://aur.archlinux.org/paru.git && cd paru
$ makepkg -si
```
This is optional and may be not recommended, but anyway:
```
$ sudo vim /etc/paru.conf
```
Under `[options]`, add this line:
```
SkipReview
```
This will stop paru asking you to review the code every. single. time.
## Additional packages
I need this and you may not, so choose wisely:
```
$ sudo pacman -S --needed \
    `# archieve         ` p7zip \
    `# audio effects    ` calf easyeffects \
    `# brightness       ` brightnessctl \
    `# clipboard        ` wl-clipboard \
    `# editor           ` vim \
    `# file manager     ` nemo \
    `# gui libraries    ` gtk3 qt5-wayland glfw-wayland \
    `# image viewers    ` swayimg \
    `# manual           ` tldr \
    `# screenshots      ` slurp grim \
    `# spotify          ` spotify-launcher \
    `# shell            ` fish starship \
    `# status bars      ` waybar \
    `# terminal         ` kitty \
    `# video players    ` mpv \
    `# wallpaper        ` swaybg \
    `# ntfs driver      ` ntfs-3g
```
It's a great time to try out `paru`:
```
$ paru -S \
    `# browsers         ` google-chrome \
    `# code editor      ` vscodium-bin \
    `# fetch            ` nitch \
    `# firmware         ` mkinitcpio-firmware \
    `# launchers        ` rofi-lbonn-wayland-git \
    `# tablet driver    ` opentabletdriver \
    `# social           ` discord whatsapp-nativefier-notray-hook
```
