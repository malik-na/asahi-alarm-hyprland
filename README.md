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

7- btop

```
sudo pacman -S btop
btop
```

if you get locale error, add this to .bashrc or .zshrc

```
export LANG=en_US.utf8
export LC_ALL=en_US.utf8

```


notes -

if some package installation fails, try sudo pacman -Syyu

```
#!/bin/bash
# Omarchy-style environment setup for Asahi Alarm (ARM)
# Tracks installed, skipped, and failed packages

set -e

# Arrays for tracking results
installed=()
already=()
failed=()

# Function: check if package is installed (pacman or flatpak)
is_installed() {
    if pacman -Qi "$1" &>/dev/null; then
        return 0
    elif flatpak list --app | grep -q "$1"; then
        return 0
    else
        return 1
    fi
}

# Function: install pacman package
install_pacman() {
    local pkg=$1
    if is_installed "$pkg"; then
        already+=("$pkg")
    else
        if sudo pacman -S --noconfirm --needed "$pkg"; then
            installed+=("$pkg")
        else
            failed+=("$pkg")
        fi
    fi
}

# Function: install yay (AUR) package
install_yay() {
    local pkg=$1
    if is_installed "$pkg"; then
        already+=("$pkg")
    else
        if yay -S --noconfirm --needed "$pkg"; then
            installed+=("$pkg")
        else
            failed+=("$pkg")
        fi
    fi
}

# Function: install flatpak package
install_flatpak() {
    local pkg=$1
    if is_installed "$pkg"; then
        already+=("$pkg")
    else
        if flatpak install -y flathub "$pkg"; then
            installed+=("$pkg")
        else
            failed+=("$pkg")
        fi
    fi
}

echo ">>> Updating system..."
sudo pacman -Syu --noconfirm

echo ">>> Installing pacman packages..."
for pkg in neovim alacritty chromium libreoffice-fresh evince \
           fzf zoxide ripgrep lazygit btop steam retroarch \
           helix docker tailscale code-oss; do
    install_pacman "$pkg"
done

echo ">>> Installing AUR packages (yay)..."
for pkg in typora zoom 1password spotify cursor zed sublime-text-4 \
           lazydocker prismlauncher dropbox vscodium-bin-arm; do
    install_yay "$pkg"
done

echo ">>> Installing Flatpak apps..."
for pkg in md.obsidian.Obsidian org.localsend.localsend_app \
           com.github.PintaProject.Pinta com.discordapp.Discord; do
    install_flatpak "$pkg"
done

echo ">>> Enabling services..."
sudo systemctl enable --now docker || true
sudo systemctl enable --now tailscaled || true

echo
echo "================ Installation Summary ================"
echo "✅ Installed: ${installed[*]}"
echo "📦 Already installed: ${already[*]}"
echo "❌ Failed: ${failed[*]}"
echo "======================================================"

```
