# Pwnagotchi Device Build

## WiFi Deauthenticator and Handshake Capture

### Description

The Pwnagotchi is based on a "digital pet"  from the late 1990s called "Tamagotchi". The original toy was a
handheld device that emulated a real pet's needs. In turn the virtual pet would grow and express its feelings to the user.
It needed to be fed to be happy, and that is the main similarity, as we will see.

The Pwnagotchi eats WPA handshakes, and that makes it a happy :) pet. It is built with a Raspberry Pi Zero WH running
a full Linux system utilizing Bettercap to fulfill its purpose. It scans for Wi-Fi networks, attempts to deauthenticate
associated devices, captures WPA handshakes as those devices try to reconnect, and saves all that handshake information
into .pcap files for later examination and password cracking with Wireshark, .

The entire process is automated. It doesn't require any user input. It just sniffs and eats. It only works on 2.4GHz (on the
Pi Zero W, using other hardware can pull 5GHz) but as you should know, a lot of devices use the 2.4GHz bandwidth. It also uses a reinforcement learning model with rewards, value of rewards, decisions, and action plans based on previously recorded data. This is a short comic strip to help explain https://hackernoon.com/intuitive-rl-intro-to-advantage-actor-critic-a2c-4ff545978752 .

Let's get into the build though.

### Building

Parts list:
- USB SD card adapter
- Raspberry Pi Zero WH (Wi-Fi and pre-soldered Headers)
- SD card (SanDisk 64G, overkill probably)
- Waveshare eInk 2.13" (mine is V4, it works, you can't get older versions anyway)
- PiSugar 2 1200mAh Battery (you can use a power bank also)
- Casing (3D printed - many available - or print your own)

<p align="center">Beginning top left clockwise: Screen, SD card USB adapter, Pi Zero WH, SD card, Pi Sugar2 battery 
  <br/>
  <img src="https://imgur.com/mR0FWTo.jpg" height="80%" width="80%" alt="parts"/><br /><br />
</p>

There are 3 parts to the sandwich here. The pi attaches to the battery interface and screws in place. Then the screen
plugs into the header pins. These are small and delicate pieces, but it's not too bad if you take your time and don't force things. I made sure not to press hard on the eInk screen, as it may damage its functionality. I connected the pi board and battery together first, then placed the screen face down to distribute pressure evenly while inserting the header pins into the screen interface. It all needs to fit firmly together to fit snugly inside the case.

<p align="center">Components from top, moving down: screen, pi board, battery pack.
  <br/>
  <img src="https://imgur.com/R8rlUZi.jpg" height="80%" width="80%" alt="parts_together"/><br /><br />
</p>


## Install and configuration

Now all that all the pieces are together, we can plug it in and install the image. Credit to https://github.com/jayofelony/pwnagotchi who has
kept the project active and is the source for stable releases.

- Download the image file https://github.com/jayofelony/pwnagotchi
- Write the image to the SD card (Pi imager, Balena etcher)
- Get some drivers so we can SSH into the device 
- Set the IP and mask (10.0.0.1 /24)
- SSH into the device (PuTTY, Powershell) with default credentials which we will change
- Make some changes to the configuration file found in /etc/pwnagotchi/config.toml
	- naming the device, language
	- whitelist any WAP you don't want to attack
	- enable display, type of display, color
	- too many config choices to list, see this reference https://github.com/evilsocket/pwnagotchi/blob/master/pwnagotchi/defaults.toml
	
- You can also run the device in a headless configuration
- Connection by bluetooth tethering to your phone is also possible
- Antenna modification can be accomplished by soldering directly to the board or possibly with USB connection (some report success)
	- This provides much better reception for sniffing

### Web GUI

There is a web UI at http://10.0.0.2:8080/
	- You will need to change the login : pass in the config.toml file we discussed earlier
	- This provides an easy way to control custom plugins and configuration
	
### Bettercap GUI

There is also a Bettercap UI at http://10.0.0.2:80/

### Placing inside the case
Placing the device inside of the case I chose was a tight fit. I used paper to protect the screen from scratches while sliding it into place. Also be careful with the ribbon cable, it is very fragile.

<p align="center">Assembled components inside the casing.
 <br/>
  <img src="https://imgur.com/VsxKy1m.jpg" height="80%" width="80%" alt="parts-inside-case"/><br /><br />
</p>

## Cracking methods

There are different methods to get the pcap files off the device. You can use a tool like FileZilla for a GUI to explore the device in a more user friendly way. The handshake files are found in /root/handshakes/ . You can then copy them to your PC, move them to a Kali VM if you don't already have an active Kali environment spun up. Now you can search through Wireshark and the files for more detail, which can betime consuming.

- You can also use hcxpcapngtool that will convert this information for you.
- 'hcxpcapngtool -o hash.hc22000 ./handshakes/*.pcap' this statement is explained below:
	- hcxpcap tool name
	- output command and file name in hashcat form
	- carry out action in handshakes directory
	- wildcard for any .pcap file there, regardless of name
- You are going to want a good GPU for cracking, or a cloud VM
- Password lists
	- WPA lists at https://www.weakpass.com
		- 'Super WPA' list 11GB
		- 'All-In-One-WiFi' list 134GB
		- You can also build your own custom wordlists in Kali and combine into custom wordlists
 

## Finished build
<p align="center"> Looks can be deceiving
  <br/>
  <img src="https://imgur.com/emJEgKR.jpg" height="80%" width="80%" alt="finished-build"/><br /><br />
</p>

Credit goes to all of the developers who worked on the device. This project reflects building and studying the device. This project is meant for ethical hacking purposes, education, and analysis. I do not condone using this device or knowledge in malicious manner.
