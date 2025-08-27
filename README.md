# asahi-alarm-hyprland

## steps after installing asahi alarm hyprland

1- Connect Wi-fi -

```
nmcli connection add type wifi ifname wlan0 con-name Airtel_Naeem_Malik ssid "Airtel_Naeem Malik"
nmcli connection modify Airtel_Naeem_Malik wifi-sec.key-mgmt wpa-psk
nmcli connection modify Airtel_Naeem_Malik wifi-sec.psk "Naeem@0447"
nmcli connection up Airtel_Naeem_Malik
```

2- Keyboard backlight -

```
sudo pacman -S brightnessctl
brightnessctl --device=kbd_backlight set 50%
``` 

3- Install essential packages

```
sudo pacman -S base-devel git

```

4- Install yay

```
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si
```

5- Install Flatpak for downloading utilities like Brave, Obsidian etc.

```
sudo pacman -S flatpak
flatpak install flathub md.obsidian.Obsidian
```

6- Lazyvim

```
git clone https://github.com/LazyVim/starter ~/.config/nvim
rm -rf ~/.config/nvim/.git
nvim
```

notes -

if some package installation fails, try sudo pacman -Syyu
