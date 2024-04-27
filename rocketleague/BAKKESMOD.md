# Installing BakkesMod for Rocket League in Linux

Latest documentation available at: https://nowsci.com/#/linux-gaming/?id=installing-bakkesmod-for-rocket-league-in-linux

While I have not tried this on Steam Deck yet, the process should work similarly since ProtonUp-Qt and SteamTinkerLaunch function basically the same way on SD. Please feel free to submit a PR to update this document with SD (or CentOS) specific instructions. This may seem complicated at first, but as you go through the steps you will realize it's pretty basic.

This guide applies to Steam, but should work for Heroic other than changing paths. A PR is welcome for updates to cover this as well.

**This guide assumes you already have Rocket League installed via Proton in Linux**

The method:
- Use [gam](https://github.com/Fmstrat/gam) to install ProtonUp-Qt (and any applications) directly from GitHub
- Use [ProtonUp-Qt](https://github.com/DavidoTek/ProtonUp-Qt), which manages Steam add-ons to install SteamTinkerLaunch
- Use [SteamTinkerLaunch](https://github.com/sonic2kk/steamtinkerlaunch), which allows you to customize game startup, to start [BakkesMod](https://github.com/bakkesmodorg/BakkesModInjectorCpp/) prior to Rocket League

## Step 1: Make sure Rocket League starts
This may require the following steps in Linux:
- Open Rocket League `Properties` in Steam
- Choose `Compatibility`
- Choose `Force the use of a specific Steam Play compatibility tool`
- Choose `Proton 7.0-6`

## Step 2: Install gam
Gam will allow you to easily install and update applications from GitHub. This is not required if you wish to install ProtonUp-Qt manually (below).
```bash
sudo apt install -y ncurses-bin debianutils jq curl tar xz-utils
sudo curl https://raw.githubusercontent.com/Fmstrat/gam/master/gam -o /usr/local/bin/gam
sudo chmod 755 /usr/local/bin/gam
```

## Step 3: Use gam to Install ProtonUp-Qt
While there are many ways to install STL, ProtonUp-Qt works in most platforms.
```bash
sudo gam install DavidoTek/ProtonUp-Qt
```
If you wish to do this manually, you can use the AppImage from the releases page: https://github.com/DavidoTek/ProtonUp-Qt/releases

## Step 4: Use ProtonUp-Qt to install SteamTinkerLauncher
Start ProtonUp-Qt:
```bash
protonup-qt
```
- Click `Add version`
- Choose `SteamTinkerLauncher`
- Add and exit ProtonUp-Qt

### Dependencies
You may get a notification for dependencies missing. If so, follow this guide: https://github.com/sonic2kk/steamtinkerlaunch/wiki/Installation#manual-installation

The most common missing item is `yad`, as many Debian based distrobutions do not include version 7.2 or higher. There are mutliple ways recommended to install, however most did not work for me, including the AppImage. Instead, I compiled from scratch which was very easy and only took about a minute:
```bash
sudo apt install -y git libglib2.0-dev libtool autoconf automake intltool libgtk-3-dev build-essential
cd /tmp
git clone https://github.com/v1cont/yad.git yad-dialog-code
cd yad-dialog-code
autoreconf -ivf && intltoolize
./configure && make
sudo make install
gtk-update-icon-cache
cd -
```

## Step 5: Get BakkesMod Installer
Download the BakkesMod installer:
```bash
cd ~/.steam/debian-installation/steamapps/common/rocketleague/Binaries/Win64
wget https://github.com/bakkesmodorg/BakkesModInjectorCpp/releases/latest/download/BakkesModSetup.zip
unzip BakkesModSetup.zip
rm -f BakkesModSetup.zip
```

## Step 6: Set up Rocket League to use SteamTinkerLaunch
- Open Rocket League `Properties` in Steam
- Choose `Compatibility`
- Choose `Force the use of a specific Steam Play compatibility tool`
- Choose `Steam Tinker Launch`

## Step 7: Set up BakkesMod
- `Play` Rocket League
- An interstitial will pop up, choose `Main menu` (if you don't, the game will start)
- Choose `Game menu`
- If you needed to set the Proton version above, then also `Set proton version` on this screen
- Check `Use custom command`
- Choose `~/.steam/debian-installation/steamapps/common/rocketleague/Binaries/Win64/BakkesModSetup.exe`
- Click `Save and Play`

This will start the BakkesMod installer. Install as normal, and say `Yes` to the update if it asks. Exit BakkesMod when completed. Rocket League will start, then exit it as well.

## Step 8: Make BakkesMod start before Rocket League
Find the BakkesMod executable:
```bash
find ~/.steam/debian-installation/steamapps -name BakkesMod.exe
```
- `Play` Rocket League
- An interstitial will pop up, choose `Main menu` (if you don't, the game will start)
- Choose `Game menu`
- Change the custom command to the above located `BakkesMod.exe`
- Check `Fork custom commmand` to put BakkesMod in the background after starting it
- `Save and Play`

You will now see the BakkesMod window pop up alongside Rocket League!