# myvolumio

Instructions about how to install and configure the PeppyMeter plugin with Volumio on a Raspberry.

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



After installation the first step is to configure your Raspberry to manage the LCD screen. In a typical scenario you'll see a gibberish screen but this is due to the fact that you need to configure the raspberry paramaters to support the specific screen.


- /boot/userconfig.txt

...
#** Add your custom config.txt options to this file, which will be preserved during updates
#### LCD Screen
max_usb_current=1
hdmi_group=2
hdmi_mode=87
hdmi_cvt 800 480 60 6 0 0 0
hdmi_drive=1
max_framebuffer_height=720
config_hdmi_boost=10
...
