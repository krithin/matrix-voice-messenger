# Matrix Voice Messenger [![Built with matrix-nio](https://img.shields.io/badge/built%20with-matrix--nio-brightgreen)](https://github.com/poljar/matrix-nio) <a href="https://matrix.to/#/#nio-template:matrix.org"><img src="https://img.shields.io/matrix/nio-template:matrix.org?color=blue&label=Join%20the%20Matrix%20Room&server_fqdn=matrix-client.matrix.org" /></a>

## Background

I had a [Voice Kit](https://aiyprojects.withgoogle.com/voice/) from Google's Maker Faire booth sitting around. I don't use voice assistants, but the kit has a Raspberry Pi Zero with a soundcard HAT, and a speaker and microphone all packed in to a pretty neat cardboard box. I figured I could build something using that to send and receive voice messages over [Matrix](https://en.wikipedia.org/wiki/Matrix_(protocol)) - sort of a modern-day answering machine.

## Hardware / Driver Setup

I'm running stock ~~raspbian~~ [Raspberry Pi OS](https://www.raspberrypi.com/software/operating-systems/) on the Pi in the box, so the first step was to get the speaker working. I made [a summary of the instructions](hardware-setup.md) for this, following the [guide to installing Google's AIY software on Raspbian](https://github.com/google/aiyprojects-raspbian/blob/aiyprojects/HACKING.md#install-aiy-software-on-an-existing-raspbian-system).

## Getting started

See [SETUP.md](SETUP.md) for how to setup and run the template project.

## Project structure

The basic structure of the project is the same as that of any other nio-template project.