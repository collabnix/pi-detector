[![License: CC BY-NC-SA 4.0](https://img.shields.io/badge/License-CC%20BY--NC--SA%204.0-green.svg)](http://creativecommons.org/licenses/by-nc-sa/4.0/)

# pi-detector
Raspberry Pi Facial Recognition using AWS Rekognition and Pi-Timolo

### Description
Pi-detector is used with [Pi-Timolo](https://github.com/pageauc/) to search motion generated images for face matches by leveraging AWS Rekognition. In its current state, matches are wrote to event.log. With some additional creativity and work, you could send out a notification or allow/deny access to a room with minimal changes. The install script will place the appropriate files in /etc/rc.local to start on boot.  

### Build Requirements
Raspberry Pi (Tested with Rpi 3) <br />
Picamera <br />
AWS Rekognition Access (Free tier options available) <br />

As an alternative, this set of scripts can be modified to watch any directory that contains images. For example, if you collect still images from another camera and save them to disk, you can alter the image path to run facial recognition against any new photo that is created.

### Install
Setup a Raspberry Pi with Raspbian Jessie <br />
https://www.raspberrypi.org/downloads/raspbian/ <br />

Clone this repo and install:<br />
git clone https://github.com/af001/pi-detector.git<br />
cd pi-detector/scripts<br />
sudo chmod +x install.sh<br />
sudo ./install<br />

```
root@node2:~# systemctl start docker
root@node2:~# docker version
Client:
 Version:           18.09.0
 API version:       1.39
 Go version:        go1.10.4
 Git commit:        4d60db4
 Built:             Wed Nov  7 00:57:21 2018
 OS/Arch:           linux/arm
 Experimental:      false

Server: Docker Engine - Community
 Engine:
  Version:          18.09.0
  API version:      1.39 (minimum version 1.12)
  Go version:       go1.10.4
  Git commit:       4d60db4
  Built:            Wed Nov  7 00:17:57 2018
  OS/Arch:          linux/arm
  Experimental:     false
root@node2:~# clear
root@node2:~# docker version
Client:
 Version:           18.09.0
 API version:       1.39
 Go version:        go1.10.4
 Git commit:        4d60db4
 Built:             Wed Nov  7 00:57:21 2018
 OS/Arch:           linux/arm
 Experimental:      false

Server: Docker Engine - Community
 Engine:
  Version:          18.09.0
  API version:      1.39 (minimum version 1.12)
  Go version:       go1.10.4
  Git commit:       4d60db4
  Built:            Wed Nov  7 00:17:57 2018
  OS/Arch:          linux/arm
  Experimental:     false
root@node2:~# ls
docker-cctv-raspbian              portainer-agent-stack.yml      tiny-cloud
dockerlabs                        real_time_object_detection.py  v19.03.0-beta1.zip
MobileNetSSD_deploy.prototxt.txt  rpi-motion-mmal
pi_object_detection.py            tf-opencv
root@node2:~# git clone https://github.com/collabnix/pi-detector
Cloning into 'pi-detector'...
remote: Enumerating objects: 156, done.
remote: Total 156 (delta 0), reused 0 (delta 0), pack-reused 156
Receiving objects: 100% (156/156), 27.45 KiB | 0 bytes/s, done.
Resolving deltas: 100% (69/69), done.
root@node2:~# cd pi
bash: cd: pi: No such file or directory
root@node2:~# cd pi-detector/
root@node2:~/pi-detector# cd scripts/
root@node2:~/pi-detector/scripts# chmod +x install.sh
root@node2:~/pi-detector/scripts# ./install.sh
[+] Updating and installing dependencies...
Get:1 http://archive.raspberrypi.org/debian stretch InRelease [25.4 kB]
Get:2 https://download.docker.com/linux/raspbian stretch InRelease [31.1 kB]
Get:3 http://raspbian.raspberrypi.org/raspbian stretch InRelease [15.0 kB]
Get:4 http://archive.raspberrypi.org/debian stretch/main armhf Packages [222 kB]
Get:5 http://raspbian.raspberrypi.org/raspbian stretch/main armhf Packages [11.7 MB]
Get:6 http://archive.raspberrypi.org/debian stretch/ui armhf Packages [44.9 kB]
Fetched 12.0 MB in 19s (627 kB/s)
Reading package lists... Done
Reading package lists... Done
Building dependency tree
Reading state information... Done
Calculating upgrade... Done
The following packages were automatically installed and are no longer required:
  coinor-libipopt1v5 libgmime-2.6-0 libgpgme11 libmumps-seq-4.10.0 liboauth0 libraw15
  wolframscript
Use 'sudo apt autoremove' to remove them.
The following packages have been kept back:
  scratch2
The following packages will be upgraded:
  chromium-browser chromium-browser-l10n chromium-codecs-ffmpeg-extra containerd.io
  libasound2 libasound2-data libbasicusageenvironment1 libgroupsock8 liblivemedia57
  libobrender32v5 libobt2v5 libpam-systemd libraspberrypi-bin libraspberrypi-dev
  libraspberrypi-doc libraspberrypi0 libsystemd0 libudev1 libusageenvironment3
  libwbclient0 lxplug-network openbox python-six python3-six raspberrypi-bootloader
  raspberrypi-kernel raspi-gpio rc-gui rpd-plym-splash rpi-chromium-mods samba-common
  systemd systemd-sysv tzdata udev wget
36 upgraded, 0 newly installed, 0 to remove and 1 not upgraded.
Need to get 186 MB of archives.
After this operation, 116 MB of additional disk space will be used.
Get:1 http://archive.raspberrypi.org/debian stretch/main armhf chromium-browser-l10n all 72.0.3626.121-0+rpt4 [2,801 kB]
Get:2 http://raspbian.mirror.net.in/raspbian/raspbian stretch/main armhf systemd-sysv armhf 232-25+deb9u11 [82.4 kB]
Get:3 https://download.docker.com/linux/raspbian stretch/stable armhf containerd.io armhf 1.2.5-1 [10.7 MB]
Get:4 http://raspbian.mirror.net.in/raspbian/raspbian stretch/main armhf libpam-systemd armhf 232-25+deb9u11 [175 kB]
Get:5 http://raspbian.mirror.net.in/raspbian/raspbian stretch/main armhf libsystemd0 armhf 232-25+deb9u11 [260 kB]
Get:6 http://raspbian.mirror.net.in/raspbian/raspbian stretch/main armhf systemd armhf 232-25+deb9u11 [2,223 kB]
Get:8 http://archive.raspberrypi.org/debian stretch/main armhf libasound2 armhf 1.1.3-5+rpt4 [441 kB]
Get:9 http://archive.raspberrypi.org/debian stretch/main armhf libasound2-data all 1.1.3-5+rpt4 [173 kB]
Get:10 http://archive.raspberrypi.org/debian stretch/main armhf libraspberrypi-doc armhf 1.20190401-1 [31.4 MB]
Get:7 http://raspbian.mirror.net.in/raspbian/raspbian stretch/main armhf udev armhf 232-25+deb9u11 [1,073 kB]
Get:20 http://archive.raspberrypi.org/debian stretch/main armhf libraspberrypi-bin armhf 1.20190401-1 [331 kB]
Get:21 http://archive.raspberrypi.org/debian stretch/main armhf libraspberrypi-dev armhf 1.20190401-1 [405 kB]
Get:22 http://archive.raspberrypi.org/debian stretch/main armhf raspberrypi-kernel armhf 1.20190401-1 [33.4 MB]
Get:11 http://raspbian.mirror.net.in/raspbian/raspbian stretch/main armhf libudev1 armhf 232-25+deb9u11 [121 kB]
Get:12 http://raspbian.mirror.net.in/raspbian/raspbian stretch/main armhf libwbclient0 armhf 2:4.5.16+dfsg-1+deb9u1 [121 kB]
Get:13 http://raspbian.mirror.net.in/raspbian/raspbian stretch/main armhf samba-common all 2:4.5.16+dfsg-1+deb9u1 [174 kB]
Get:14 http://raspbian.mirror.net.in/raspbian/raspbian stretch/main armhf tzdata all 2019a-0+deb9u1 [273 kB]
Get:15 http://raspbian.mirror.net.in/raspbian/raspbian stretch/main armhf wget armhf 1.18-5+deb9u3 [768 kB]
Get:16 http://raspbian.mirror.net.in/raspbian/raspbian stretch/main armhf libbasicusageenvironment1 armhf 2016.11.28-1+deb9u2 [19.9 kB]
Get:17 http://raspbian.mirror.net.in/raspbian/raspbian stretch/main armhf libgroupsock8 armhf 2016.11.28-1+deb9u2 [25.0 kB]
Get:18 http://raspbian.mirror.net.in/raspbian/raspbian stretch/main armhf liblivemedia57 armhf 2016.11.28-1+deb9u2 [265 kB]
Get:19 http://raspbian.mirror.net.in/raspbian/raspbian stretch/main armhf libusageenvironment3 armhf 2016.11.28-1+deb9u2 [12.3 kB]
Get:23 http://archive.raspberrypi.org/debian stretch/main armhf libraspberrypi0 armhf 1.20190401-1 [838 kB]
Get:24 http://archive.raspberrypi.org/debian stretch/main armhf raspberrypi-bootloader armhf 1.20190401-1 [3,565 kB]
Get:25 http://archive.raspberrypi.org/debian stretch/main armhf chromium-browser armhf 72.0.3626.121-0+rpt4 [84.8 MB]
Get:26 http://archive.raspberrypi.org/debian stretch/main armhf chromium-codecs-ffmpeg-extra armhf 72.0.3626.121-0+rpt4 [1,756 kB]
Get:27 http://archive.raspberrypi.org/debian stretch/ui armhf rpi-chromium-mods armhf 20190409 [9,182 kB]
Get:28 http://archive.raspberrypi.org/debian stretch/ui armhf libobt2v5 armhf 3.6.1-4+rpi10 [62.2 kB]
Get:29 http://archive.raspberrypi.org/debian stretch/ui armhf libobrender32v5 armhf 3.6.1-4+rpi10 [69.8 kB]
Get:30 http://archive.raspberrypi.org/debian stretch/ui armhf rc-gui armhf 1.23 [42.5 kB]
Get:31 http://archive.raspberrypi.org/debian stretch/ui armhf lxplug-network armhf 0.14 [31.5 kB]
Get:32 http://archive.raspberrypi.org/debian stretch/ui armhf openbox armhf 3.6.1-4+rpi10 [281 kB]
Get:33 http://archive.raspberrypi.org/debian stretch/ui armhf python-six all 1.12.0-0+rpt1 [13.4 kB]
Get:34 http://archive.raspberrypi.org/debian stretch/ui armhf python3-six all 1.12.0-0+rpt1 [13.4 kB]
Get:35 http://archive.raspberrypi.org/debian stretch/main armhf raspi-gpio armhf 0.20190304 [8,972 B]
Get:36 http://archive.raspberrypi.org/debian stretch/ui armhf rpd-plym-splash armhf 0.18 [32.2 kB]
Fetched 186 MB in 1min 46s (1,742 kB/s)
Extracting templates from packages: 100%
Preconfiguring packages ...
(Reading database ... 99754 files and directories currently installed.)
Preparing to unpack .../systemd-sysv_232-25+deb9u11_armhf.deb ...
Unpacking systemd-sysv (232-25+deb9u11) over (232-25+deb9u9) ...
Preparing to unpack .../libpam-systemd_232-25+deb9u11_armhf.deb ...
Unpacking libpam-systemd:armhf (232-25+deb9u11) over (232-25+deb9u9) ...
Preparing to unpack .../libsystemd0_232-25+deb9u11_armhf.deb ...
Unpacking libsystemd0:armhf (232-25+deb9u11) over (232-25+deb9u9) ...
Setting up libsystemd0:armhf (232-25+deb9u11) ...
(Reading database ... 99754 files and directories currently installed.)
Preparing to unpack .../systemd_232-25+deb9u11_armhf.deb ...
Unpacking systemd (232-25+deb9u11) over (232-25+deb9u9) ...
Preparing to unpack .../udev_232-25+deb9u11_armhf.deb ...
Unpacking udev (232-25+deb9u11) over (232-25+deb9u9) ...
Preparing to unpack .../libudev1_232-25+deb9u11_armhf.deb ...
Unpacking libudev1:armhf (232-25+deb9u11) over (232-25+deb9u9) ...
Setting up libudev1:armhf (232-25+deb9u11) ...
(Reading database ... 99754 files and directories currently installed.)
Preparing to unpack .../00-chromium-browser-l10n_72.0.3626.121-0+rpt4_all.deb ...
Unpacking chromium-browser-l10n (72.0.3626.121-0+rpt4) over (65.0.3325.181-0+rpt4) ...
Preparing to unpack .../01-libasound2_1.1.3-5+rpt4_armhf.deb ...
Unpacking libasound2:armhf (1.1.3-5+rpt4) over (1.1.3-5+rpi3) ...
Preparing to unpack .../02-libasound2-data_1.1.3-5+rpt4_all.deb ...
Unpacking libasound2-data (1.1.3-5+rpt4) over (1.1.3-5+rpi3) ...
Preparing to unpack .../03-libraspberrypi-doc_1.20190401-1_armhf.deb ...
Unpacking libraspberrypi-doc (1.20190401-1) over (1.20190215-1) ...
Preparing to unpack .../04-libraspberrypi-bin_1.20190401-1_armhf.deb ...
Unpacking libraspberrypi-bin (1.20190401-1) over (1.20190215-1) ...
Preparing to unpack .../05-libraspberrypi-dev_1.20190401-1_armhf.deb ...
Unpacking libraspberrypi-dev (1.20190401-1) over (1.20190215-1) ...
Preparing to unpack .../06-raspberrypi-kernel_1.20190401-1_armhf.deb ...
Adding 'diversion of /boot/bcm2708-rpi-0-w.dtb to /usr/share/rpikernelhack/bcm2708-rpi-0-w.dtb by rpikernelhack'
Adding 'diversion of /boot/bcm2708-rpi-b-plus.dtb to /usr/share/rpikernelhack/bcm2708-rpi-b-plus.dtb by rpikernelhack'
Adding 'diversion of /boot/bcm2708-rpi-b.dtb to /usr/share/rpikernelhack/bcm2708-rpi-b.dtb by rpikernelhack'
Adding 'diversion of /boot/bcm2708-rpi-cm.dtb to /usr/share/rpikernelhack/bcm2708-rpi-cm.dtb by rpikernelhack'
Adding 'diversion of /boot/bcm2709-rpi-2-b.dtb to /usr/share/rpikernelhack/bcm2709-rpi-2-b.dtb by rpikernelhack'
Adding 'diversion of /boot/bcm2710-rpi-3-b-plus.dtb to /usr/share/rpikernelhack/bcm2710-rpi-3-b-plus.dtb by rpikernelhack'
Adding 'diversion of /boot/bcm2710-rpi-3-b.dtb to /usr/share/rpikernelhack/bcm2710-rpi-3-b.dtb by rpikernelhack'
Adding 'diversion of /boot/bcm2710-rpi-cm3.dtb to /usr/share/rpikernelhack/bcm2710-rpi-cm3.dtb by rpikernelhack'
Adding 'diversion of /boot/kernel.img to /usr/share/rpikernelhack/kernel.img by rpikernelhack'
Adding 'diversion of /boot/kernel7.img to /usr/share/rpikernelhack/kernel7.img by rpikernelhack'
Adding 'diversion of /boot/COPYING.linux to /usr/share/rpikernelhack/COPYING.linux by rpikernelhack'
Adding 'diversion of /boot/overlays/3dlab-nano-player.dtbo to /usr/share/rpikernelhack/overlays/3dlab-nano-player.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/README to /usr/share/rpikernelhack/overlays/README by rpikernelhack'
Adding 'diversion of /boot/overlays/adau1977-adc.dtbo to /usr/share/rpikernelhack/overlays/adau1977-adc.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/adau7002-simple.dtbo to /usr/share/rpikernelhack/overlays/adau7002-simple.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/ads1015.dtbo to /usr/share/rpikernelhack/overlays/ads1015.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/ads1115.dtbo to /usr/share/rpikernelhack/overlays/ads1115.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/ads7846.dtbo to /usr/share/rpikernelhack/overlays/ads7846.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/adv7282m.dtbo to /usr/share/rpikernelhack/overlays/adv7282m.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/adv728x-m.dtbo to /usr/share/rpikernelhack/overlays/adv728x-m.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/akkordion-iqdacplus.dtbo to /usr/share/rpikernelhack/overlays/akkordion-iqdacplus.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/allo-boss-dac-pcm512x-audio.dtbo to /usr/share/rpikernelhack/overlays/allo-boss-dac-pcm512x-audio.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/allo-digione.dtbo to /usr/share/rpikernelhack/overlays/allo-digione.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/allo-katana-dac-audio.dtbo to /usr/share/rpikernelhack/overlays/allo-katana-dac-audio.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/allo-piano-dac-pcm512x-audio.dtbo to /usr/share/rpikernelhack/overlays/allo-piano-dac-pcm512x-audio.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/allo-piano-dac-plus-pcm512x-audio.dtbo to /usr/share/rpikernelhack/overlays/allo-piano-dac-plus-pcm512x-audio.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/applepi-dac.dtbo to /usr/share/rpikernelhack/overlays/applepi-dac.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/at86rf233.dtbo to /usr/share/rpikernelhack/overlays/at86rf233.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/audioinjector-addons.dtbo to /usr/share/rpikernelhack/overlays/audioinjector-addons.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/audioinjector-ultra.dtbo to /usr/share/rpikernelhack/overlays/audioinjector-ultra.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/audioinjector-wm8731-audio.dtbo to /usr/share/rpikernelhack/overlays/audioinjector-wm8731-audio.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/audiosense-pi.dtbo to /usr/share/rpikernelhack/overlays/audiosense-pi.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/audremap.dtbo to /usr/share/rpikernelhack/overlays/audremap.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/balena-fin.dtbo to /usr/share/rpikernelhack/overlays/balena-fin.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/bmp085_i2c-sensor.dtbo to /usr/share/rpikernelhack/overlays/bmp085_i2c-sensor.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/dht11.dtbo to /usr/share/rpikernelhack/overlays/dht11.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/dionaudio-loco-v2.dtbo to /usr/share/rpikernelhack/overlays/dionaudio-loco-v2.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/dionaudio-loco.dtbo to /usr/share/rpikernelhack/overlays/dionaudio-loco.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/dpi18.dtbo to /usr/share/rpikernelhack/overlays/dpi18.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/dpi24.dtbo to /usr/share/rpikernelhack/overlays/dpi24.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/dwc-otg.dtbo to /usr/share/rpikernelhack/overlays/dwc-otg.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/dwc2.dtbo to /usr/share/rpikernelhack/overlays/dwc2.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/enc28j60-spi2.dtbo to /usr/share/rpikernelhack/overlays/enc28j60-spi2.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/enc28j60.dtbo to /usr/share/rpikernelhack/overlays/enc28j60.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/exc3000.dtbo to /usr/share/rpikernelhack/overlays/exc3000.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/fe-pi-audio.dtbo to /usr/share/rpikernelhack/overlays/fe-pi-audio.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/goodix.dtbo to /usr/share/rpikernelhack/overlays/goodix.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/googlevoicehat-soundcard.dtbo to /usr/share/rpikernelhack/overlays/googlevoicehat-soundcard.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/gpio-fan.dtbo to /usr/share/rpikernelhack/overlays/gpio-fan.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/gpio-ir-tx.dtbo to /usr/share/rpikernelhack/overlays/gpio-ir-tx.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/gpio-ir.dtbo to /usr/share/rpikernelhack/overlays/gpio-ir.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/gpio-key.dtbo to /usr/share/rpikernelhack/overlays/gpio-key.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/gpio-no-bank0-irq.dtbo to /usr/share/rpikernelhack/overlays/gpio-no-bank0-irq.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/gpio-no-irq.dtbo to /usr/share/rpikernelhack/overlays/gpio-no-irq.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/gpio-poweroff.dtbo to /usr/share/rpikernelhack/overlays/gpio-poweroff.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/gpio-shutdown.dtbo to /usr/share/rpikernelhack/overlays/gpio-shutdown.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/hd44780-lcd.dtbo to /usr/share/rpikernelhack/overlays/hd44780-lcd.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/hifiberry-amp.dtbo to /usr/share/rpikernelhack/overlays/hifiberry-amp.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/hifiberry-dac.dtbo to /usr/share/rpikernelhack/overlays/hifiberry-dac.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/hifiberry-dacplus.dtbo to /usr/share/rpikernelhack/overlays/hifiberry-dacplus.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/hifiberry-digi-pro.dtbo to /usr/share/rpikernelhack/overlays/hifiberry-digi-pro.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/hifiberry-digi.dtbo to /usr/share/rpikernelhack/overlays/hifiberry-digi.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/hy28a.dtbo to /usr/share/rpikernelhack/overlays/hy28a.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/hy28b-2017.dtbo to /usr/share/rpikernelhack/overlays/hy28b-2017.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/hy28b.dtbo to /usr/share/rpikernelhack/overlays/hy28b.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/i2c-bcm2708.dtbo to /usr/share/rpikernelhack/overlays/i2c-bcm2708.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/i2c-gpio.dtbo to /usr/share/rpikernelhack/overlays/i2c-gpio.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/i2c-mux.dtbo to /usr/share/rpikernelhack/overlays/i2c-mux.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/i2c-pwm-pca9685a.dtbo to /usr/share/rpikernelhack/overlays/i2c-pwm-pca9685a.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/i2c-rtc-gpio.dtbo to /usr/share/rpikernelhack/overlays/i2c-rtc-gpio.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/i2c-rtc.dtbo to /usr/share/rpikernelhack/overlays/i2c-rtc.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/i2c-sensor.dtbo to /usr/share/rpikernelhack/overlays/i2c-sensor.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/i2c0-bcm2708.dtbo to /usr/share/rpikernelhack/overlays/i2c0-bcm2708.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/i2c1-bcm2708.dtbo to /usr/share/rpikernelhack/overlays/i2c1-bcm2708.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/i2s-gpio28-31.dtbo to /usr/share/rpikernelhack/overlays/i2s-gpio28-31.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/iqaudio-dac.dtbo to /usr/share/rpikernelhack/overlays/iqaudio-dac.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/iqaudio-dacplus.dtbo to /usr/share/rpikernelhack/overlays/iqaudio-dacplus.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/iqaudio-digi-wm8804-audio.dtbo to /usr/share/rpikernelhack/overlays/iqaudio-digi-wm8804-audio.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/jedec-spi-nor.dtbo to /usr/share/rpikernelhack/overlays/jedec-spi-nor.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/justboom-dac.dtbo to /usr/share/rpikernelhack/overlays/justboom-dac.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/justboom-digi.dtbo to /usr/share/rpikernelhack/overlays/justboom-digi.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/lirc-rpi.dtbo to /usr/share/rpikernelhack/overlays/lirc-rpi.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/ltc294x.dtbo to /usr/share/rpikernelhack/overlays/ltc294x.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/mbed-dac.dtbo to /usr/share/rpikernelhack/overlays/mbed-dac.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/mcp23017.dtbo to /usr/share/rpikernelhack/overlays/mcp23017.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/mcp23s17.dtbo to /usr/share/rpikernelhack/overlays/mcp23s17.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/mcp2515-can0.dtbo to /usr/share/rpikernelhack/overlays/mcp2515-can0.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/mcp2515-can1.dtbo to /usr/share/rpikernelhack/overlays/mcp2515-can1.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/mcp3008.dtbo to /usr/share/rpikernelhack/overlays/mcp3008.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/mcp3202.dtbo to /usr/share/rpikernelhack/overlays/mcp3202.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/mcp342x.dtbo to /usr/share/rpikernelhack/overlays/mcp342x.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/media-center.dtbo to /usr/share/rpikernelhack/overlays/media-center.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/midi-uart0.dtbo to /usr/share/rpikernelhack/overlays/midi-uart0.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/midi-uart1.dtbo to /usr/share/rpikernelhack/overlays/midi-uart1.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/mmc.dtbo to /usr/share/rpikernelhack/overlays/mmc.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/mpu6050.dtbo to /usr/share/rpikernelhack/overlays/mpu6050.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/mz61581.dtbo to /usr/share/rpikernelhack/overlays/mz61581.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/ov5647.dtbo to /usr/share/rpikernelhack/overlays/ov5647.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/papirus.dtbo to /usr/share/rpikernelhack/overlays/papirus.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/pi3-act-led.dtbo to /usr/share/rpikernelhack/overlays/pi3-act-led.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/pi3-disable-bt.dtbo to /usr/share/rpikernelhack/overlays/pi3-disable-bt.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/pi3-disable-wifi.dtbo to /usr/share/rpikernelhack/overlays/pi3-disable-wifi.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/pi3-miniuart-bt.dtbo to /usr/share/rpikernelhack/overlays/pi3-miniuart-bt.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/pibell.dtbo to /usr/share/rpikernelhack/overlays/pibell.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/piscreen.dtbo to /usr/share/rpikernelhack/overlays/piscreen.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/piscreen2r.dtbo to /usr/share/rpikernelhack/overlays/piscreen2r.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/pisound.dtbo to /usr/share/rpikernelhack/overlays/pisound.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/pitft22.dtbo to /usr/share/rpikernelhack/overlays/pitft22.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/pitft28-capacitive.dtbo to /usr/share/rpikernelhack/overlays/pitft28-capacitive.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/pitft28-resistive.dtbo to /usr/share/rpikernelhack/overlays/pitft28-resistive.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/pitft35-resistive.dtbo to /usr/share/rpikernelhack/overlays/pitft35-resistive.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/pps-gpio.dtbo to /usr/share/rpikernelhack/overlays/pps-gpio.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/pwm-2chan.dtbo to /usr/share/rpikernelhack/overlays/pwm-2chan.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/pwm-ir-tx.dtbo to /usr/share/rpikernelhack/overlays/pwm-ir-tx.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/pwm.dtbo to /usr/share/rpikernelhack/overlays/pwm.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/qca7000.dtbo to /usr/share/rpikernelhack/overlays/qca7000.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/rotary-encoder.dtbo to /usr/share/rpikernelhack/overlays/rotary-encoder.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/rpi-backlight.dtbo to /usr/share/rpikernelhack/overlays/rpi-backlight.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/rpi-cirrus-wm5102.dtbo to /usr/share/rpikernelhack/overlays/rpi-cirrus-wm5102.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/rpi-dac.dtbo to /usr/share/rpikernelhack/overlays/rpi-dac.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/rpi-display.dtbo to /usr/share/rpikernelhack/overlays/rpi-display.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/rpi-ft5406.dtbo to /usr/share/rpikernelhack/overlays/rpi-ft5406.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/rpi-poe.dtbo to /usr/share/rpikernelhack/overlays/rpi-poe.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/rpi-proto.dtbo to /usr/share/rpikernelhack/overlays/rpi-proto.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/rpi-sense.dtbo to /usr/share/rpikernelhack/overlays/rpi-sense.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/rpi-tv.dtbo to /usr/share/rpikernelhack/overlays/rpi-tv.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/rra-digidac1-wm8741-audio.dtbo to /usr/share/rpikernelhack/overlays/rra-digidac1-wm8741-audio.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/sc16is750-i2c.dtbo to /usr/share/rpikernelhack/overlays/sc16is750-i2c.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/sc16is752-i2c.dtbo to /usr/share/rpikernelhack/overlays/sc16is752-i2c.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/sc16is752-spi1.dtbo to /usr/share/rpikernelhack/overlays/sc16is752-spi1.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/sdhost.dtbo to /usr/share/rpikernelhack/overlays/sdhost.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/sdio-1bit.dtbo to /usr/share/rpikernelhack/overlays/sdio-1bit.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/sdio.dtbo to /usr/share/rpikernelhack/overlays/sdio.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/sdtweak.dtbo to /usr/share/rpikernelhack/overlays/sdtweak.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/smi-dev.dtbo to /usr/share/rpikernelhack/overlays/smi-dev.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/smi-nand.dtbo to /usr/share/rpikernelhack/overlays/smi-nand.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/smi.dtbo to /usr/share/rpikernelhack/overlays/smi.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/spi-gpio35-39.dtbo to /usr/share/rpikernelhack/overlays/spi-gpio35-39.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/spi-rtc.dtbo to /usr/share/rpikernelhack/overlays/spi-rtc.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/spi0-cs.dtbo to /usr/share/rpikernelhack/overlays/spi0-cs.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/spi0-hw-cs.dtbo to /usr/share/rpikernelhack/overlays/spi0-hw-cs.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/spi1-1cs.dtbo to /usr/share/rpikernelhack/overlays/spi1-1cs.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/spi1-2cs.dtbo to /usr/share/rpikernelhack/overlays/spi1-2cs.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/spi1-3cs.dtbo to /usr/share/rpikernelhack/overlays/spi1-3cs.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/spi2-1cs.dtbo to /usr/share/rpikernelhack/overlays/spi2-1cs.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/spi2-2cs.dtbo to /usr/share/rpikernelhack/overlays/spi2-2cs.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/spi2-3cs.dtbo to /usr/share/rpikernelhack/overlays/spi2-3cs.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/ssd1306.dtbo to /usr/share/rpikernelhack/overlays/ssd1306.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/superaudioboard.dtbo to /usr/share/rpikernelhack/overlays/superaudioboard.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/sx150x.dtbo to /usr/share/rpikernelhack/overlays/sx150x.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/tc358743-audio.dtbo to /usr/share/rpikernelhack/overlays/tc358743-audio.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/tc358743.dtbo to /usr/share/rpikernelhack/overlays/tc358743.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/tinylcd35.dtbo to /usr/share/rpikernelhack/overlays/tinylcd35.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/tpm-slb9670.dtbo to /usr/share/rpikernelhack/overlays/tpm-slb9670.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/uart0.dtbo to /usr/share/rpikernelhack/overlays/uart0.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/uart1.dtbo to /usr/share/rpikernelhack/overlays/uart1.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/upstream-aux-interrupt.dtbo to /usr/share/rpikernelhack/overlays/upstream-aux-interrupt.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/upstream.dtbo to /usr/share/rpikernelhack/overlays/upstream.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/vc4-fkms-v3d.dtbo to /usr/share/rpikernelhack/overlays/vc4-fkms-v3d.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/vc4-kms-kippah-7inch.dtbo to /usr/share/rpikernelhack/overlays/vc4-kms-kippah-7inch.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/vc4-kms-v3d.dtbo to /usr/share/rpikernelhack/overlays/vc4-kms-v3d.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/vga666.dtbo to /usr/share/rpikernelhack/overlays/vga666.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/w1-gpio-pullup.dtbo to /usr/share/rpikernelhack/overlays/w1-gpio-pullup.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/w1-gpio.dtbo to /usr/share/rpikernelhack/overlays/w1-gpio.dtbo by rpikernelhack'
Adding 'diversion of /boot/overlays/wittypi.dtbo to /usr/share/rpikernelhack/overlays/wittypi.dtbo by rpikernelhack'
Unpacking raspberrypi-kernel (1.20190401-1) over (1.20190215-1) ...
run-parts: executing /etc/kernel/postrm.d/initramfs-tools 4.14.98+ /boot/kernel.img
run-parts: executing /etc/kernel/postrm.d/initramfs-tools 4.14.98-v7+ /boot/kernel7.img
Preparing to unpack .../07-libraspberrypi0_1.20190401-1_armhf.deb ...
Unpacking libraspberrypi0 (1.20190401-1) over (1.20190215-1) ...
Preparing to unpack .../08-raspberrypi-bootloader_1.20190401-1_armhf.deb ...
Adding 'diversion of /boot/start.elf to /usr/share/rpikernelhack/start.elf by rpikernelhack'
Adding 'diversion of /boot/start_cd.elf to /usr/share/rpikernelhack/start_cd.elf by rpikernelhack'
Adding 'diversion of /boot/start_db.elf to /usr/share/rpikernelhack/start_db.elf by rpikernelhack'
Adding 'diversion of /boot/start_x.elf to /usr/share/rpikernelhack/start_x.elf by rpikernelhack'
Adding 'diversion of /boot/fixup.dat to /usr/share/rpikernelhack/fixup.dat by rpikernelhack'
Adding 'diversion of /boot/fixup_cd.dat to /usr/share/rpikernelhack/fixup_cd.dat by rpikernelhack'
Adding 'diversion of /boot/fixup_db.dat to /usr/share/rpikernelhack/fixup_db.dat by rpikernelhack'
Adding 'diversion of /boot/fixup_x.dat to /usr/share/rpikernelhack/fixup_x.dat by rpikernelhack'
Adding 'diversion of /boot/bootcode.bin to /usr/share/rpikernelhack/bootcode.bin by rpikernelhack'
Adding 'diversion of /boot/LICENCE.broadcom to /usr/share/rpikernelhack/LICENCE.broadcom by rpikernelhack'
Unpacking raspberrypi-bootloader (1.20190401-1) over (1.20190215-1) ...
Preparing to unpack .../09-chromium-browser_72.0.3626.121-0+rpt4_armhf.deb ...
Unpacking chromium-browser (72.0.3626.121-0+rpt4) over (65.0.3325.181-0+rpt4) ...
Preparing to unpack .../10-chromium-codecs-ffmpeg-extra_72.0.3626.121-0+rpt4_armhf.deb ...
Unpacking chromium-codecs-ffmpeg-extra (72.0.3626.121-0+rpt4) over (65.0.3325.181-0+rpt4) ...
Preparing to unpack .../11-libwbclient0_2%3a4.5.16+dfsg-1+deb9u1_armhf.deb ...
Unpacking libwbclient0:armhf (2:4.5.16+dfsg-1+deb9u1) over (2:4.5.16+dfsg-1) ...
Preparing to unpack .../12-rpi-chromium-mods_20190409_armhf.deb ...
Unpacking rpi-chromium-mods (20190409) over (20190312) ...
Preparing to unpack .../13-samba-common_2%3a4.5.16+dfsg-1+deb9u1_all.deb ...
Unpacking samba-common (2:4.5.16+dfsg-1+deb9u1) over (2:4.5.16+dfsg-1) ...
Preparing to unpack .../14-tzdata_2019a-0+deb9u1_all.deb ...
Unpacking tzdata (2019a-0+deb9u1) over (2018i-0+deb9u1) ...
Preparing to unpack .../15-wget_1.18-5+deb9u3_armhf.deb ...
Unpacking wget (1.18-5+deb9u3) over (1.18-5+deb9u2) ...
Preparing to unpack .../16-containerd.io_1.2.5-1_armhf.deb ...
Unpacking containerd.io (1.2.5-1) over (1.2.4-1) ...
Preparing to unpack .../17-libbasicusageenvironment1_2016.11.28-1+deb9u2_armhf.deb ...
Unpacking libbasicusageenvironment1:armhf (2016.11.28-1+deb9u2) over (2016.11.28-1+deb9u1) ...
Preparing to unpack .../18-libgroupsock8_2016.11.28-1+deb9u2_armhf.deb ...
Unpacking libgroupsock8:armhf (2016.11.28-1+deb9u2) over (2016.11.28-1+deb9u1) ...
Preparing to unpack .../19-liblivemedia57_2016.11.28-1+deb9u2_armhf.deb ...
Unpacking liblivemedia57:armhf (2016.11.28-1+deb9u2) over (2016.11.28-1+deb9u1) ...
Preparing to unpack .../20-libobt2v5_3.6.1-4+rpi10_armhf.deb ...
Unpacking libobt2v5 (3.6.1-4+rpi10) over (3.6.1-4+rpi9) ...
Preparing to unpack .../21-libobrender32v5_3.6.1-4+rpi10_armhf.deb ...
Unpacking libobrender32v5 (3.6.1-4+rpi10) over (3.6.1-4+rpi9) ...
Preparing to unpack .../22-libusageenvironment3_2016.11.28-1+deb9u2_armhf.deb ...
Unpacking libusageenvironment3:armhf (2016.11.28-1+deb9u2) over (2016.11.28-1+deb9u1) ...
Preparing to unpack .../23-rc-gui_1.23_armhf.deb ...
Unpacking rc-gui (1.23) over (1.22) ...
Preparing to unpack .../24-lxplug-network_0.14_armhf.deb ...
Unpacking lxplug-network (0.14) over (0.13) ...
Preparing to unpack .../25-openbox_3.6.1-4+rpi10_armhf.deb ...
Unpacking openbox (3.6.1-4+rpi10) over (3.6.1-4+rpi9) ...
Preparing to unpack .../26-python-six_1.12.0-0+rpt1_all.deb ...
Unpacking python-six (1.12.0-0+rpt1) over (1.10.0-3) ...
Preparing to unpack .../27-python3-six_1.12.0-0+rpt1_all.deb ...
2019-04-11 15:47:14 py2deb.hooks[2214] INFO Cleaned up 1 Python bytecode file(s) for python3-six package.
Unpacking python3-six (1.12.0-0+rpt1) over (1.12.0) ...
Preparing to unpack .../28-raspi-gpio_0.20190304_armhf.deb ...
Unpacking raspi-gpio (0.20190304) over (0.20171123) ...
Preparing to unpack .../29-rpd-plym-splash_0.18_armhf.deb ...
Unpacking rpd-plym-splash (0.18) over (0.17) ...
Setting up libwbclient0:armhf (2:4.5.16+dfsg-1+deb9u1) ...
Setting up libusageenvironment3:armhf (2016.11.28-1+deb9u2) ...
Setting up containerd.io (1.2.5-1) ...
Processing triggers for mime-support (3.60) ...
Processing triggers for desktop-file-utils (0.23-1) ...
Processing triggers for install-info (6.3.0.dfsg.1-1+b1) ...
Setting up libobt2v5 (3.6.1-4+rpi10) ...
Setting up libbasicusageenvironment1:armhf (2016.11.28-1+deb9u2) ...
Setting up tzdata (2019a-0+deb9u1) ...

Current default time zone: 'Asia/Kolkata'
Local time is now:      Thu Apr 11 15:47:22 IST 2019.
Universal Time is now:  Thu Apr 11 10:17:22 UTC 2019.
Run 'dpkg-reconfigure tzdata' if you wish to change it.

Setting up libasound2-data (1.1.3-5+rpt4) ...
Setting up liblivemedia57:armhf (2016.11.28-1+deb9u2) ...
Setting up python3-six (1.12.0-0+rpt1) ...
Setting up raspberrypi-bootloader (1.20190401-1) ...
Removing 'diversion of /boot/start.elf to /usr/share/rpikernelhack/start.elf by rpikernelhack'
Removing 'diversion of /boot/start_cd.elf to /usr/share/rpikernelhack/start_cd.elf by rpikernelhack'
Removing 'diversion of /boot/start_db.elf to /usr/share/rpikernelhack/start_db.elf by rpikernelhack'
Removing 'diversion of /boot/start_x.elf to /usr/share/rpikernelhack/start_x.elf by rpikernelhack'
Removing 'diversion of /boot/fixup.dat to /usr/share/rpikernelhack/fixup.dat by rpikernelhack'
Removing 'diversion of /boot/fixup_cd.dat to /usr/share/rpikernelhack/fixup_cd.dat by rpikernelhack'
Removing 'diversion of /boot/fixup_db.dat to /usr/share/rpikernelhack/fixup_db.dat by rpikernelhack'
Removing 'diversion of /boot/fixup_x.dat to /usr/share/rpikernelhack/fixup_x.dat by rpikernelhack'
Removing 'diversion of /boot/bootcode.bin to /usr/share/rpikernelhack/bootcode.bin by rpikernelhack'
Removing 'diversion of /boot/LICENCE.broadcom to /usr/share/rpikernelhack/LICENCE.broadcom by rpikernelhack'
Setting up samba-common (2:4.5.16+dfsg-1+deb9u1) ...
Setting up libasound2:armhf (1.1.3-5+rpt4) ...
Setting up libobrender32v5 (3.6.1-4+rpi10) ...
Setting up python-six (1.12.0-0+rpt1) ...
Processing triggers for libc-bin (2.24-11+deb9u4) ...
Setting up udev (232-25+deb9u11) ...
addgroup: The group `input' already exists as a system group. Exiting.
update-initramfs: deferring update (trigger activated)
Setting up raspberrypi-kernel (1.20190401-1) ...
Removing 'diversion of /boot/bcm2708-rpi-0-w.dtb to /usr/share/rpikernelhack/bcm2708-rpi-0-w.dtb by rpikernelhack'
Removing 'diversion of /boot/bcm2708-rpi-b-plus.dtb to /usr/share/rpikernelhack/bcm2708-rpi-b-plus.dtb by rpikernelhack'
Removing 'diversion of /boot/bcm2708-rpi-b.dtb to /usr/share/rpikernelhack/bcm2708-rpi-b.dtb by rpikernelhack'
Removing 'diversion of /boot/bcm2708-rpi-cm.dtb to /usr/share/rpikernelhack/bcm2708-rpi-cm.dtb by rpikernelhack'
Removing 'diversion of /boot/bcm2709-rpi-2-b.dtb to /usr/share/rpikernelhack/bcm2709-rpi-2-b.dtb by rpikernelhack'
Removing 'diversion of /boot/bcm2710-rpi-3-b-plus.dtb to /usr/share/rpikernelhack/bcm2710-rpi-3-b-plus.dtb by rpikernelhack'
Removing 'diversion of /boot/bcm2710-rpi-3-b.dtb to /usr/share/rpikernelhack/bcm2710-rpi-3-b.dtb by rpikernelhack'
Removing 'diversion of /boot/bcm2710-rpi-cm3.dtb to /usr/share/rpikernelhack/bcm2710-rpi-cm3.dtb by rpikernelhack'
Removing 'diversion of /boot/kernel.img to /usr/share/rpikernelhack/kernel.img by rpikernelhack'
Removing 'diversion of /boot/kernel7.img to /usr/share/rpikernelhack/kernel7.img by rpikernelhack'
Removing 'diversion of /boot/COPYING.linux to /usr/share/rpikernelhack/COPYING.linux by rpikernelhack'
Removing 'diversion of /boot/overlays/3dlab-nano-player.dtbo to /usr/share/rpikernelhack/overlays/3dlab-nano-player.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/README to /usr/share/rpikernelhack/overlays/README by rpikernelhack'
Removing 'diversion of /boot/overlays/adau1977-adc.dtbo to /usr/share/rpikernelhack/overlays/adau1977-adc.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/adau7002-simple.dtbo to /usr/share/rpikernelhack/overlays/adau7002-simple.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/ads1015.dtbo to /usr/share/rpikernelhack/overlays/ads1015.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/ads1115.dtbo to /usr/share/rpikernelhack/overlays/ads1115.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/ads7846.dtbo to /usr/share/rpikernelhack/overlays/ads7846.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/adv7282m.dtbo to /usr/share/rpikernelhack/overlays/adv7282m.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/adv728x-m.dtbo to /usr/share/rpikernelhack/overlays/adv728x-m.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/akkordion-iqdacplus.dtbo to /usr/share/rpikernelhack/overlays/akkordion-iqdacplus.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/allo-boss-dac-pcm512x-audio.dtbo to /usr/share/rpikernelhack/overlays/allo-boss-dac-pcm512x-audio.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/allo-digione.dtbo to /usr/share/rpikernelhack/overlays/allo-digione.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/allo-katana-dac-audio.dtbo to /usr/share/rpikernelhack/overlays/allo-katana-dac-audio.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/allo-piano-dac-pcm512x-audio.dtbo to /usr/share/rpikernelhack/overlays/allo-piano-dac-pcm512x-audio.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/allo-piano-dac-plus-pcm512x-audio.dtbo to /usr/share/rpikernelhack/overlays/allo-piano-dac-plus-pcm512x-audio.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/applepi-dac.dtbo to /usr/share/rpikernelhack/overlays/applepi-dac.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/at86rf233.dtbo to /usr/share/rpikernelhack/overlays/at86rf233.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/audioinjector-addons.dtbo to /usr/share/rpikernelhack/overlays/audioinjector-addons.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/audioinjector-ultra.dtbo to /usr/share/rpikernelhack/overlays/audioinjector-ultra.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/audioinjector-wm8731-audio.dtbo to /usr/share/rpikernelhack/overlays/audioinjector-wm8731-audio.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/audiosense-pi.dtbo to /usr/share/rpikernelhack/overlays/audiosense-pi.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/audremap.dtbo to /usr/share/rpikernelhack/overlays/audremap.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/balena-fin.dtbo to /usr/share/rpikernelhack/overlays/balena-fin.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/bmp085_i2c-sensor.dtbo to /usr/share/rpikernelhack/overlays/bmp085_i2c-sensor.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/dht11.dtbo to /usr/share/rpikernelhack/overlays/dht11.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/dionaudio-loco-v2.dtbo to /usr/share/rpikernelhack/overlays/dionaudio-loco-v2.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/dionaudio-loco.dtbo to /usr/share/rpikernelhack/overlays/dionaudio-loco.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/dpi18.dtbo to /usr/share/rpikernelhack/overlays/dpi18.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/dpi24.dtbo to /usr/share/rpikernelhack/overlays/dpi24.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/dwc-otg.dtbo to /usr/share/rpikernelhack/overlays/dwc-otg.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/dwc2.dtbo to /usr/share/rpikernelhack/overlays/dwc2.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/enc28j60-spi2.dtbo to /usr/share/rpikernelhack/overlays/enc28j60-spi2.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/enc28j60.dtbo to /usr/share/rpikernelhack/overlays/enc28j60.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/exc3000.dtbo to /usr/share/rpikernelhack/overlays/exc3000.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/fe-pi-audio.dtbo to /usr/share/rpikernelhack/overlays/fe-pi-audio.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/goodix.dtbo to /usr/share/rpikernelhack/overlays/goodix.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/googlevoicehat-soundcard.dtbo to /usr/share/rpikernelhack/overlays/googlevoicehat-soundcard.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/gpio-fan.dtbo to /usr/share/rpikernelhack/overlays/gpio-fan.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/gpio-ir-tx.dtbo to /usr/share/rpikernelhack/overlays/gpio-ir-tx.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/gpio-ir.dtbo to /usr/share/rpikernelhack/overlays/gpio-ir.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/gpio-key.dtbo to /usr/share/rpikernelhack/overlays/gpio-key.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/gpio-no-bank0-irq.dtbo to /usr/share/rpikernelhack/overlays/gpio-no-bank0-irq.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/gpio-no-irq.dtbo to /usr/share/rpikernelhack/overlays/gpio-no-irq.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/gpio-poweroff.dtbo to /usr/share/rpikernelhack/overlays/gpio-poweroff.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/gpio-shutdown.dtbo to /usr/share/rpikernelhack/overlays/gpio-shutdown.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/hd44780-lcd.dtbo to /usr/share/rpikernelhack/overlays/hd44780-lcd.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/hifiberry-amp.dtbo to /usr/share/rpikernelhack/overlays/hifiberry-amp.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/hifiberry-dac.dtbo to /usr/share/rpikernelhack/overlays/hifiberry-dac.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/hifiberry-dacplus.dtbo to /usr/share/rpikernelhack/overlays/hifiberry-dacplus.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/hifiberry-digi-pro.dtbo to /usr/share/rpikernelhack/overlays/hifiberry-digi-pro.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/hifiberry-digi.dtbo to /usr/share/rpikernelhack/overlays/hifiberry-digi.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/hy28a.dtbo to /usr/share/rpikernelhack/overlays/hy28a.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/hy28b-2017.dtbo to /usr/share/rpikernelhack/overlays/hy28b-2017.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/hy28b.dtbo to /usr/share/rpikernelhack/overlays/hy28b.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/i2c-bcm2708.dtbo to /usr/share/rpikernelhack/overlays/i2c-bcm2708.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/i2c-gpio.dtbo to /usr/share/rpikernelhack/overlays/i2c-gpio.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/i2c-mux.dtbo to /usr/share/rpikernelhack/overlays/i2c-mux.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/i2c-pwm-pca9685a.dtbo to /usr/share/rpikernelhack/overlays/i2c-pwm-pca9685a.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/i2c-rtc-gpio.dtbo to /usr/share/rpikernelhack/overlays/i2c-rtc-gpio.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/i2c-rtc.dtbo to /usr/share/rpikernelhack/overlays/i2c-rtc.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/i2c-sensor.dtbo to /usr/share/rpikernelhack/overlays/i2c-sensor.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/i2c0-bcm2708.dtbo to /usr/share/rpikernelhack/overlays/i2c0-bcm2708.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/i2c1-bcm2708.dtbo to /usr/share/rpikernelhack/overlays/i2c1-bcm2708.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/i2s-gpio28-31.dtbo to /usr/share/rpikernelhack/overlays/i2s-gpio28-31.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/iqaudio-dac.dtbo to /usr/share/rpikernelhack/overlays/iqaudio-dac.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/iqaudio-dacplus.dtbo to /usr/share/rpikernelhack/overlays/iqaudio-dacplus.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/iqaudio-digi-wm8804-audio.dtbo to /usr/share/rpikernelhack/overlays/iqaudio-digi-wm8804-audio.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/jedec-spi-nor.dtbo to /usr/share/rpikernelhack/overlays/jedec-spi-nor.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/justboom-dac.dtbo to /usr/share/rpikernelhack/overlays/justboom-dac.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/justboom-digi.dtbo to /usr/share/rpikernelhack/overlays/justboom-digi.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/lirc-rpi.dtbo to /usr/share/rpikernelhack/overlays/lirc-rpi.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/ltc294x.dtbo to /usr/share/rpikernelhack/overlays/ltc294x.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/mbed-dac.dtbo to /usr/share/rpikernelhack/overlays/mbed-dac.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/mcp23017.dtbo to /usr/share/rpikernelhack/overlays/mcp23017.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/mcp23s17.dtbo to /usr/share/rpikernelhack/overlays/mcp23s17.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/mcp2515-can0.dtbo to /usr/share/rpikernelhack/overlays/mcp2515-can0.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/mcp2515-can1.dtbo to /usr/share/rpikernelhack/overlays/mcp2515-can1.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/mcp3008.dtbo to /usr/share/rpikernelhack/overlays/mcp3008.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/mcp3202.dtbo to /usr/share/rpikernelhack/overlays/mcp3202.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/mcp342x.dtbo to /usr/share/rpikernelhack/overlays/mcp342x.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/media-center.dtbo to /usr/share/rpikernelhack/overlays/media-center.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/midi-uart0.dtbo to /usr/share/rpikernelhack/overlays/midi-uart0.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/midi-uart1.dtbo to /usr/share/rpikernelhack/overlays/midi-uart1.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/mmc.dtbo to /usr/share/rpikernelhack/overlays/mmc.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/mpu6050.dtbo to /usr/share/rpikernelhack/overlays/mpu6050.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/mz61581.dtbo to /usr/share/rpikernelhack/overlays/mz61581.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/ov5647.dtbo to /usr/share/rpikernelhack/overlays/ov5647.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/papirus.dtbo to /usr/share/rpikernelhack/overlays/papirus.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/pi3-act-led.dtbo to /usr/share/rpikernelhack/overlays/pi3-act-led.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/pi3-disable-bt.dtbo to /usr/share/rpikernelhack/overlays/pi3-disable-bt.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/pi3-disable-wifi.dtbo to /usr/share/rpikernelhack/overlays/pi3-disable-wifi.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/pi3-miniuart-bt.dtbo to /usr/share/rpikernelhack/overlays/pi3-miniuart-bt.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/pibell.dtbo to /usr/share/rpikernelhack/overlays/pibell.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/piscreen.dtbo to /usr/share/rpikernelhack/overlays/piscreen.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/piscreen2r.dtbo to /usr/share/rpikernelhack/overlays/piscreen2r.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/pisound.dtbo to /usr/share/rpikernelhack/overlays/pisound.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/pitft22.dtbo to /usr/share/rpikernelhack/overlays/pitft22.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/pitft28-capacitive.dtbo to /usr/share/rpikernelhack/overlays/pitft28-capacitive.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/pitft28-resistive.dtbo to /usr/share/rpikernelhack/overlays/pitft28-resistive.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/pitft35-resistive.dtbo to /usr/share/rpikernelhack/overlays/pitft35-resistive.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/pps-gpio.dtbo to /usr/share/rpikernelhack/overlays/pps-gpio.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/pwm-2chan.dtbo to /usr/share/rpikernelhack/overlays/pwm-2chan.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/pwm-ir-tx.dtbo to /usr/share/rpikernelhack/overlays/pwm-ir-tx.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/pwm.dtbo to /usr/share/rpikernelhack/overlays/pwm.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/qca7000.dtbo to /usr/share/rpikernelhack/overlays/qca7000.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/rotary-encoder.dtbo to /usr/share/rpikernelhack/overlays/rotary-encoder.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/rpi-backlight.dtbo to /usr/share/rpikernelhack/overlays/rpi-backlight.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/rpi-cirrus-wm5102.dtbo to /usr/share/rpikernelhack/overlays/rpi-cirrus-wm5102.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/rpi-dac.dtbo to /usr/share/rpikernelhack/overlays/rpi-dac.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/rpi-display.dtbo to /usr/share/rpikernelhack/overlays/rpi-display.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/rpi-ft5406.dtbo to /usr/share/rpikernelhack/overlays/rpi-ft5406.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/rpi-poe.dtbo to /usr/share/rpikernelhack/overlays/rpi-poe.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/rpi-proto.dtbo to /usr/share/rpikernelhack/overlays/rpi-proto.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/rpi-sense.dtbo to /usr/share/rpikernelhack/overlays/rpi-sense.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/rpi-tv.dtbo to /usr/share/rpikernelhack/overlays/rpi-tv.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/rra-digidac1-wm8741-audio.dtbo to /usr/share/rpikernelhack/overlays/rra-digidac1-wm8741-audio.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/sc16is750-i2c.dtbo to /usr/share/rpikernelhack/overlays/sc16is750-i2c.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/sc16is752-i2c.dtbo to /usr/share/rpikernelhack/overlays/sc16is752-i2c.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/sc16is752-spi1.dtbo to /usr/share/rpikernelhack/overlays/sc16is752-spi1.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/sdhost.dtbo to /usr/share/rpikernelhack/overlays/sdhost.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/sdio-1bit.dtbo to /usr/share/rpikernelhack/overlays/sdio-1bit.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/sdio.dtbo to /usr/share/rpikernelhack/overlays/sdio.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/sdtweak.dtbo to /usr/share/rpikernelhack/overlays/sdtweak.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/smi-dev.dtbo to /usr/share/rpikernelhack/overlays/smi-dev.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/smi-nand.dtbo to /usr/share/rpikernelhack/overlays/smi-nand.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/smi.dtbo to /usr/share/rpikernelhack/overlays/smi.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/spi-gpio35-39.dtbo to /usr/share/rpikernelhack/overlays/spi-gpio35-39.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/spi-rtc.dtbo to /usr/share/rpikernelhack/overlays/spi-rtc.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/spi0-cs.dtbo to /usr/share/rpikernelhack/overlays/spi0-cs.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/spi0-hw-cs.dtbo to /usr/share/rpikernelhack/overlays/spi0-hw-cs.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/spi1-1cs.dtbo to /usr/share/rpikernelhack/overlays/spi1-1cs.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/spi1-2cs.dtbo to /usr/share/rpikernelhack/overlays/spi1-2cs.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/spi1-3cs.dtbo to /usr/share/rpikernelhack/overlays/spi1-3cs.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/spi2-1cs.dtbo to /usr/share/rpikernelhack/overlays/spi2-1cs.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/spi2-2cs.dtbo to /usr/share/rpikernelhack/overlays/spi2-2cs.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/spi2-3cs.dtbo to /usr/share/rpikernelhack/overlays/spi2-3cs.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/ssd1306.dtbo to /usr/share/rpikernelhack/overlays/ssd1306.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/superaudioboard.dtbo to /usr/share/rpikernelhack/overlays/superaudioboard.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/sx150x.dtbo to /usr/share/rpikernelhack/overlays/sx150x.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/tc358743-audio.dtbo to /usr/share/rpikernelhack/overlays/tc358743-audio.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/tc358743.dtbo to /usr/share/rpikernelhack/overlays/tc358743.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/tinylcd35.dtbo to /usr/share/rpikernelhack/overlays/tinylcd35.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/tpm-slb9670.dtbo to /usr/share/rpikernelhack/overlays/tpm-slb9670.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/uart0.dtbo to /usr/share/rpikernelhack/overlays/uart0.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/uart1.dtbo to /usr/share/rpikernelhack/overlays/uart1.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/upstream-aux-interrupt.dtbo to /usr/share/rpikernelhack/overlays/upstream-aux-interrupt.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/upstream.dtbo to /usr/share/rpikernelhack/overlays/upstream.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/vc4-fkms-v3d.dtbo to /usr/share/rpikernelhack/overlays/vc4-fkms-v3d.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/vc4-kms-kippah-7inch.dtbo to /usr/share/rpikernelhack/overlays/vc4-kms-kippah-7inch.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/vc4-kms-v3d.dtbo to /usr/share/rpikernelhack/overlays/vc4-kms-v3d.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/vga666.dtbo to /usr/share/rpikernelhack/overlays/vga666.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/w1-gpio-pullup.dtbo to /usr/share/rpikernelhack/overlays/w1-gpio-pullup.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/w1-gpio.dtbo to /usr/share/rpikernelhack/overlays/w1-gpio.dtbo by rpikernelhack'
Removing 'diversion of /boot/overlays/wittypi.dtbo to /usr/share/rpikernelhack/overlays/wittypi.dtbo by rpikernelhack'
run-parts: executing /etc/kernel/postinst.d/apt-auto-removal 4.14.98+ /boot/kernel.img
run-parts: executing /etc/kernel/postinst.d/initramfs-tools 4.14.98+ /boot/kernel.img
run-parts: executing /etc/kernel/postinst.d/apt-auto-removal 4.14.98-v7+ /boot/kernel7.img
run-parts: executing /etc/kernel/postinst.d/initramfs-tools 4.14.98-v7+ /boot/kernel7.img
Setting up rpd-plym-splash (0.18) ...
update-initramfs: deferring update (trigger activated)
Setting up systemd (232-25+deb9u11) ...
addgroup: The group `systemd-journal' already exists as a system group. Exiting.
Removed /etc/systemd/system/getty.target.wants/getty@tty1.service.
Created symlink /etc/systemd/system/getty.target.wants/getty@tty1.service → /lib/systemd/system/getty@.service.
Setting up wget (1.18-5+deb9u3) ...
Setting up rc-gui (1.23) ...
Setting up libgroupsock8:armhf (2016.11.28-1+deb9u2) ...
Processing triggers for man-db (2.7.6.1-2) ...
Setting up raspi-gpio (0.20190304) ...
Processing triggers for dbus (1.10.26-0+deb9u1) ...
Processing triggers for hicolor-icon-theme (0.15-1) ...
Setting up libraspberrypi0 (1.20190401-1) ...
Setting up lxplug-network (0.14) ...
Setting up libraspberrypi-doc (1.20190401-1) ...
Setting up systemd-sysv (232-25+deb9u11) ...
Setting up libraspberrypi-dev (1.20190401-1) ...
Setting up openbox (3.6.1-4+rpi10) ...
Setting up chromium-codecs-ffmpeg-extra (72.0.3626.121-0+rpt4) ...
Setting up libraspberrypi-bin (1.20190401-1) ...
Setting up chromium-browser (72.0.3626.121-0+rpt4) ...
Setting up rpi-chromium-mods (20190409) ...
Installing new version of config file /etc/chromium-browser/customizations/00-rpi-vars ...
Setting up libpam-systemd:armhf (232-25+deb9u11) ...
Setting up chromium-browser-l10n (72.0.3626.121-0+rpt4) ...
Processing triggers for initramfs-tools (0.130) ...
Processing triggers for libc-bin (2.24-11+deb9u4) ...
[+] Freeing up space. Removing Wolfram-Engine...
Reading package lists... Done
Building dependency tree
Reading state information... Done
Package 'wolfram-engine' is not installed, so not removed
The following packages were automatically installed and are no longer required:
  coinor-libipopt1v5 libgmime-2.6-0 libgpgme11 libmumps-seq-4.10.0 liboauth0 libraw15
  wolframscript
Use 'sudo apt autoremove' to remove them.
0 upgraded, 0 newly installed, 0 to remove and 1 not upgraded.
Reading package lists... Done
Building dependency tree
Reading state information... Done
awscli is already the newest version (1.11.13-1).
build-essential is already the newest version (12.3).
python-setuptools is already the newest version (33.1.1-1).
python-pip is already the newest version (9.0.1-2+rpt2).
The following packages were automatically installed and are no longer required:
  coinor-libipopt1v5 libgmime-2.6-0 libgpgme11 libmumps-seq-4.10.0 liboauth0 libraw15
  wolframscript
Use 'sudo apt autoremove' to remove them.
0 upgraded, 0 newly installed, 0 to remove and 1 not upgraded.
Requirement already satisfied: boto3 in /usr/local/lib/python2.7/dist-packages
Requirement already satisfied: watchdog in /usr/local/lib/python2.7/dist-packages
Requirement already satisfied: simplejson in /usr/local/lib/python2.7/dist-packages
Requirement already satisfied: PiCamera in /usr/local/lib/python2.7/dist-packages
Requirement already satisfied: jmespath<1.0.0,>=0.7.1 in /usr/local/lib/python2.7/dist-packages (from boto3)
Requirement already satisfied: s3transfer<0.3.0,>=0.2.0 in /usr/local/lib/python2.7/dist-packages (from boto3)
Requirement already satisfied: botocore<1.13.0,>=1.12.129 in /usr/local/lib/python2.7/dist-packages (from boto3)
Requirement already satisfied: argh>=0.24.1 in /usr/local/lib/python2.7/dist-packages (from watchdog)
Requirement already satisfied: PyYAML>=3.10 in /usr/lib/python2.7/dist-packages (from watchdog)
Requirement already satisfied: pathtools>=0.1.1 in /usr/local/lib/python2.7/dist-packages (from watchdog)
Requirement already satisfied: futures<4.0.0,>=2.2.0; python_version == "2.6" or python_version == "2.7" in /usr/local/lib/python2.7/dist-packages (from s3transfer<0.3.0,>=0.2.0->boto3)
Requirement already satisfied: docutils>=0.10 in /usr/local/lib/python2.7/dist-packages (from botocore<1.13.0,>=1.12.129->boto3)
Requirement already satisfied: python-dateutil<3.0.0,>=2.1; python_version >= "2.7" in /usr/lib/python2.7/dist-packages (from botocore<1.13.0,>=1.12.129->boto3)
Requirement already satisfied: urllib3<1.25,>=1.20; python_version == "2.7" in /usr/local/lib/python2.7/dist-packages (from botocore<1.13.0,>=1.12.129->boto3)
[+] Configuring AWS...
AWS Access Key ID [****************OXNQ]: AKIAXXXXXXXXXO4JA
AWS Secret Access Key [****************uDlb]: 3cjE/C1yiXXXXXXXXXRJWz/C98pQ83m
Default region name [None]: us-east-1
Default output format [None]:
[+] Installing Pi motion sensor module...
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
100 10891  100 10891    0     0   8189      0  0:00:01  0:00:01 --:--:--  8189
INFO  : New pi-timolo Install
INFO  : pi-timolo Folder Created

-------------------------------------------------------------
INFO  : bash 11.2  written by Claude Pageau
        New Install from https://github.com/pageauc/pi-timolo
-------------------------------------------------------------

config.py             100%[========================>]  12.15K  --.-KB/s    in 0.003s
menubox.sh            100%[========================>]  21.45K  --.-KB/s    in 0.05s
pi-timolo.py          100%[========================>]  89.18K  --.-KB/s    in 0.1s
pi-timolo.sh          100%[========================>]   2.03K  --.-KB/s    in 0.001s
webserver.py          100%[========================>]  10.84K  --.-KB/s    in 0.01s
webserver.sh          100%[========================>]   1.33K  --.-KB/s    in 0s
watch-app.sh          100%[========================>]  13.09K  --.-KB/s    in 0.05s
shutdown.py           100%[========================>]   1.67K  --.-KB/s    in 0s
convid.sh             100%[========================>]  12.03K  --.-KB/s    in 0.008s
makevideo.sh          100%[========================>]  11.61K  --.-KB/s    in 0.006s
video.conf            100%[========================>]   2.78K  --.-KB/s    in 0s
mvleavelast.sh        100%[========================>]   1.90K  --.-KB/s    in 0s
remote-run.sh         100%[========================>]     638  --.-KB/s    in 0s
config.py.new         100%[========================>]  12.15K  --.-KB/s    in 0.006s
watch-app-new.sh      100%[========================>]  13.09K  --.-KB/s    in 0.04s
video.conf.new        100%[========================>]   2.78K  --.-KB/s    in 0s
Readme.md             100%[========================>]   7.92K  --.-KB/s    in 0.002s
media/webserver.txt   100%[========================>]   1.06K  --.-KB/s    in 0.006s
rclone-test.sh        100%[========================>]   4.32K  --.-KB/s    in 0.001s
user_motion_code.py   100%[========================>]   1.24K  --.-KB/s    in 0s
INFO  : New Install Check/Install pi-timolo/plugins    Wait ...
__init__.py               [ <=>                     ]       0  --.-KB/s    in 0s
dashcam.py            100%[========================>]   2.39K  --.-KB/s    in 0.001s
secfast.py            100%[========================>]   3.79K  --.-KB/s    in 0.001s
secQTL.py             100%[========================>]   3.17K  --.-KB/s    in 0.001s
secstill.py           100%[========================>]   3.38K  --.-KB/s    in 0.001s
secvid.py             100%[========================>]   3.47K  --.-KB/s    in 0.001s
strmvid.py            100%[========================>]   3.86K  --.-KB/s    in 0.001s
shopcam.py            100%[========================>]   2.73K  --.-KB/s    in 0s
slowmo.py             100%[========================>]   3.51K  --.-KB/s    in 0.001s
TLlong.py             100%[========================>]   4.32K  --.-KB/s    in 0.001s
TLshort.py            100%[========================>]   3.74K  --.-KB/s    in 0.001s
INFO  : New Install Check/Install pi-timolo/rclone-samples    Wait ...
Readme.md             100%[========================>]   1.13K  --.-KB/s    in 0s
rclone-master.sh      100%[========================>]   4.32K  --.-KB/s    in 0s
rclone-mo-copy-videos 100%[========================>]   4.26K  --.-KB/s    in 0.001s
rclone-mo-sync.sh     100%[========================>]   4.26K  --.-KB/s    in 0.001s
rclone-mo-sync-lockfi 100%[========================>]   4.25K  --.-KB/s    in 0.001s
rclone-mo-sync-recent 100%[========================>]   4.27K  --.-KB/s    in 0.001s
rclone-tl-copy.sh     100%[========================>]   4.26K  --.-KB/s    in 0.001s
rclone-tl-sync-recent 100%[========================>]   4.27K  --.-KB/s    in 0.001s
rclone-cleanup.sh     100%[========================>]   1.04K  --.-KB/s    in 0s
rclone v1.46
- os/arch: linux/arm
- go version: go1.11.5
INFO  : Install Latest Rclone from https://downloads.rclone.org/rclone-current-linux-arm.zip
rclone.zip            100%[========================>]   8.08M  1.39MB/s    in 8.0s
INFO  : unzip rclone.zip to folder rclone-tmp
Archive:  rclone.zip
  inflating: rclone-tmp/rclone.1
  inflating: rclone-tmp/rclone
  inflating: rclone-tmp/README.txt
  inflating: rclone-tmp/README.html
 extracting: rclone-tmp/git-log.txt
INFO  : Install files and man pages
Purging old database entries in /usr/man...
Processing manual pages under /usr/man...
Purging old database entries in /usr/share/man...
Processing manual pages under /usr/share/man...
Purging old database entries in /usr/share/man/pt...
Processing manual pages under /usr/share/man/pt...
Purging old database entries in /usr/share/man/fr...
Processing manual pages under /usr/share/man/fr...
Purging old database entries in /usr/share/man/nl...
Processing manual pages under /usr/share/man/nl...
Purging old database entries in /usr/share/man/cs...
Processing manual pages under /usr/share/man/cs...
Purging old database entries in /usr/share/man/id...
Processing manual pages under /usr/share/man/id...
Purging old database entries in /usr/share/man/gl...
Processing manual pages under /usr/share/man/gl...
Purging old database entries in /usr/share/man/zh_CN...
Processing manual pages under /usr/share/man/zh_CN...
Purging old database entries in /usr/share/man/uk...
Processing manual pages under /usr/share/man/uk...
Purging old database entries in /usr/share/man/ru...
Processing manual pages under /usr/share/man/ru...
Purging old database entries in /usr/share/man/sv...
Processing manual pages under /usr/share/man/sv...
Purging old database entries in /usr/share/man/da...
Processing manual pages under /usr/share/man/da...
Purging old database entries in /usr/share/man/pt_BR...
Processing manual pages under /usr/share/man/pt_BR...
Purging old database entries in /usr/share/man/it...
Processing manual pages under /usr/share/man/it...
Purging old database entries in /usr/share/man/pl...
Processing manual pages under /usr/share/man/pl...
Purging old database entries in /usr/share/man/es...
Processing manual pages under /usr/share/man/es...
Purging old database entries in /usr/share/man/tr...
Processing manual pages under /usr/share/man/tr...
Purging old database entries in /usr/share/man/de...
Processing manual pages under /usr/share/man/de...
Purging old database entries in /usr/share/man/fr.UTF-8...
Processing manual pages under /usr/share/man/fr.UTF-8...
Purging old database entries in /usr/share/man/ko...
Processing manual pages under /usr/share/man/ko...
Purging old database entries in /usr/share/man/fr.ISO8859-1...
Processing manual pages under /usr/share/man/fr.ISO8859-1...
Purging old database entries in /usr/share/man/zh_TW...
Processing manual pages under /usr/share/man/zh_TW...
Purging old database entries in /usr/share/man/hu...
Processing manual pages under /usr/share/man/hu...
Purging old database entries in /usr/share/man/sl...
Processing manual pages under /usr/share/man/sl...
Purging old database entries in /usr/share/man/ja...
Processing manual pages under /usr/share/man/ja...
Purging old database entries in /usr/share/man/fi...
Processing manual pages under /usr/share/man/fi...
Purging old database entries in /usr/local/man...
Processing manual pages under /usr/local/man...
0 man subdirectories contained newer manual pages.
0 manual pages were added.
0 stray cats were added.
0 old database entries were purged.
INFO  : Deleting rclone.zip and Folder rclone-tmp
INFO  : /usr/bin/rclone Install Complete
INFO  : New Install Install pi-timolo Dependencies Wait ...
Reading package lists...
Building dependency tree...
Reading state information...
python-picamera is already the newest version (1.13).
The following packages were automatically installed and are no longer required:
  coinor-libipopt1v5 libgmime-2.6-0 libgpgme11 libmumps-seq-4.10.0 liboauth0 libraw15
  wolframscript
Use 'sudo apt autoremove' to remove them.
0 upgraded, 0 newly installed, 0 to remove and 1 not upgraded.
Reading package lists...
Building dependency tree...
Reading state information...
python3-picamera is already the newest version (1.13).
The following packages were automatically installed and are no longer required:
  coinor-libipopt1v5 libgmime-2.6-0 libgpgme11 libmumps-seq-4.10.0 liboauth0 libraw15
  wolframscript
Use 'sudo apt autoremove' to remove them.
0 upgraded, 0 newly installed, 0 to remove and 1 not upgraded.
Reading package lists...
Building dependency tree...
Reading state information...
python-imaging is already the newest version (4.0.0-4).
The following packages were automatically installed and are no longer required:
  coinor-libipopt1v5 libgmime-2.6-0 libgpgme11 libmumps-seq-4.10.0 liboauth0 libraw15
  wolframscript
Use 'sudo apt autoremove' to remove them.
0 upgraded, 0 newly installed, 0 to remove and 1 not upgraded.
Reading package lists...
Building dependency tree...
Reading state information...
python3-pil is already the newest version (4.0.0-4).
The following packages were automatically installed and are no longer required:
  coinor-libipopt1v5 libgmime-2.6-0 libgpgme11 libmumps-seq-4.10.0 liboauth0 libraw15
  wolframscript
Use 'sudo apt autoremove' to remove them.
0 upgraded, 0 newly installed, 0 to remove and 1 not upgraded.
Reading package lists...
Building dependency tree...
Reading state information...
dos2unix is already the newest version (7.3.4-3).
The following packages were automatically installed and are no longer required:
  coinor-libipopt1v5 libgmime-2.6-0 libgpgme11 libmumps-seq-4.10.0 liboauth0 libraw15
  wolframscript
Use 'sudo apt autoremove' to remove them.
0 upgraded, 0 newly installed, 0 to remove and 1 not upgraded.
Reading package lists...
Building dependency tree...
Reading state information...
python-pyexiv2 is already the newest version (0.3.2-8+b1).
The following packages were automatically installed and are no longer required:
  coinor-libipopt1v5 libgmime-2.6-0 libgpgme11 libmumps-seq-4.10.0 liboauth0 libraw15
  wolframscript
Use 'sudo apt autoremove' to remove them.
0 upgraded, 0 newly installed, 0 to remove and 1 not upgraded.
Reading package lists...
Building dependency tree...
Reading state information...
libav-tools is already the newest version (7:3.2.12-1~deb9u1+rpt1).
The following packages were automatically installed and are no longer required:
  coinor-libipopt1v5 libgmime-2.6-0 libgpgme11 libmumps-seq-4.10.0 liboauth0 libraw15
  wolframscript
Use 'sudo apt autoremove' to remove them.
0 upgraded, 0 newly installed, 0 to remove and 1 not upgraded.
Reading package lists...
Building dependency tree...
Reading state information...
pandoc is already the newest version (1.17.2~dfsg-3).
The following packages were automatically installed and are no longer required:
  coinor-libipopt1v5 libgmime-2.6-0 libgpgme11 libmumps-seq-4.10.0 liboauth0 libraw15
  wolframscript
Use 'sudo apt autoremove' to remove them.
0 upgraded, 0 newly installed, 0 to remove and 1 not upgraded.
Reading package lists...
Building dependency tree...
Reading state information...
gpac is already the newest version (0.5.2-426-gc5ad4e4+dfsg5-3).
The following packages were automatically installed and are no longer required:
  coinor-libipopt1v5 libgmime-2.6-0 libgpgme11 libmumps-seq-4.10.0 liboauth0 libraw15
  wolframscript
Use 'sudo apt autoremove' to remove them.
0 upgraded, 0 newly installed, 0 to remove and 1 not upgraded.
Reading package lists...
Building dependency tree...
Reading state information...
fonts-freefont-ttf is already the newest version (20120503-6).
The following packages were automatically installed and are no longer required:
  coinor-libipopt1v5 libgmime-2.6-0 libgpgme11 libmumps-seq-4.10.0 liboauth0 libraw15
  wolframscript
Use 'sudo apt autoremove' to remove them.
0 upgraded, 0 newly installed, 0 to remove and 1 not upgraded.
Reading package lists...
Building dependency tree...
Reading state information...
python-opencv is already the newest version (2.4.9.1+dfsg1-2).
The following packages were automatically installed and are no longer required:
  coinor-libipopt1v5 libgmime-2.6-0 libgpgme11 libmumps-seq-4.10.0 liboauth0 libraw15
  wolframscript
Use 'sudo apt autoremove' to remove them.
0 upgraded, 0 newly installed, 0 to remove and 1 not upgraded.
Reading package lists...
Building dependency tree...
Reading state information...
python-pip is already the newest version (9.0.1-2+rpt2).
The following packages were automatically installed and are no longer required:
  coinor-libipopt1v5 libgmime-2.6-0 libgpgme11 libmumps-seq-4.10.0 liboauth0 libraw15
  wolframscript
Use 'sudo apt autoremove' to remove them.
0 upgraded, 0 newly installed, 0 to remove and 1 not upgraded.
Reading package lists...
Building dependency tree...
Reading state information...
python3-dateutil is already the newest version (2.5.3-2).
The following packages were automatically installed and are no longer required:
  coinor-libipopt1v5 libgmime-2.6-0 libgpgme11 libmumps-seq-4.10.0 liboauth0 libraw15
  wolframscript
Use 'sudo apt autoremove' to remove them.
0 upgraded, 0 newly installed, 0 to remove and 1 not upgraded.
Reading package lists...
Building dependency tree...
Reading state information...
python-dateutil is already the newest version (2.5.3-2).
The following packages were automatically installed and are no longer required:
  coinor-libipopt1v5 libgmime-2.6-0 libgpgme11 libmumps-seq-4.10.0 liboauth0 libraw15
  wolframscript
Use 'sudo apt autoremove' to remove them.
0 upgraded, 0 newly installed, 0 to remove and 1 not upgraded.
Requirement already satisfied: python-dateutil in /usr/lib/python2.7/dist-packages
INFO  : New Install Done Dependencies Install
INFO  : New Install rclone is installed at /usr/bin/rclone
rclone v1.46
- os/arch: linux/arm
- go version: go1.11.5

-----------------------------------------------
INFO  : New Install Complete ver 10.x
-----------------------------------------------
Minimal Instructions:
1 - It is suggested you run sudo apt-get update and sudo apt-get upgrade
    Reboot RPI if there are significant Raspbian system updates.
2 - If config.py already exists then latest file is config.py.new
3 - To Test Run pi-timolo execute the following commands in RPI SSH
    or terminal session. Default is Motion Track On and TimeLapse On

    cd ~/pi-timolo
    ./pi-timolo.py

4 - To manage pi-timolo, Run menubox.sh Execute commands below

    cd ~/pi-timolo
    ./menubox.sh

For Full Instructions See Wiki at https://github.com/pageauc/pi-timolo/wiki

Good Luck Claude ...
Bye
[+] Adding auto launch for pi-timolo to start on boot
[+] Adding cronjob and changing file permissions...
[+] Done!
```

### Getting started

First, you need to create a new collection on AWS Rekognition. Creating a 'home' collection would look like:

cd pi-detector/scripts<br />
python add_collection.py -n 'home'<br />

Next, add your images to the pi-detector/faces folder. The more images of a person the better results you will get for detection. I would recommend several different poses in different lighting.

cd pi-detector/faces<br />
python ../scripts/add_image.py -i 'image.jpg' -c 'home' -l 'Tom'<br />

I found the best results by taking a photo in the same area that the camera will be placed, and by using the picam. If you want to do this, I created a small python script to take a photo with a 10 second delay and then puts it into the pi-detector/faces folder. To use it:

cd pi-detector/scripts<br />
python take_selfie.py<br />

Once complete, you can go back and rename the file and repeat the steps above to add your images to AWS Rekognition. Once you create a new collection, or add a new image, two reference files will be created as a future reference. These are useful if you plan on deleting images or collections in the future.

To delete a face from your collection, use the following:

cd pi-detector/scripts<br />
python del_faces.py -i '000-000-000-000' -c 'home'<br />

If you need to find the image id or a collection name, reference your faces.txt and collections.txt files.

To remove a collection:

cd pi-detector/scripts<br />
python del_collections.py -c 'home'<br />

Note that the above will also delete all the faces you have stored in AWS. 

The last script is facematch.py. If you have images updated and just want to test static photos against the faces you have stored on AWS, do the following:

cd pi-detector/scripts<br />
python facematch.py -i 'tom.jpg' -c 'home'<br />

The results will be printed to screen, to include the percentage of similarity and confidence. 
