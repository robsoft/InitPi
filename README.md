# InitPi

headless pi setup;

create an image of raspbian lite (use the raspi imager program)  
put the __wpa_supplicant.conf__ file into the root of the boot partition (no install_ prefix)  
edit it accordingly for the local wifi  
create an empty file __ssh__ in the root of the boot partition  
boot the card in a pi  

once it's had a chance to connect;  
```
ssh pi@raspberrypi  
(password = raspberry)  

sudo apt update  
sudo apt full-upgrade -y
(this may take a while)
sudo apt install -y samba
sudo apt-get install -y python3
sudo apt-get install -y python3-pip
sudo apt-get install -y git
pip3 install azure.iot.device
pip3 install board
pip3 install Rpi.GPIO  (may already be there)
sudo nano /boot/config.txt
```
change so it includes this at the end;
```
[all]
dtoverlay=w1_gpio,gpiopin=4
```
save it and return to the command line
```
sudo nano /etc/modules
```
add this in the bottom if not present
```
w1_therm
w1_gpio
```
save it and return to the command line
```
sudo raspi-config
```
set the gpu amount to 16 (from 64)  
enable ssh and 1-wire  
change the machine (host) name eg piboardd  
```
sudo reboot now
```
reconnect with ssh as above
```
lsmod | grep w1
```
(confirm __w1_gpio__, __w1_therm__ are present)
```
cd /home/pi
git clone https://github.com/robsoft/InitPi
cd InitPi
sudo chmod +777 setup_git.sh (if not already executable)
./setup_git.sh
```
edit the __first_fetch.sh__ file accordingly and run it
```
nano first_fetch.sh
./first_fetch.sh
ls
```
if PiTest has appeared, we're good to go and you can now get rid of all this;
```
sudo rm PiTest -r
cd ..
sudo rm InitPi -r
ls
```
confirm the InitPi folder has gone - now git is all set ready to fetch other private projects
