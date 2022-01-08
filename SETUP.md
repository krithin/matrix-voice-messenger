# Setup

I typically run the matrix-voice-messenger bot as a Python process on the Raspberry Pi, rather than as a Docker container. This is because it needs to interface with custom hardware on the box, including the combination LED/button, and I haven't tested yet if the drivers and libraries for that that actually work when running through Docker.

## Install the dependencies

### Hardware setup

See the [hardware setup](hardware-setup.md) page.

### Systemwide stuff

1. Install `libolm`. I had to [build this from source](https://gitlab.matrix.org/matrix-org/olm#building) and then install it by hand (with `sudo make install`) because of some incompatibility issues with the version from my package manager at the time of writing (early 2022). On the low-power Pi Zero in the AIY Voice Kit this might take literally hours to complete.
2. Install python libraries: `sudo apt install python3-dev` on my Ubuntu system.
3. Install libffi: `sudo apt install libffi-dev`.
4. Regenerate the system linker cache. I'm not sure why I needed to do this, but I ran into runtime difficulties without it. `sudo rm /etc/ld.so.cache && sudo ldconfig`.

#### Install Python dependencies

1. Clone this repository. I'll assume it was cloned into `/home/pi/matrix-voice-messenger`.
2. Create and activate a Python 3 virtual environment, and set it up with all the right dependencies:

```
python3 -m venv ./env
source env/bin/activate
pip install -U pip
pip install -e .
```

## Installation and Configuration

1. Copy the top-level `matrix-voice-messenger.service` file to an appropriate systemd directory: `sudo cp matrix-voice-messenger.service /etc/systemd/system/`. Then edit that file if necessary to replace the `/home/pi` paths with the path to the place you actually cloned the repository.
    
    You may also need to run `sudo systemctl daemon-reload` here.

2. Copy the config file (`sudo mkdir /etc/matrix-voice-messenger && sudo cp sample.config.yaml /etc/matrix-voice-messenger/config.yaml`) and then edit that file to pass in the actual matrix credentials and homeserver you'd like the bot to use.

3. Enable and start the service! `sudo systemctl start matrix-voice-messenger && sudo systemctl enable matrix-voice-messenger`.

## Testing the bot works

Invite the bot to a room and it should accept the invite and join.

By default nio-template comes with an `echo` command. Let's test this now.
After the bot has successfully joined the room, try sending the following
in a message:

```
!c echo I am a bot!
```

The message should be repeated back to you by the bot.

## Going forwards

Congratulations! Your bot is up and running. Now you can modify the code,
re-run the bot and see how it behaves. Have fun!

## Troubleshooting

If you had any difficulties with this setup process, please [file an
issue](https://github.com/anoadragon453/nio-template/issues]) or come talk
about it in [the matrix room](https://matrix.to/#/#nio-template).
