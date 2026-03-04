# Setup instructions

## Step 0: Ensure that your keyboard has the proper layout before enclosing it in your shell
Keyboard layout .json files may be found on [the Folio Github](https://github.com/charleskcisco/folio).

Download the appropriate layout (probably the "stabilized" version, unless you're on a very early prototype).

On your computer in Chrome, go to [VIA](https://usevia.app/). Plug in your keyboard, press the "Authorize Device" button, and connect to the Skelett 40.

Load the .json you just downloaded and you'll be good to go.

## Step 1: Flash Raspberry Pi OS Lite
On your computer, download the Raspberry Pi OS Imager, open it with your SD card inserted, and follow these steps:

1. Choose "Raspberry Pi Zero 2" W as your device.
2. For the operating system, select the category "Raspberry Pi OS (other)", and choose "Raspberry Pi OS Lite (64-bit)".
3. Choose the SD card plugged into your device. It should be about 30 gigabytes.
4. As your host name, enter something like \[first name or initial]\[surname]-folio (i.e. jsmith-folio).
5. For localization settings, select Capital city: Washington DC, Time zone: America/New York, and Keyboard layout: U.S.
6. For Linux-based systems, I find that the best username is your first name. Select a password that you'll remember.
7. You'll have to enter a Wi-Fi SSID and password. We'll provide those for you in class.
8. At this point there's no reason to enable SSH by default. We might talk about how to do this at a later time if needed.
9. We also don't need Raspberry Pi Connect.
10. You're now at the point in the imager where you want to confirm that you've customized hostname, localization, user account, and Wi-Fi. Assuming that you have done so, you can click the big red button that says "Write."
11. Insert your SD card into its slot on the Raspberry Pi and connect your battery.

## Step 2: Assemble Folio
You'll need the following parts to assemble:

- [ ] Folio shell
- [ ] Raspberry Pi Zero 2w (with prepped SD card)
- [ ] HMTECH 7 display
- [ ] Skelett 40 keyboard
- [ ] HDMI-to-MiniHDMI
- [ ] Power cable (female USB-C to 2x male micro-USB)
- [ ] Keyboard cable (male micro-USB to male USB-C)

## Step 3: Basic Raspberry Pi Setup
Run the following commands in the following order:

```bash
sudo raspi-config # opens Raspberry Pi configuration utility; see excursus below for instructions
```

### Excursus: In Raspi-Config

#### Setup auto login

- 1 System Options
- S6 Auto Login

#### Expand filesystem
- 6 Advanced Options
- A1 Expand Filesystem

Reboot later.

```bash
sudo apt update # checks for system updates
sudo apt upgrade # applies the same
sudo apt install git # allows the cloning of github repositories onto your device
git clone https://github.com/charleskcisco/manuscripts.git
cd manuscripts # moves into Manuscripts directory
./device-setup.sh # moves foot.ini into place, sets up .bashrc and boot script
./app-setup.sh # installs dependencies, sets up venv, reboots (so make sure to do this one last)
```

## Step 4: Write
Assuming you have correctly done the above, when your writerdeck reboots, it should boot into Manuscripts and you're ready to start composing.

## Appendix: Shell Script Explanations

### device-setup.sh
`device-setup.sh` configures the device to auto-launch Manuscripts at boot. It does three things:

1. **Appends an auto-launch block to `~/.bashrc`.** The block checks whether the current shell is on TTY1 (the physical console) with no graphical session running (`$DISPLAY` and `$WAYLAND_DISPLAY` both unset). If so, it executes `~/start-deck.sh`. A marker comment makes the append idempotent — if it's already present, the script skips it.
2. **Installs `~/start-deck.sh`** by copying a template from `support/start-deck.sh` and substituting the placeholder path with the actual repo directory via `sed`. This script sets `XKB_DEFAULT_OPTIONS` for compose-key support, then launches Cage (a single-window Wayland compositor) running Foot (a Wayland terminal emulator), which in turn `cd`s into the repo and runs Manuscripts.
3. **Copies `foot.ini`** into `~/.config/foot/`, configuring the terminal to use JetBrains Mono at size 13 and hide the mouse cursor while typing.

### app-setup.sh
`app-setup.sh` installs all system-level and Python dependencies. It runs `set -e` so any failure aborts the script immediately. In order:

1. **`apt update` / `apt upgrade -y`**—fetches and applies all available system package updates.
2. **`apt install`**—installs: `micro` and `ranger` (terminal editor and file manager), `pandoc` and `libreoffice` (document conversion), `cups`/`cups-client`/`lpr` (printing), `git` (version control), `cage` and `foot` (Wayland compositor and terminal), `fonts-jetbrains-mono` (UI font), and `python3`/`pip`/`venv` (Python runtime).
3. **Creates a Python virtual environment** in the repo directory (if one doesn't already exist) and installs `prompt_toolkit`, `pygments`, `aiohttp`, and `zeroconf`—the Python libraries Manuscripts depends on.
4. **Reboots** after a 5-second delay to apply kernel and system updates.
