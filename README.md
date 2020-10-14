# darkice14-libaacplus-rpi-guide
Steps to get Darkice 1.4 compiled with libaacplus support on a Raspberry PI 4

## Install latest updates for the OS
```sudo apt-get update```

```$ sudo apt-get upgrade```

```$ sudo apt-get install dh-autoreconf libtool libtool-bin libasound2-dev libfftw3-dev build-essential devscripts autotools-dev fakeroot dpkg-dev debhelper autotools-dev dh-make quilt ccache libsamplerate0-dev libpulse-dev libaudio-dev lame libjack-jackd2-dev libasound2-dev libtwolame-dev libfaad-dev libflac-dev libmp4v2-dev libshout3-dev libmp3lame-dev libopus-dev wget```

### Install libfaac from source
Change the filenames in third step to downloaded ones (latest version available)
``` mkdir /tmp/build && cd /tmp/build```

```$ apt-get -b source libfaac0 faac```

```$ sudo dpkg -i libfaac0_1.29.9.2-2_armhf.deb  libfaac-dev_1.29.9.2-2_armhf.deb faac_1.29.9.2-2_armhf.deb```

```$ cd<```

### Download libaacplus from tipok.org.ua
```$ mkdir src && cd src```

```>$ wget http://tipok.org.ua/downloads/media/aacplus/libaacplus/libaacplus-2.0.2.tar.gz```

```$ tar -xzf libaacplus-2.0.2.tar.gz```

```$ cd libaacplus-2.0.2```

```$ ./autogen.sh --host=arm-unknown-linux-gnueabi --enable-static --enable-shared```

#### Edit (and backup) frontend/au_channel.h file to get compilation works again
Remove all appearances of the word "inline" in frontend/au_channel.h, or use the edited one from pastebin.

```$ cd frontend/```

```$ sudo cp au_channel.h au_channel.h-org```

```$ wget -O au_channel.h https://pastebin.com/raw/w7s6c021```

### Compile libaacplus

```$ make```

```$ sudo make install```

```$ sudo ldconfig```

```$ sudo reboot```

### Install Darkice 1.4 from source

```$ cd src```

```$ wget https://github.com/rafael2k/darkice/releases/download/v1.4/darkice-1.4.tar.gz```

```$ tar -xzf darkice-1.4.tar.gz```

```$ ./configure --with-faac --with-faac-prefix=/usr/lib/arm-linux-gnueabihf --with-opus --with-pulseaudio --with-lame --with-lame-prefix=/usr/lib/arm-linux-gnueabihf --with-alsa --with-jack --with-aacplus --with-samplerate --with-vorbis```

```$ make```

```$ sudo make install```

### Copy and edit (with nano) standard Darkice 1.4 config-file to /etc folder

```$ sudo cp ~/src/darkice-1.4/darkice.cfg /etc/darkice.cfg```

```$ sudo nano /etc/darkice.cfg```

## Sources
Thanks for the support and knowledge to get Darkice 1.4 running on an Raspberry PI 4.

```
https://stmllr.net/blog/live-mp3-streaming-from-audio-in-with-darkice-and-icecast2-on-raspberry-pi/

https://lyonelb.github.io/Darkice-All-codecs-RPi/

https://github.com/coreyk/darkice-libaacplus-rpi-guide/issues/1

https://github.com/rafael2k/darkice/issues/153```

