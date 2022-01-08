# AIY Voice Kit Driver Setup
## From stock Raspbian to playing a sound

This is largely lifted from [Google's AIY Projects guide](https://github.com/google/aiyprojects-raspbian/blob/aiyprojects/HACKING.md#install-aiy-software-on-an-existing-raspbian-system). It assumes you're starting with an assembled [Voice Kit](https://aiyprojects.withgoogle.com/voice/) - specifically the [V2, with Voice Bonnet](https://pinout.xyz/pinout/aiy_voice_bonnet) - and that you've got [stock Raspbian running](https://www.raspberrypi.global/raspberry-pi-os-software.html) on the Pi inside it and have keyboard or SSH access set up.

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