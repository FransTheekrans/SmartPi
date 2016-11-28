# Installation
Download Raspbian Jessie Lite from https://www.raspberrypi.org/downloads/raspbian/ and copy it on your SD card.

* Expand Filesystem
* Change user (pi) password
* Advanced Options > Enable I2C
	sudo raspi-config 

##### Update packet list and update packets

    sudo apt-get update
    sudo apt-get upgrade

##### Install additional packets. Especially, "librrd-dev" is required to build SmartPi tools.

    sudo apt-get install librrd-dev rrdtool git i2c-tools avahi-daemon

##### Test if i2c is correctly enabled:

    i2cdetect -l

The output should list your I2C channel:

    i2c-1   i2c             20804000.i2c                            I2C adapter

##### If an I2C channel is being found, you may search for connected devices:

    i2cdetect -y 1

In case of an SMartPi connected to an RPI3, the output should look like

		 0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f
	00:          -- -- -- -- -- -- -- -- -- -- -- -- --
	10: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
	20: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
	30: -- -- -- -- -- -- -- -- 38 -- -- -- -- -- -- --
	40: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
	50: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
	60: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
	70: -- -- -- -- -- -- -- --
	
##### Install go
Download the archive and extract it into /usr/local, creating a Go tree in /usr/local/go.
Currently version 1.7.3 is up to date. You may need to adapt the filenamy according to latest version.
	
    cd /usr/local
    sudo wget https://storage.googleapis.com/golang/go1.7.3.linux-armv6l.tar.gz
    sudo tar -C /usr/local -xzf go1.7.3.linux-armv6l.tar.gz
    sudo rm go1.7.3.linux-armv6l.tar.gz

Add "/usr/local/go/bin" to the PATH environment variable.
You can do this by adding this line to your /etc/profile (for a system-wide installation) before the path is being exported.

    PATH=$PATH:/usr/local/go/bin

Create a directory to contain your workspace and SmartPi git-repo, $HOME/SmartPi in this case,
and set the GOPATH environment variable to point to that location.

    $ export GOPATH=$HOME/SmartPi

Download of SmartPi sources

    cd ~
    git clone https://github.com/nDenerserve/SmartPi.git

Delete library folder of Smartpi git repository.
We will create them again.
(Caused problem on my rpi.)

    rm ~/SmartPi/src/github.com
    rm ~/SmartPi/src/golang.org
    rm ~/SmartPi/src/gopkg.in

##### Go dependencies:

    go get github.com/gorilla/mux
    go get github.com/nathan-osman/go-rpigpio
    go get github.com/ziutek/rrd
    go get gopkg.in/ini.v1
    go get github.com/secsy/goftp
    go get golang.org/x/exp/io/i2c
    go get golang.org/x/exp/io/i2c	

##### build SmartPi tools
	cd  ~/SmartPi
	make

## Change Log

### 11/28/11/16
 * Added MQTT Client
 * producecounter and consumecounter files make use of Databasedir -> co-located to rrd database
 * fixed "}" compilation issue
 * Added this readme.md
 
## ToDo's:
   * Round Robin Logging
