# AIY Voice Kit Driver Setup

## Overview

There are two ways to get the sound card and button set up on the Raspberry Pi inside the AIY kit box. One is to use the prebuilt AIY disk image. If you go this route there is nothing more to do here - just boot off that disk image, and maybe run a regular `sudo apt update && sudo apt upgrade`, then go back to the rest of the setup instructions in [the SETUP page](SETUP.md).

The other approach is to start with a stock release of Raspberry Pi OS and then follow the rest of this guide to set up the sound card and LED button drivers.

## From stock Raspbian to playing a sound

This is largely lifted from [Google's AIY Projects guide](https://github.com/google/aiyprojects-raspbian/blob/aiyprojects/HACKING.md#install-aiy-software-on-an-existing-raspbian-system). It assumes you're starting with an assembled [Voice Kit](https://aiyprojects.withgoogle.com/voice/) - specifically the [V2, with Voice Bonnet](https://pinout.xyz/pinout/aiy_voice_bonnet) - and that you've got the Buster release of [Raspbian](https://www.raspberrypi.global/raspberry-pi-os-software.html) running on the Pi inside it and have keyboard or SSH access set up.

NOTE: I also tried this with the the `bullseye` release (from Oct/Nov 2021), but for mysterious reasons was not able to get any sound to come out of the raspberry pi speaker. You should probably stick with `buster` (from before 2021).

### 1. Add the AIY Debian packages repo

```bash
echo "deb https://packages.cloud.google.com/apt aiyprojects-stable main" | sudo tee /etc/apt/sources.list.d/aiyprojects.list
```

Add Google package keys:

```bash
curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
```

Update and install the latest system updates (including kernel):

```bash
sudo apt update
sudo apt upgrade
```

Reboot after update:

```bash
sudo reboot
```

### 2.  RGB Button Driver

```bash
sudo apt install leds-ktd202x-dkms
```

### 3. Install Voice Bonnet packages

Voice Bonnet requires driver installation:
```bash
sudo apt install aiy-voicebonnet-soundcard-dkms
```

Disable built-in audio:

```bash
sudo sed -i -e "s/^dtparam=audio=on/#\0/" /boot/config.txt
```

Install PulseAudio:

```bash
sudo apt install pulseaudio
sudo mkdir -p /etc/pulse/daemon.conf.d/
echo "default-sample-rate = 48000" | sudo tee /etc/pulse/daemon.conf.d/aiy.conf
```

Reboot:

```bash
sudo reboot
```

### 4. Verify that you can record audio:

```bash
arecord -f cd test.wav
```

...and play a sound:

```bash
aplay test.wav
```

### 5. Install `vlc`

We'll be using `cvlc` to play ogg audio files from the command line (the version of `oggdec` available in the buster repository is too old).

```bash
sudo apt install vlc
```