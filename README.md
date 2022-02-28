# How to install a vu meter pluging on Volumio using PeppyMeter

Instructions about how to install and configure the PeppyMeter plugin with Volumio on a Raspberry.

** Note: after these instructions the regular update procedure of Volumio can stop working. So, for every update, you'll have to re-configure your Volumio from scratch.**

First of all the suggestion is to start with a freshly installed Volumio, via factory reset or by installing it on a fresh card.

For a lot of activity you'll need an SSH access to the volumio, to do so go to
```
http://volumio.local/dev/
```
And enable SSH access (third button from the top), you'll get no feedback but test the connection from a Windows (or Mac or Linux) command prompt by connecting to volumio:
```
ssh volumio@volumio.local
```
The system will ask if to trust the raspberry key and then asks for a password, that is the default:
```
volumio
```

This is the expected result:
```
PS C:\Users\xxx> ssh volumio@volumio.local
The authenticity of host 'volumio.local (192.168.178.123)' can't be established.
ECDSA key fingerprint is SHA256:rGUp/DLKjdD4tV2cp0K2BRAAAWlXQeyJZHsoSK92Lbk.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'volumio.local' (ECDSA) to the list of known hosts.
volumio@volumio.local's password:
                       ___
                      /\_ \                        __
         __  __    ___\//\ \    __  __    ___ ___ /\_\    ___
        /\ \/\ \  / __`\\ \ \  /\ \/\ \ /' __` __`\/\ \  / __`\
        \ \ \_/ |/\ \L\ \\_\ \_\ \ \_\ \/\ \/\ \/\ \ \ \/\ \L\ \
         \ \___/ \ \____//\____\\ \____/\ \_\ \_\ \_\ \_\ \____/
          \/__/   \/___/ \/____/ \/___/  \/_/\/_/\/_/\/_/\/___/

             Free Audiophile Linux Music Player - Version 3.0

          © 2015-2021 Michelangelo Guarise - Volumio Team - Volumio.org

Volumio Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Welcome to Volumio for Raspberry Pi (5.10.92-v7+ armv7l)
volumio@volumio:~$
```

Some useful commands if you're not accustomed to Linux platform:

```
ls: list the content of a directory
cd: change directory, works as in a regular command
nano: the default editor
```

After installation the first step is to configure your Raspberry to manage the LCD screen. In a typical scenario you'll see a gibberish screen but this is due to the fact that you need to configure the raspberry paramaters to support the specific screen. What follows are the configuration for my screem, from a brand named waveshare, a 5 inches display. This parameter are usually found in the screen manuals or in the support web site, google is your friend.

The best way to include these information is editing the userconfig.txt file

```
sudo nano /boot/userconfig.txt
```
If the system asks for a supplemental password to enable sudo (a way to do operations a regular user is not allowed to do) simply use volumio again.

Editing with nano you can simply copy and paste the correct configuration, then use CONTROL-O to write the file and CONTROL-X to quit nano. Note that nano is not the most user friendly text editor of the world, but thanks volumio team to have it included instead of the default vi editor.

```
# Add your custom config.txt options to this file, which will be preserved during updates
#### LCD Screen
max_usb_current=1
hdmi_group=2
hdmi_mode=87
hdmi_cvt 800 480 60 6 0 0 0
hdmi_drive=1
max_framebuffer_height=720
config_hdmi_boost=10
```
After the configuration is done you can reboot your system, using the volumio web interface or via your trustly sudo commando
```
sudo reboot now
```
After rebooting the system you should see the request for the login from the volumio operating system in the LCD screen, if this is OK then the LCD screen is working, we can then move to configure Volumio to use it at its best.

The first step is to install and enable the **Touch Display** from the pluging setting page of Volumio. This will install all the base software required and, after installing and enabling it (and another reboot) the LCD should display the same web user interface you see on the web browser.

*If, at this point of the configuration, you do not have the LCD screen displaying the Volumio web UI it's useless to proceed forward.*

After that you need to install the peppymeter plugin, unluckily this has not been published in the regular Volumio plugin store (but let's hope it will be, sooner or later), so you need to install it manually using SSH.

You need to install the peppy_screensaver plugin, to do so login to Volumio via SSH and run the following commands:
```
mkdir peppy
wget https://github.com/2aCD-creator/volumio-plugins/raw/gh-pages/plugins/volumio/armhf/miscellanea/peppy_screensaver/peppy_screensaver.zip
miniunzip peppy_screensaver.zip -d ./peppy
cd peppy
volumio plugin install
```
To achieve better performance and a snappier vu meters I suggest also to edit the file named /opt/volumiokiosk.sh to change some parameters of the startup of chromium interface:

```
sudo nano /opt/volumiokiosk.sh
```

The changes in the startup screen are the following (suggestion is to comment with a # all the existing lines from "while true" to "done" and then copy this ones.

```
while true; do
  /usr/bin/chromium-browser \
    --simulate-outdated-no-au='Tue, 31 Dec 2099 23:59:59 GMT' \
    --force-device-scale-factor=1 \
    --disable-pinch \
    --kiosk \
    --no-first-run \
    --noerrdialogs \
    --disable-3d-apis \
    --disable-breakpad \
    --disable-crash-reporter \
    --disable-infobars \
    --disable-session-crashed-bubble \
    --disable-translate \
    --disable-smooth-scrolling \
    --disable-gpu-compositing \
    --enable-experimental-canvas-features \
    --enable-scroll-prediction \
    --disable-quic \
    --max-tiles-for-interest-area=512 \
    --num-raster-threads=4 \
    --enable-low-res-tiling \
    --enable-native-gpu-memory-buffers \
    --enable-gpu-rasterization \
    --enable-zero-copy \
    --use-gl=egl \
    --disk-cache-dir='/tmp' \
    --user-data-dir='/data/volumiokiosk'     http://localhost:3000
done
```

To get the better performance I suggest also to enable the **Music Service Shield** that you can find (wrongly) classified under the Music Services plugin category. This is a really nice and useful plugin that allow your Raspberry to allocate some of its cores only to music reproduction and so have the better performance for music reprodction also if Volumio is busy performing other tasks, like indexing music or... displaying vumeters!
