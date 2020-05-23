# **!!! Under development /  Not working yet !!!**

# RaspiWiFi - capture

[RaspiWiFi](https://github.com/jasbur/RaspiWiFi) is a program to headlessly configure a Raspberry Pi's WiFi
connection using using any other WiFi-enabled device developed by [jasbur](https://github.com/jasbur).

The configuration using RaspiWiFi works great. However, it is missing a captive portal that allows for promting the user with the configuration page when connecting to the config wifi.  

In this fork I attempt to adding such [captive portal](https://tools.ietf.org/html/rfc7710) function to the RaspiWiFi project.  

## Script-Based Installation Instructions

1. Navigate to the directory where you downloaded or cloned RaspiWiFi
2. Run: `sudo python3 initial_setup.py`
3. The script will ask you for some settings and install all necessary prerequisites and copy all necessary config and library files, then reboot.  
When it finishes booting it should present itself in "Configuration Mode" as a WiFi access point with the name `"RaspiWiFi[xxxx] Setup"` (Or the SSID you have chosen during installation).
4. The original RaspiWiFi directory that you ran the Initial Setup is no longer
needed after installation and can be safely deleted. All necessary files are
copied to `/usr/lib/raspiwifi/` on setup.

## Configuration

You will be prompted to set a few variables during the Initial Setup script. All of these variables can be set at any time after the Initial Setup has
been run by editing the `/etc/raspiwifi/raspiwifi.conf`

- `SSID Prefix` *[default: "RaspiWiFi Setup"]*:  
  - This is the prefix of the SSID that your Pi will broadcast for you to connect to during Configuration Mode (Host Mode). The last four of you Pi's serial number will be appended to whatever you enter here.  
- `WPA Encryption` *[default: No]*:  
  - If oyu enable this setting the Access Point created during Configuration Mode will be encrypted using WPA2 encryption. The prompt following this one will let you specify the Wireless Key to be used. You can leave the password blank if you chose 'N' to this option.  
- `Auto-Config mode` *[default: n]*:  
  - If you choose to enable this mode your Pi will check for an active connection while in normal operation mode (Client Mode). If an active connection has been determined to be lost, the Pi will reboot back into Configuration Mode (Host Mode) automatically.  
- `Auto-Config delay` *[default: 300 seconds]*:  
  - This is the time in consecutive seconds to wait with an inactive connection before triggering a reset into Configuration Mode (Host Mode). This is only applicable if the "Auto-Config mode" mentioned above is set to active.  
- `Server port` *[default: 80]*:  
  - This is the server port that the web server hosting the Configuration App page will be listening on. If you change this port make sure to add it to the end of the address when you're connecting to it. For example, if you speficiy 12345 as the port number you would navigate to the page like this: `http://10.0.0.1:12345` If you leave the port at the default setting [80] there is no need to specify the port when navigating to the page.  
- `SSL Mode` *[default: n]*:  
  - With this option enabled your RaspiWifi configuration page will be sent over an SSL encrypted connection (don't forget the "s" when navigating to `https://10.0.0.1:9191` when using this mode). You will get a certificate error from your web browser when connecting. The error is just a warning that the certificate has not been verified by a third party but everything will be properly encrypted anyway.

## Usage

1. Connect to the `RaspiWiFi[xxxx] Setup` access point using any other WiFi enabled device.  
2. Navigate to `http(s)://10.0.0.1`  or any other http domain. You should be redirected to the configuration page.
3. Select the WiFi connection you'd like your Raspberry Pi to connect to from the drop down list and enter its wireless password on the page provided. If no encryption is enabled, leave the password box blank. You may also manually specify your network information by clicking on the `"manual SSID entry"` link.  
4. Click the `"Connect"` button.  
5. At this point your Raspberry Pi will reboot and connect to the access point
specified.

- You can view the current WPA encryption settings and change them from the main Web Configuration interface. The current settings are visible in a panel in the upper left corner of the screen. If you click the values in this display you will be taken to a page where you can change them. If you change them your device will reboot to enable the new configuration.  
  
- You can also use the Pi in a point-to-point connection mode by leaving it in Configuration Mode. All services will be addresible in their normal way at `http://10.0.0.1` while connected to the `"RaspiWiFi[xxxx] Setup"` AP.

## Resetting The Device

If GPIO 18 is pulled HIGH for 10 seconds or more the Raspberry Pi will reset all settings, reboot, and enter "Configuration Mode" again. It's useful to have a simple button wired on GPIO 18 to reset easily if moving to a new location, or if incorrect connection information is ever entered. Just press and hold for 10 seconds or longer.

You can also reset the device by running the `manual_reset.py` in the `/usr/lib/raspiwifi/reset_device` directory as root or with sudo.

- `sudo python /usr/lib/raspiwifi/reset_device/manual_reset.py`

## Uninstallation

You can uninstall RaspiWiFi at any time by running:

- `sudo python3 /usr/lib/raspiwifi/uninstall.py`

You can also run it from the "libs/" directory from a fresh clone if you've installed from a previous version and don't have `/usr/lib/raspiwifi/uninstall.py` available.
