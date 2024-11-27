# Pwnagotchi Device Build

## WiFi Deauthenticator and Handshake Capture

### Description

The Pwnagotchi is inspired by the "digital pet"  from the late 1990s called the "Tamagotchi". The original toy was a
handheld device that emulated a real pet's needs. In turn the virtual pet would grow and express its feelings to the user.
It needed to be fed to be happy, and that is the main similarity, as we will see.

The Pwnagotchi eats WPA handshakes, and that makes it a happy :) pet. It is built with a Raspberry Pi Zero WH running
a full Linux system utilizing Bettercap to fulfill its purpose. It scans for Wi-Fi networks, attempts to deauthenticate
associated devices, captures WPA handshakes as those devices try to reconnect, and saves all that handshake information
in `.pcap` files for later examination with Wireshark and password cracking with other tools.

The entire process carried out by the device is automated. It does not require any user input. It just sniffs and eats. It only works on 2.4GHz (on the
Pi Zero W, using other hardware can pull 5GHz) but as you should know, a lot of devices use the 2.4GHz frequency. It even employs a 
reinforcement learning model to optimize its behavior based on prior data, which is explained further [here](https://hackernoon.com/intuitive-rl-intro-to-advantage-actor-critic-a2c-4ff545978752).

### Building the Device
Parts list:
- USB SD card adapter
- Raspberry Pi Zero WH (with Wi-Fi and pre-soldered Headers)
- SD card (SanDisk 64G, overkill probably)
- Waveshare eInk 2.13" (mine is V4, it works, older versions are hard to obtain)
- PiSugar 2 1200mAh Battery (or a power bank)
- Casing (3D printed - many designs are available online - or print your own)

<p align="center">Clockwise from top left: Screen, SD card USB adapter, Pi Zero WH, SD card, Pi Sugar2 battery 
	
![parts](https://github.com/user-attachments/assets/b8183f09-e120-4e21-a042-59491de84dc4)

</p>

To assemble, attach the pi board to the battery interface and screw it in place. Then plug the screen into the header pins. Take care with the small and delicate components. I made sure not to press hard on the eInk screen, 
as it may damage its functionality. I placed the screen face down on a mouse pad to distribute pressure evenly while carefully inserting the header pins into the screen. It all needs to fit firmly together to fit snugly inside the case.

<p align="center">Components from top, moving down: screen, pi board, battery pack.
	
![parts_together](https://github.com/user-attachments/assets/5a912464-964f-4caa-8f8e-95f321497328)

</p>


## Install and configuration

Credit to [jayofelony](https://github.com/jayofelony/pwnagotchi) who has
kept the project active and is the source for the most recent stable release.

### Download and Write the Image:
   - Download the image file [at this github page](https://github.com/jayofelony/pwnagotchi)
   - Write the image to the SD card using Pi imager or Balena Etcher
### Setup the Network and SSH:
- Set the IP and mask (10.0.0.1 /24)
   - SSH into the device (PuTTY, Powershell) with default credentials which will be changed later
### Edit the Configuration File `etc/pwnagotchi/config.toml`:
   - Name the device, choose language
   - Whitelist any WAP you don't want to attack
   	- Enable display and set type of display
   	- There are a lot of configuration options, see this [reference](https://github.com/evilsocket/pwnagotchi/blob/master/pwnagotchi/defaults.toml) for more detail.
     
- You can also run the device in a headless configuration and connect to your phone via Bluetooth

### Web GUI

There is a web UI at http://10.0.0.2:8080/
	- You will need to change the login : pass in the config.toml file we discussed earlier
	- This provides an easy way to control custom plugins and configuration
	
### Bettercap GUI

There is also a Bettercap UI at http://10.0.0.2:80/

### Placing inside the case
Placing the device inside of the case I chose was a tight fit. I used paper to protect the screen from scratches while sliding it into place.
> **Note**: Be careful with the ribbon cable, it is very fragile.

<p align="center">Assembled components inside the casing.

![parts_in_case](https://github.com/user-attachments/assets/6eeec87c-ab06-49b0-961c-89cfde575c06)

</p>

## Cracking methods

There are different methods to get the `.pcap` files off the device. You can use a tool like FileZilla that offers a GUI to explore the device's files easily. 
The handshake files are found in `/root/handshakes/`. Copy these to your PC or transfer them to a Kali VM if you have one available. Now you can analyze the files in Wireshark for more detail, though
it can be time consuming.

- You can also use the `hcxpcapngtool` that will convert this information for you.
- `hcxpcapngtool -o hash.hc22000 ./handshakes/*.pcap` this statement is explained below:
	- `hcxpcapngtool`: Tool for conversion
	- `-o hash.hc22000`: Output file in hashcat format
	- `./handshakes/*.pcap`: Input directory for `.pcap` files
- A powerful GPU or Cloud VM is recommended for cracking WPA passwords
- Password lists
	- WPA password lists can be found [here](https://www.weakpass.com).
		- 'Super WPA' list 11GB
		- 'All-In-One-WiFi' list 134GB
	- You can also build your own custom wordlists in Kali and combine into custom wordlists
 

## Finished build
<p align="center"> Looks can be deceiving

![finished_build](https://github.com/user-attachments/assets/ab9d369d-ef6c-4880-97f3-56f7fd41c4ad)

</p>

You can see the I/O ports and access windows in the image above.

Credit goes to all of the developers who worked on the device. This project reflects building and studying the device. 
>Note: This project is meant for ethical hacking purposes, education, and analysis. I do not condone using this device or knowledge for malicious purposes.
