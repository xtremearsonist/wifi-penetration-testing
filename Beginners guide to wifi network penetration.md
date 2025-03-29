# Beginners guide to Wifi network penetration 

---
ASSESSING THE SECURITY POSTURE OF A WIFI ACCESS POINT USING
  AIRCRACK-NG AND KALI LINUX
---

# INTRODUCTION

Through this project, you will learn to set up the necessary lab and
tools, and learn to attack and penetrate a wi-fi access point and
retrieve its password. This is a step-by-step guide, which will cover
the methodology and also some common issues that you might face along
the way. It will help you understand how wi-fi access points work, the
vulnerabilities of a wi-fi network and also learn to use Kali Linux and
aircrack-ng to scan and discover wi-fi access points, capture handshake
packets and resolve the password of wi-fi access points that use
WEP/WPA/WPA2 security protocols.

# PRE-REQUISITES

I understand that many of you would be a beginner in cybersecurity
field, or just a little bit curious. So, I'll try my best to explain
things as close to basics as possible. But still, I hope you guys have
some kind of experience with using computers.

There are four things that you would need to complete this project:

-   A computer running Windows 10 or above

-   A wi-fi adapter that supports monitor mode:

Monitor mode is an operating mode in which the wi-fi adapter is able to
capture wireless traffic on a channel without having to connect to an
access point or network. In simple words, wi-fi adapters that support
monitor mode will be able to capture the data sent from a device to the
wi-fi access point without the adapter having to be connected to that
wi-fi access point. Not all wi-fi adapters support monitor mode. So,
you'll have to buy one with a chipset that supports it. Since this
project is targeted mainly at beginners, I'd recommend TP-Link WN722N.
There are four versions of this wi-fi adapter, the latest being V4.0.
All four versions of this adapter support monitor mode, although the
chipset of the first version is Atheros while the later versions come
with Realtek. The difference in chipset is crucial while installing the
necessary drivers for the adapter.

-   A good internet connection

-   A wi-fi network that belongs to you or one that you have permission
    to conduct these tests on:

Trying to attack a wi-fi network that does not belong to you, or
attacking one which you don't have enough permissions for, will probably
get you in trouble because there are laws that are in place to prevent
you from illegally accessing someone's wi-fi network.

# 

# SETTING UP YOUR LAB AND TOOLS

**Step 1 : Setting up your lab**

The first and foremost thing to do would be to set up a safe environment
to work so that we wont mess up our own computer in the process.
Assuming most of you would be running Windows 10 or above, you would
need to have a virtual machine with Kali Linux installed so that you can
use the tools that we need. A virtual machine is a virtual computer
inside your own computer, that is isolated from the files and
applications that you have in your computer. You can mess up the VM as
much as you want, and it will not affect the working of your PC to a
certain extent.

There are two mainstream virtual machines that are available to us for
free, Oracle VirtualBox and Broadcom VMWare. Both are excellent, but
let's just use VirtualBox for now.

You can find the download link for VirtualBox here :
<https://www.virtualbox.org/wiki/Downloads>

After downloading and installing VirtualBox, you will need an image file
of Kali Linux which you can use to run Kali Linux in your system using
VirtualBox. You can find the download link for the image here :
<https://www.kali.org/get-kali/#kali-virtual-machines>

When downloading, make sure you choose the image for VirtualBox, and not
any other virtual machine since we are using VirtualBox for our current
scenario.

After you have downloaded the compressed file from the website, right
click on it and extract it. Open VirtualBox application, click on the
Add button with "+" sign, browse to the folder and select the file and
open it. Once the image file has been loaded into VirtualBox, click on
Start button to start the Kali Linux machine. It will take a couple of
minutes for it to load up. Once it shows the login screen, enter the
default credentials to login:

Username : kali

Password : kali

Once you log in, open a terminal by clicking on the icon that looks like
a black box with white outline with a dollar sign inside, or just press
ctrl+alt+t .

The first thing we have to do is to update the Kali Linux system. So
type the following command and press enter:
```sh
sudo apt-get update -y
```
sudo -- Superuser do (sudo) is a keyword which provides "root"
privileges (higher privileges or administrator privileges which are
required for some commands to work)

apt-get -- Aptitude(apt) is a command-line tool which handles packages
(files which are required for a process to run smoothly). The 'get'
command requests the package that follows the command.

update -- To update the package lists

-y -- While executing some commands, there could be Yes or No questions
asking the user for permission to do something. Giving a '-y' suffix to
a command will automatically give 'YES' permission to any request that
appears while execution.

Once the first command finished executing, type the following command
and press enter:
```sh
sudo apt-get upgrade -y
```
upgrade -- installs the newest packages

It will probably take some time to finish, so sit back and relax for a
bit.

After the upgrade has finished, type the following command to reboot the
Kali VM:
```sh
sudo reboot
```
**Step 2 : Installing tools**

The tools we require for conducting these tests come pre-installed in
Kali Linux. But just to make sure, type the command "aircrack-ng" into
the command terminal and press enter. You should be seeing something
like this:

Aircrack-ng 1.7 - © 2006-2022 Thomas d'Otreppe

<https://www.aircrack-ng.org>

Followed by the usage of the command and the different options that the
tool offers.

**Step 3 : Installing wi-fi adapter drivers**

After updating and upgrading the system, we need to install the drivers
which are necessary for our wi-fi adapter to work with Kali Linux.
Assuming we are using the adapter that I mentioned above, the TP Link
WN722N, I will guide you through the installation procedure. As I
mentioned, there are four versions of this adapter. I'm only explaining
the procedures for the latest version which is the V4.0 which also
applies to V2.0 and V3.0. The reason for excluding the procedures for
installing drivers for version 1 is because it is no longer being
manufactured by the company.

As I mentioned before, a virtual machine is isolated from the OS that it
runs on. Which means that you would have to manually select which
devices that you have connected to your computer is also accessible from
the virtual machine. For that, first you have to close the Kali Linux
window. Open the Oracle virtualbox window, and right click on the Kali
Linux image, and click on settings. There, you can see a list of
different categories. Click on USB, and then click on the small dot on
the left side of 'USB 3.0(xHCI) controller'. You'll see a section 'USB
device filters' and a big box under that. On the right side of this box,
you'll see small icons one under the other. The second icon has a small
plus sign, click on that and a drop down list will open up. Select
'Realtek 802.11n NIC' and you'll notice that the adapter has appeared as
a check list in the big box. Now just click 'OK' and the window will
close.

Now, start the Kali Linux VM again and open a terminal. In the terminal,
enter the following command:
```sh
sudo apt install -y realtek-rtl8188eus-dkms
```
After the command finishes execution, enter the command 'reboot' and the
Kali Linux VM will reboot. Once it reboots, again open a terminal and
enter the following commands:
```sh
sudo airmon-ng check kill
```
This command will kill any processes that are currently running which
interferes with the wi-fi adapter we want to use
```sh
sudo airmon-ng start wlan0
```
This command will put the wi-fi adapter, which is wlan0, to monitor
mode.

There is also another set of commands that can put your adapter into
monitor mode:
```sh
sudo ifconfig wlan0 down
```
```sh
sudo airmon-ng check kill
```
```sh
sudo iwconfig wlan0 mode monitor
```
```sh
sudo ifconfig wlan0 up
```
As you can guess, we put the adapter into monitor mode.

Since we have put the adapter into monitor mode, lets get to the next
phase.

# HOW WI-FI ACCESS POINTS WORK AND HOW WE EXPLOIT IT

Wi-fi access points connect with devices using a "4 way handshake". This
part is going to be a bit tough to understand for beginners, so pay
close attention.

How a 4 way handshake between an Access point and a Phone works is:

-   Step 1: Access Point (AP) starts the conversation and gives Phone a
    unique number.

-   Step 2: Phone creates its own number, combines it with the APs
    number, and the password you typed in, to create a special key.

-   Step 3: AP also makes the key. It sends back its version of the key
    and a group key for everyone on the network to use.

-   Step 4: Phone checks that the key which the AP made matches the one
    it made. If they match, then they are both using the correct
    password, and says that the connection is ready and connects to the
    AP.

This is where aircrack-ng comes into place. As I described earlier,
monitor mode helps to capture the data packets that are sent over wi-fi.
We use aircrack-ng to capture this '4 way handshake' and decrypt it to
get the wi-fi password. How we decrypt is, we compare the captured
handshake to a list of passwords called a wordlist using aircrack-ng.
That is, for each password in the list, aircrack-ng performs the same
calculations which the access point and the phone performed in steps 2
and 3 of the handshake process.

This doesn't mean that we will surely be able to get the password 100%
of the time. There are many more passwords that people use, which are
not present in the wordlist that we use. But if the password used by the
wi-fi AP is in the wordlist, then surely we will get it. There are tools
which can be used to generate wordlists using information that you may
have about the owner of the wi-fi AP. But let's just focus on the task
at hand.

# HOW TO USE AIRCRACK-NG TO ATTACK AND TEST WI-FI ACCESS POINTS

Since we have already put our adapter in monitor mode, lets get to
scanning the available wi-fi networks. For that, enter the following
command :
```sh
sudo airodump-ng wlan0
```
You should now be able to see a list of wi-fi access points that are in
range, with their BSSID (the name of the AP), MAC address (a name given
to the wi-fi AP for other devices to identify it by), channel number
(like channels on a television, wi-fi APs use different channels to
transmit data) and the signal strength of each of them as shown below:

CH 7\]\[ Elapsed: 36 s\]\[ 2025-03-15 19:16

BSSID PWR Beacons #Data, #/S CH MB ENC CIPHER AUTH ESSID

B4:3D:08:52:54:D0 -68 97 0 0 5 270 WPA2 CCMP PSK HomeWIFI

It will keep on scanning until you suspend the process or quit the
process. To quit, press ctrl+c. To suspend, press ctrl+z.

Here, we need to note down a few things. There is only a single wi-fi AP
shown here. There could be several wi-fi APs shown depending upon where
you are and the range of nearby wi-fi APs. For our current scenario, I'm
only explaining with a single example.

The name of the wi-fi AP is "HomeWIFI".

The MAC address for this wi-fi AP is "B4:3D:08:52:54:D0".

This network is on channel no. 5.

These are the three things we need for the next step.

sudo airodump-ng \--bssid XX:XX:XX:XX:XX:XX -c X -w capture wlan0

This command targets a specific wi-fi AP, whose BSSID you mention after
the keyword "---bssid". You should also mention the channel number of
the wi-fi AP. In our case, it is 5. Mention the channel number in the
place of 'X' after the keyword "-c".

For example, once I fill in the BSSID and channel number, the command
should look like this:
```sh
sudo airodump-ng --bssid B4:3D:08:52:54:D0 -c 5 -w capture wlan0
```
After you have filled in the data, press enter and it should start
scanning for devices that are connected to that specific wi-fi AP. The
output would look something like this:

CH 11 \]\[ Elapsed: 3 mins \]\[ 2025-03-29 05:29

BSSID PWR RXQ Beacons #Data, #/s CH MB ENC CIPHER AU

B4:3D:08:51:52:D9 -41 96 2194 278 0 11 270 WPA2 CCMP PS

BSSID STATION PWR Rate Lost Frames Notes

B4:3D:08:51:52:D9 AA:1B:9F:D0:BA:9B -58 1e- 1e 169 1637

Now, open another terminal. We need to deauth (disconnect) the device
that is connected to the target network, in this case, it is
AA:1B:9F:D0:BA:9B. So, to deauth this device from the wi-fi AP, enter
the following command:
```sh
sudo aireplay-ng \--deauth 10 -a B4:3D:08:51:52:D9 -c AA:1B:9F:D0:BA:9B wlan0
```
aireplay-ng : It is a tool that's part of the aircrack-ng suite, which
is used to inject packets, in this case, send a deauth packet to the
device connected to the wi-fi AP.

\--deauth 10 : It specifies to the aireplay-ng tool that we want to
conduct a deauth attack. The number 10 indicates the number of
deauthentication packets that we wish to send.

-a B4:3D:08:51:52:D9 : It indicates the BSSID of the wi-fi AP

-c AA:1B:9F:D0:BA:9B : It indicates the MAC address of the device we
wish to disconnect from the wi-fi AP

Wlan0 : This specifies the wireless network interface that we wish to
use in order to perform the deauthentication attack.

Once this command is executed, you will see a long list of deauth
packets that are being sent to the target device. You can check the
device to see if it is being disconnected. After it gets disconnected,
it will try to reconnect and that is when we will capture the handshake.
Once the handshake is captured, the result will look something like
this:

05:25:34 Created capture file \"capture-01.cap\".

CH 11 \]\[ Elapsed: 3 mins \]\[ 2025-03-29 05:29 \]\[ WPA handshake:
B4:3D:08:51:

BSSID PWR RXQ Beacons #Data, #/s CH MB ENC CIPHER AU

B4:3D:08:51:52:D9 -41 96 2194 278 0 11 270 WPA2 CCMP PS

BSSID STATION PWR Rate Lost Frames Notes

B4:3D:08:51:52:D9 AA:1B:9F:D0:BA:9B -58 1e- 1e 169 1637 EAPOL

(REMEMBER THAT THIS OUTPUT WILL ONLY BE SEEN IN THE TERMINAL WINDOW
WHERE YOU RAN THE PREVIOUS COMMAND)

Notice the 'WPA handshake : XX:XX:XX:XX:XX' in the top right corner?
That indicates that the handshake has been captured. Now you can quit
this process by pressing ctrl+c.

Also, if you scroll up to the line right after the previous command was
executed, you can see a line '05:25:34 Created capture file
\"capture-01.cap\" '. The file "capture-01.cap" is the file where the
handshake has been stored. We use this file to get the password.

# HOW TO CRACK THE PASSWORD FROM HANDSHAKE FILE

For cracking the handshake file, we need two things: the capture file
and a wordlist. A wordlist is a file, much like a document file, where
there would be a large number of possible password combinations that we
use to try to crack the captured handshake file and the process of doing
so, is called a dictionary attack. The bigger the number of password
combinations, the better chance you have in cracking the password. There
are also several tools available which we can use to make wordlist files
depending upon the data we feed into it to make customised wordlists
based on the owner of the wi-fi AP. But for now, we are using a default
wordlist file that already exists in Kali Linux.

Enter the following command:
```sh
sudo aircrack-ng -w /usr/share/wordlists/rockyou.txt capture-01.cap
```
-w /usr/share/wordlists/rockyou.txt : This specifies the path to the
folder where the wordlist we are going to use is located. "rockyou.txt"
is the name of the wordlist. This file is just over a 100 MB in size,
whereas you can download "rockyou2024.txt" from the internet which is
over 140 GB in size when extracted. As I mentioned before, the bigger
the wordlist, the better. If by any chance when you run this command and
the file is not present in this location, then just search and download
rockyou.txt file and then drag and drop to the Kali Linux VM desktop,
and the location specified in the command would change
(/home/kali/Desktop/rockyou.txt)

Capture-01.cap : The name of the file where the handshake we captured is
stored.

Once the command is entered and execution starts, you will see a
"Matrix-like" interface where you can see each password being tested
against the handshake we captured. This is going to take some time
depending upon the power of the processor of your computer.

If the password of the wi-fi AP is in there, surely you will get a
match. And if there are no matches, then rest assured, you have a strong
password that's going to be much harder to crack down.