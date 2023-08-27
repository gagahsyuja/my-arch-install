# After Reboot
After booting your freshly installed Arch Linux, we'll be downloading some packages and do some configuration. Remember to use `sudo` everytime you need root access, especially `pacman`.
## Connect to the internet
We had `network-manager` previously installed and enabled the service. To connect to the internet using the tui:
```
$ sudo nmtui
```
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
$ sudo pacman -S pipewire wireplumber pipewire-audio pipewire-pulse
```
## AUR helpers
I'll be using `paru`, to install it make sure you've installed `git`:
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
    `# audio effects    ` easyeffects \
    `# brightness       ` brightnessctl \
    `# browsers         ` firefox \
    `# clipboard        ` wl-clipboard \
    `# editor           ` vim \
    `# file manager     ` nemo \
    `# gui libraries    ` gtk3 qt5-wayland glfw-wayland \
    `# image viewers    ` swayimg \
    `# manual           ` tldr \
    `# screenshots      ` slurp grim \
    `# shell            ` fish starship \
    `# status bars      ` waybar \
    `# terminal         ` kitty \
    `# video players    ` mpv \
    `# wallpaper        ` swaybg
```
It's a great time to try out `paru`:
```
$ paru -S \
    `# browsers         ` librewolf \
    `# code editor      ` vscodium-bin \
    `# fetch            ` nitch \
    `# launchers        ` rofi-lbonn-wayland-git \
    `# tablet driver    ` opentabletdriver \
    `# u know dis       ` spotify discord whatsapp-nativefier
```