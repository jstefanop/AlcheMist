# AlcheMist


This is the Python driver modified to run on a raspberry pi, and can control an individual Alcheminer blade using the UART pins on the pi (or more if you use USB->UART dongles).



Code is maintained by jstefanop (jstefanop at mac.com)

Donations are always appreciated!

LTC: LX5vpxrQE4eLRLPobKwZhw2comkKFCh3p4 

BTC: 1N7XfuyUPSFo32o4pgzx8khJp8iGdsojqL

#Install

Follow these instructions to get your pi ready to run the driver

Update your pi

```
sudo apt-get update
sudo apt-get upgrade
```

Install python-dev and build essentials

```
sudo apt-get install python-dev
sudo apt-get install build-essential
```

Install twisted

```
wget https://pypi.python.org/packages/source/T/Twisted/Twisted-12.2.0.tar.bz2#md5=9a321b904d01efd695079f8484b37861
tar jxf Twisted-12.2.0.tar.bz2 
sudo python setup.py build
sudo python setup.py install --prefix=/usr/local
```

Install autobahn (need pip to do this)

```
sudo apt-get install python-pip
sudo pip install autobahn[twisted]
```

We now need to free up the serial port on the pi that is being used for console output, to do this download this scrypt, run it and reboot

```
sudo wget https://raw.githubusercontent.com/lurch/rpi-serial-console/master/rpi-serial-console -O /usr/bin/rpi-serial-console && sudo chmod +x /usr/bin/rpi-serial-console
sudo rpi-serial-console disable
sudo reboot
```


cd to home directory and install this repo

```
git clone -b AlcheMist_raspi https://github.com/jstefanop/AlcheMist.git
```

Install stratum libraries
```
cd Alchemist/miner/stratum-mining-proxy/
sudo python setup.py develop
```

Need to compile some libraries used by stratum proxy for rasp pi CPU

```
cd Alchemist/miner/stratum-mining-proxy/litecoin_scrypt
sudo python setup.py install
cd Alchemist/miner/stratum-mining-proxy/midstatec
make
```

Thats it! Now connect your pi to the UART port using jumper cables or create your own UART harness
UART connect on board is labeled 1-6 Pin 1 is RX, 2 TX, 4 GND, 6 Board Reset
Connect RX Pin to TX on pi, TX pin to RX, GND to GND, and reset pin to GPIO pin 23 on pi. 

To start miner run

```
cd Alchemist/miner
sudo sh go&
```
to stop
```
sudo sh shutdown
```

---
#Clock Speed
The driver is currently configured to run at 320mhz (which provides the highest MH/watt performance), and is recommended, as the cooling system for the boards is inadiquate for anything higher and will cause high amount of rejects. 

If you wish to change the clock speed there are 3 other options in the asic_miner.py file (support for more options and command line/GUI option to change clock will be added soon).

The options are commented by the values below, there are 5 places in the code you need to change to switch the clock speed, just comment the current speed block and uncomment the desired frequency
```
#288 Mhz underclock
#320 Mhz underclock  <---Driver curreently set to this
#352 Mhz Stock Clock <--not recommended
# 384 Mhz Overclock  <--not recommended
```

---
