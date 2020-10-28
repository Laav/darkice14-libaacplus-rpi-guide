# darkice14-libaacplus-rpi-guide
A step by step guide/tutorial to get Darkice Release v1.4 (03-01-2020) compiled with libaacplus (aac & mp3) support on a Raspberry PI 4 (4Gb) running Raspberry Pi OS (32-bit) Lite.

## Install latest updates for the OS via APT, I assume you allraedy installed Raspberry Pi OS.

Remove the # symbol before de db-src line (3) in sources.list

```$ sudo nano /etc/apt/sources.list```

```$ sudo apt update```

```$ sudo apt upgrade```

```$ sudo apt-get install dh-autoreconf libtool libtool-bin libasound2-dev libfftw3-dev build-essential devscripts autotools-dev fakeroot dpkg-dev debhelper autotools-dev dh-make quilt ccache libsamplerate0-dev libpulse-dev libaudio-dev lame libjack-jackd2-dev libasound2-dev libtwolame-dev libfaad-dev libflac-dev libmp4v2-dev libshout3-dev libmp3lame-dev libopus-dev wget```

### Install libfaac from source
Change the filenames used in "sudo dpkg" to you're local (downloaded) files

``` mkdir /tmp/build && cd /tmp/build```

```$ apt-get -b source libfaac0 faac```

```ls```

```$ sudo dpkg -i libfaac0_1.29.9.2-2_armhf.deb libfaac-dev_1.29.9.2-2_armhf.deb faac_1.29.9.2-2_armhf.deb```

```$ cd```

### Download libaacplus from tipok.org.ua

```$ mkdir src && cd src```

```$ wget http://tipok.org.ua/downloads/media/aacplus/libaacplus/libaacplus-2.0.2.tar.gz```

```$ tar -xzf libaacplus-2.0.2.tar.gz```

```$ cd libaacplus-2.0.2```

```$ ./autogen.sh --host=arm-unknown-linux-gnueabi --enable-static --enable-shared```

#### Edit (and backup) frontend/au_channel.h file, otherwise compiling will fail! 
Remove all appearances of the word "inline" in frontend/au_channel.h, or use the edited one from Github.

```$ cd frontend/```

```$ sudo cp au_channel.h au_channel.h-org```

```$ wget -O au_channel.h https://raw.githubusercontent.com/Laav/darkice14-libaacplus-rpi-guide/main/au_channel.h```

### Compile libaacplus

```$ cd ..```

```$ make```

```$ sudo make install```

```$ sudo ldconfig```

```$ sudo reboot```

### Install Darkice 1.4 from source

```$ cd src```

```$ wget https://github.com/rafael2k/darkice/releases/download/v1.4/darkice-1.4.tar.gz```

```$ tar -xzf darkice-1.4.tar.gz```

```$ cd darkice-1.4/```

```$ ./configure --with-faac --with-faac-prefix=/usr/lib/arm-linux-gnueabihf --with-opus --with-pulseaudio --with-lame --with-lame-prefix=/usr/lib/arm-linux-gnueabihf --with-alsa --with-jack --with-aacplus --with-samplerate --with-vorbis```

```$ make```

```$ sudo make install```

### Copy and edit (with nano) standard Darkice 1.4 config-file to /etc folder

```$ sudo cp ~/src/darkice-1.4/darkice.cfg /etc/darkice.cfg```

```$ sudo nano /etc/darkice.cfg```

### Make Darkice 1.4 auto start with Raspberry PI boot (thanks LyonelB) and use Supervisor with built in web interface for even more easier/better process monitoring

```$ sudo apt-get install supervisor```

```$ cd /home/pi/```

```$ mkdir config && cd config```

```$ mkdir supervisor && cd supervisor```

```$ wget https://raw.githubusercontent.com/LyonelB/Graffiti/master/darkice-supervisor/darkice.conf```

```$ cd```

```$ sudo ln -s /home/pi/config/supervisor/darkice.conf /etc/supervisor/conf.d/darkice.conf```

#### Edit Supervisor credentials and optional portnumber.
```$ sudo nano /etc/supervisor/supervisord.conf```

```
[inet_http_server]
port = 3773 ; portnumber (You can find the page at: http://raspberrypi:3773)
username = user ; Auth username
password = pass ; Auth password
```

## Sources
Thanks for the support and knowledge to get Darkice 1.4 running on an Raspberry PI 4.

```
https://stmllr.net/blog/live-mp3-streaming-from-audio-in-with-darkice-and-icecast2-on-raspberry-pi/

https://lyonelb.github.io/Darkice-All-codecs-RPi/

https://github.com/coreyk/darkice-libaacplus-rpi-guide/issues/1

https://github.com/rafael2k/darkice/issues/153

https://github.com/LyonelB/Darkice-All-codecs-RPi

https://github.com/rafael2k/darkice
```

