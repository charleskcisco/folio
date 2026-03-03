# Setup instructions

## Step 0: Ensure that your keyboard has the proper layout before enclosing it in your shell
Keyboard layout .json files may be found on [ the FolioGithub](https://github.com/charleskcisco/folio).

Download the appropriate layout (probably the "stabilized" version, unless you're on a very early prototype).

On your computer in Chrome, go to [VIA](https://usevia.app/). Plug in your keyboard, press the "Authorize Device" button, and connect to the Skelett 40.

Load the .json you just downloaded and you'll be good to go.

## Step 1: Flash Raspberry Pi OS Lite
On your computer, download the Raspberry Pi OS Imager, open it with your SD card inserted, and follow these steps:

- Choose "Raspberry Pi Zero 2" W as your device.
- For the operating system, select the category "Raspberry Pi OS (other)", and choose "Raspberry Pi OS Lite (64-bit)".
- Choose the SD card plugged into your device. It should be about 30 gigabytes.
- As your host name, enter something like \[first name or initial]\[surname]-folio (i.e. jsmith-folio).
- For localization settings, select Capital city: Washington DC, Time zone: America/New York, and Keyboard layout: U.S.
- For Linux-based systems, I find that the best username is your first name. Select a password that you'll remember.
- You'll have to enter a Wi-Fi SSID and password. We'll provide those for you in class.
- At this point there's no reason to enable SSH by default. We might talk about how to do this at a later time if needed.
- We also don't need Raspberry Pi Connect.
- You're now at the point in the imager where you want to confirm that you've customized hostname, localization, user account, and Wi-Fi. Assuming that you have done so, you can click the big red button that says "Write."
- Insert your SD card into its slot on the Raspberry Pi and connect your battery.

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
sudo raspi-config # opens Raspberry Pi configuration utility; see below
sudo apt update # checks for system updates
sudo apt upgrade # applies the same
sudo apt install git # allows the cloning of github repositories onto your device
git clone https://github.com/charleskcisco/manuscripts.git
cd manuscripts # moves into Manuscripts directory
./device-setup.sh # moves foot.ini into place, sets up .bashrc and boot script
./app-setup.sh # installs dependencies, sets up venv, reboots (so make sure to do this one last)
```

### Excursus: In Raspi-Config

#### Setup auto login

- 1 System Options
- S6 Auto Login

#### Expand filesystem
- 6 Advanced Options
- A1 Expand Filesystem

Reboot later.

## Step 4: Write
Assuming you have correctly done the above, when your writerdeck reboots, it should boot into Manuscripts and you're ready to start composing.
