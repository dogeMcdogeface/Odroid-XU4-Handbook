<div class="container">

# XU4 Handbook 25/04/2023

This is a collection of useful scripts, commands and procedures for setting up and controlling an Odroid XU4, an ARM based SBC

Markdown autogenerated by [browserling.com](https://www.browserling.com/tools/html-to-markdown).  
Some functions might not be available if you are viewing a markdown version

</div>

<div id="scriptGenerated" style="display:none;">

<div class="details toc-container notransition"><summary>

Table of Contents

</summary></div>

### Guide Settings

Auto fill-in tutorial commands:

<table id="controlsTable"></table>

<!--const data = [ [ {label: "Network SSID", fieldName: "net-ssid", placeHolder: "examWifi623"}, {label: "Network password", fieldName: "net-pswd", placeHolder: "Passw0rd"}, ], [ {label: "Device hostname", fieldName: "net-hnme", placeHolder: "odroidXU4"}, {label: "Static ip", fieldName: "net-ip", placeHolder: "192.186.2.123/24"}, ], [ {label: "User name", fieldName: "usr-name", placeHolder: "User"}, {label: "User password", fieldName: "usr-pswd", placeHolder: "P4ssw0r8"}, ], [ {label: "Unattended-Upgrades mail", fieldName: "upd-mail", placeHolder: "your@mail.gum"}, {label: "Unattended-Upgrades Reboot Hour", fieldName: "upd-hour", placeHolder: "02:00"}, ], ];--></div>

<div class="details"><summary>

## Flash OS

</summary>

[Official OS Installation Guide](https://wiki.odroid.com/odroid-xu4/getting_started/os_installation_guide#tab__odroid-xu4) Power supply minimum: 5V/2A recommended: 5V/6A

[Ubuntu MATE 22.04 desktop image.](https://odroid.in/ubuntu_22.04lts/XU3_XU4_MC1_HC1_HC2/ubuntu-22.04-5.4-mate-odroid-xu4-20220522.img.xz) At least 8 GB is recommended

`user/passwd: odroid/odroid` `root/odroid`

[Ubuntu Minimal 22.04 image.](https://odroid.in/ubuntu_22.04lts/XU3_XU4_MC1_HC1_HC2/ubuntu-22.04-5.4-minimal-odroid-xu4-20220721.img.xz) At least 4 GB is recommended

`user/passwd: root/odroid`

1.  Download an OS image
2.  Download Etcher
3.  Open Etcher.
4.  Select the downloaded OS image.
5.  Select the inserted memory card. (Normally, the memory card is detected automatically.)
6.  Click Flash

</div>

<div class="details"><summary>

## Connect to Wi-Fi

</summary>

If you don't have access to an ethernet connection, you will need a wireless adaptor (e.g. an usb dongle) to ssh into the machine.  
The easiest way to interact with the machine without a network connection will be to connect a monitor and keyboard and use the terminal.  
This guide assumes a minimal installation with no gui, but network manager installed.

### Enable Wi-Fi

<pre>sudo su
nmcli radio wifi on
nmcli radio wifi

nmcli dev wifi list  </pre>

> Output:
>
> <pre>enabled</pre>
>
> <pre>    
>
> <table>
>         
>
> <thead>
>         
>
> <tr>
>             
>
> <th>IN-USE</th>
>
>             
>
> <th>BSSID</th>
>
>             
>
> <th>SSID</th>
>
>             
>
> <th>MODE</th>
>
>             
>
> <th>CHAN</th>
>
>             
>
> <th>RATE</th>
>
>             
>
> <th>SIGNAL</th>
>
>             
>
> <th>BARS</th>
>
>             
>
> <th>SECURITY</th>
>
>         </tr>
>
>         </thead>
>
>         
>
> <tbody>
>         
>
> <tr>
>             
>
> <td>*</td>
>
>             
>
> <td>ZZ:ZZ:QE:34:05:RQ</td>
>
>             
>
> <td class="net-ssid">Network-name</td>
>
>             
>
> <td>Infra</td>
>
>             
>
> <td>77</td>
>
>             
>
> <td>111 Mbit/s</td>
>
>             
>
> <td>95</td>
>
>             
>
> <td>****</td>
>
>             
>
> <td>WPA2</td>
>
>         </tr>
>
>         
>
> <tr>
>             
>
> <td></td>
>
>             
>
> <td>ZZ:ZZ:SS:CD:TT:FW</td>
>
>             
>
> <td>ANother-Ntwk</td>
>
>             
>
> <td>Infra</td>
>
>             
>
> <td>44</td>
>
>             
>
> <td>222 Mbit/s</td>
>
>             
>
> <td>92</td>
>
>             
>
> <td>****</td>
>
>             
>
> <td>WPA2</td>
>
>         </tr>
>
>         
>
> <tr>
>             
>
> <td></td>
>
>             
>
> <td>ZZ:ZZ:CC:RR:1S:BB</td>
>
>             
>
> <td>blipBloppers</td>
>
>             
>
> <td>Infra</td>
>
>             
>
> <td>63</td>
>
>             
>
> <td>333 Mbit/s</td>
>
>             
>
> <td>59</td>
>
>             
>
> <td>***</td>
>
>             
>
> <td>WPA2</td>
>
>         </tr>
>
>         </tbody>
>
>     </table>
>
> </pre>

### Interactive network options

Network Manager offers a tui interface:

<pre>nmtui</pre>

> <pre>    ┌─┤ NetworkManager TUI ├──┐        ┌───────────────────────────────────────────────┐
>     │                         │        │                                               │
>     │ Please select an option │        │                                               │
>     │                         │        │ ┌──────────────────────────────┐              │
>     │ Edit a connection       │        │ │ Wired                        │ [Activate]   │
>     │ Activate a connection   │        │ │ * Wired connection 1       ▒ │              │
>     │ Set system hostname     │        │ │                            ▒ │              │
>     │                         │        │ │ Wi-Fi                      ▒ │              │
>     │ Quit                    │        │ │   Network-wifi       ***   ▒ │              │
>     │                         │        │ │   Neighbr-ntwk32     ****  ▒ │              │
>     │                    [OK] │        │ │                            ▒ │              │
>     │                         │        │ │                            ▒ │              │
>     └─────────────────────────┘        │ │                            ▒ │              │
>
>     </pre>

### Command line network options

> Warning: these commands will display your Wi-Fi password in the terminal, and might leak it in the command history. Use the interactive tools if possible.

Command line connection:

<pre>nmcli device wifi connect <ins class="net-ssid">network_ssid</ins> password <ins class="net-pswd">network_pswd</ins></pre>

Connect with a static ip:

<pre>nmcli con add con-name "<ins class="net-ssid">network_ssid</ins>" type wifi ifname wlan0 ssid "<ins class="net-ssid">network_ssid</ins>" -- wifi-sec.key-mgmt wpa-psk wifi-sec.psk "<ins class="net-pswd">network_pswd</ins>" ipv4.method manual ipv4.address <ins class="net-ip">YOUR IP</ins>
nmcli con up "<ins class="net-ssid">network_ssid</ins>"</pre>

> <pre>    Connection '<ins class="net-ssid">network_ssid</ins>' (12312323-3333-3333-3333-asd222sss2222) successfully added.
>     Connection successfully activated (D-Bus active path: /org/freedesktop/NetworkManager/ActiveConnection/4)</pre>

### Delete a connection

<pre>nmcli connection delete "<ins class="net-ssid">network_ssid</ins>"</pre>

> <pre>Connection '<ins class="net-ssid">network_ssid</ins>' (123123123-1233-3211-qwee-42124234d) successfully deleted.</pre>

### Change connection priority

<pre>    nmcli connection modify "Wired connection 1" connection.autoconnect-priority 10
    nmcli -f NAME,UUID,AUTOCONNECT,AUTOCONNECT-PRIORITY c</pre>

> <pre>    
>
> <table>
>   
>
> <tbody>
>
> <tr>
>     
>
> <th>Name</th>
>
>     
>
> <th>UUID</th>
>
>     
>
> <th>Autoconnect</th>
>
>     
>
> <th>Autoconnect-priority</th>
>
>   </tr>
>
>   
>
> <tr>
>     
>
> <td>Wired connection 1</td>
>
>     
>
> <td>12312323-4444-4444-4444-123123123123</td>
>
>     
>
> <td>yes</td>
>
>     
>
> <td>10</td>
>
>   </tr>
>
> </tbody>
>
> </table>
>
> </pre>

### Verify your connections

<pre>nmcli connection show
ip a |grep inet</pre>

> Output:
>
> <pre>
>
> <table>
>         
>
> <thead>
>         
>
> <tr>
>             
>
> <th>NAME</th>
>
>             
>
> <th>UUID</th>
>
>             
>
> <th>TYPE</th>
>
>             
>
> <th>DEVICE</th>
>
>         </tr>
>
>         </thead>
>
>         
>
> <tbody>
>         
>
> <tr>
>             
>
> <td>Wired connection 1</td>
>
>             
>
> <td>55555555-1111-2222-3333-444444444444</td>
>
>             
>
> <td>ethernet</td>
>
>             
>
> <td>eth0</td>
>
>         </tr>
>
>         
>
> <tr>
>             
>
> <td><ins class="net-ssid">network_ssid</ins></td>
>
>             
>
> <td>55555555-1111-2222-3333-444444444444</td>
>
>             
>
> <td>wlan0</td>
>
>             
>
> <td>Infra</td>
>
>         </tr>
>
>       </tbody>
>
>     </table>
>
>         </pre>
>
> <pre>inet <mark>192.168.1.111/24</mark> brd 192.168.1.255 scope global <mark>dynamic</mark> noprefixroute eth0      <--Ethernet Adapter
> inet <mark><ins class="net-ip">YOUR IP</ins></mark> brd 192.168.1.255 scope global noprefixroute wlan0              <--Wifi Adapter</pre>

> If you see a connection marked as dynamic, the IP might change unpredictably, e.g. after reboots, and you will not be able to ssh in until you figure out the new IP!

Verify internet access:

<pre>ping google.com</pre>

> <pre>    64 bytes from mil04s43-in-f14.1e100.net (142.250.180.142): icmp_seq=1 ttl=115 time=7.66 ms
>     64 bytes from mil04s43-in-f14.1e100.net (142.250.180.142): icmp_seq=2 ttl=115 time=7.60 ms
>     ...</pre>

### Set host name

The system hostname is used to identify a machine in a network in a human-readable format.

View current hostname:

<pre>hostname</pre>

> <pre>hostname odroid</pre>

Change hostname:

<pre>sudo hostnamectl set-hostname <ins class="net-hnme">new_hostname</ins>
hostname</pre>

> <pre>hostname <ins class="net-hnme">new_hostname</ins></pre>

</div>

<div class="details"><summary>

## Network Info

</summary>

### Network Managers

View available network managers:

<pre>systemctl --type=service | grep --ignore-case 'network'</pre>

> <pre>    NetworkManager.service                         loaded active running Network Manager
>     systemd-networkd-wait-online.service           loaded active exited  Wait for Network to be Configured
>     ...</pre>

### ip

Show addresses assigned to all network interfaces:

<pre>ip a</pre>

More concise output:

<pre> ip a | grep "inet "</pre>

> <pre>    inet 127.0.0.1/8 scope host lo
>     inet 192.168.1.111/24 brd 192.168.1.255 scope global dynamic noprefixroute eth0
>     inet 192.168.1.222/24 brd 192.168.1.255 scope global noprefixroute wlan0</pre>

Get connection statistics:

<pre>ip -s link</pre>

> <pre>    
>
> <table>
>   
>
> <tbody>
>
> <tr>
>     
>
> <th>Interface</th>
>
>     
>
> <th>Description</th>
>
>     
>
> <th>MTU</th>
>
>     
>
> <th>State</th>
>
>     
>
> <th>Mode</th>
>
>     
>
> <th>Group</th>
>
>     
>
> <th>Qlen</th>
>
>     
>
> <th>Link/ether</th>
>
>     
>
> <th>Broadcast</th>
>
>   </tr>
>
>       
>   
>
> <tr>
>     
>
> <td>eth0</td>
>
>     
>
> <td>BROADCAST,MULTICAST,UP,LOWER_UP</td>
>
>     
>
> <td>1500</td>
>
>     
>
> <td>fq_codel state UP</td>
>
>     
>
> <td>DEFAULT</td>
>
>     
>
> <td>default</td>
>
>     
>
> <td>1000</td>
>
>     
>
> <td>00:1e:06:37:42:ff</td>
>
>     
>
> <td>ff:ff:ff:ff:ff:ff</td>
>
>   </tr>
>
>   
>
> <tr>
>     
>
> <td></td>
>
>     
>
> <td>RX:</td>
>
>     
>
> <td>bytes</td>
>
>     
>
> <td>packets</td>
>
>     
>
> <td>errors</td>
>
>     
>
> <td>dropped</td>
>
>     
>
> <td>missed</td>
>
>     
>
> <td>mcast</td>
>
>     
>
> <td></td>
>
>   </tr>
>
>   
>
> <tr>
>     
>
> <td></td>
>
>     
>
> <td></td>
>
>     
>
> <td>1062498</td>
>
>     
>
> <td>6196</td>
>
>     
>
> <td>0</td>
>
>     
>
> <td>57</td>
>
>     
>
> <td>0</td>
>
>     
>
> <td>0</td>
>
>     
>
> <td></td>
>
>   </tr>
>
>   
>
> <tr>
>     
>
> <td></td>
>
>     
>
> <td>TX:</td>
>
>     
>
> <td>bytes</td>
>
>     
>
> <td>packets</td>
>
>     
>
> <td>errors</td>
>
>     
>
> <td>dropped</td>
>
>     
>
> <td>carrier</td>
>
>     
>
> <td>collsns</td>
>
>     
>
> <td></td>
>
>   </tr>
>
>   
>
> <tr>
>     
>
> <td></td>
>
>     
>
> <td></td>
>
>     
>
> <td>3794910</td>
>
>     
>
> <td>32297</td>
>
>     
>
> <td>0</td>
>
>     
>
> <td>0</td>
>
>     
>
> <td>0</td>
>
>     
>
> <td>0</td>
>
>     
>
> <td></td>
>
>       </tr>
>
>   
>
> <tr>
>     
>
> <td>wlan0</td>
>
>     
>
> <td>BROADCAST,MULTICAST,UP,LOWER_UP</td>
>
>     
>
> <td>1500</td>
>
>     
>
> <td>mq state UP</td>
>
>     
>
> <td>DORMANT</td>
>
>     
>
> <td>default</td>
>
>     
>
> <td>1000</td>
>
>     
>
> <td>1c:bf:ce:77:84:74</td>
>
>     
>
> <td>ff:ff:ff:ff:ff:ff</td>
>
>   </tr>
>
>   
>
> <tr>
>     
>
> <td></td>
>
>     
>
> <td>RX:</td>
>
>     
>
> <td>bytes</td>
>
>     
>
> <td>packets</td>
>
>     
>
> <td>errors</td>
>
>     
>
> <td>dropped</td>
>
>     
>
> <td>missed</td>
>
>     
>
> <td>mcast</td>
>
>     
>
> <td></td>
>
>   </tr>
>
>   
>
> <tr>
>     
>
> <td></td>
>
>     
>
> <td></td>
>
>     
>
> <td>3992249</td>
>
>     
>
> <td>33808</td>
>
>     
>
> <td>0</td>
>
>     
>
> <td>24</td>
>
>     
>
> <td>0</td>
>
>     
>
> <td>0</td>
>
>     
>
> <td></td>
>
>   </tr>
>
>   
>
> <tr>
>     
>
> <td></td>
>
>     
>
> <td>TX:</td>
>
>     
>
> <td>bytes</td>
>
>     
>
> <td>packets</td>
>
>     
>
> <td>errors</td>
>
>     
>
> <td>dropped</td>
>
>     
>
> <td>carrier</td>
>
>     
>
> <td>collsns</td>
>
>     
>
> <td></td>
>
>   </tr>
>
>   
>
> <tr>
>     
>
> <td></td>
>
>     
>
> <td></td>
>
>     
>
> <td>40978</td>
>
>     
>
> <td>416</td>
>
>     
>
> <td>0</td>
>
>     
>
> <td>0</td>
>
>     
>
> <td>0</td>
>
>     
>
> <td>0</td>
>
>     
>
> <td></td>
>
>   </tr>
>
> </tbody>
>
> </table>
>
>     </pre>

### nmcli

nmcli is a command-line tool for controlling NetworkManager and reporting network status.

Get information on devices and connections:

<pre>nmcli</pre>

Get information on active connections:

<pre>nmcli -p device</pre>

> <pre>    =====================
>      Status of devices
>     =====================
>     DEVICE  TYPE      STATE      CONNECTION
>     -----------------------------------------------------------------------
>     eth0    ethernet  connected  Wired connection 1
>     wlan0   wifi      connected  <ins class="net-ssid">network_ssid</ins></pre>

See connection priorities:

<pre>nmcli -f NAME,UUID,AUTOCONNECT,AUTOCONNECT-PRIORITY c</pre>

> <pre>    
>
> <table>
>       
>
> <tbody>
>
> <tr>
>         
>
> <th>Name</th>
>
>         
>
> <th>UUID</th>
>
>         
>
> <th>Autoconnect</th>
>
>         
>
> <th>Autoconnect-priority</th>
>
>       </tr>
>
>       
>
> <tr>
>         
>
> <td>Wired connection 1</td>
>
>         
>
> <td>12312323-4444-4444-4444-123123123123</td>
>
>         
>
> <td>yes</td>
>
>         
>
> <td>-999</td>
>
>       </tr>
>
>       
>
> <tr>
>         
>
> <td><ins class="net-ssid">network_ssid</ins></td>
>
>         
>
> <td>12312323-4444-4444-4444-123123123123</td>
>
>         
>
> <td>yes</td>
>
>         
>
> <td>0</td>
>
>       </tr>
>
>     </tbody>
>
> </table>
>
>     </pre>

### iwconfig

Iwconfig may be used to display the parameters of the network interface, and the wireless statistics.

<pre>iwconfig</pre>

> <pre>    lo        no wireless extensions.
>
>     wlan0     IEEE 802.11  ESSID:"<ins class="net-ssid">network_ssid</ins>"
>     Mode:Managed  Frequency:2.462 GHz  Access Point: 11:22:33:44:55:66
>     Bit Rate=65 Mb/s   Tx-Power=1744 dBm
>     Retry short limit:4   RTS thr:off   Fragment thr:off
>     Encryption key:on
>     Power Management:off
>     Link Quality=67/70  Signal level=43 dBm
>     Rx invalid nwid:0  Rx invalid crypt:0  Rx invalid frag:0
>     Tx excessive retries:1  Invalid misc:199   Missed beacon:0
>
>     eth0      no wireless extensions.</pre>

</div>

<div class="details"><summary>

## User Management

</summary>

List all users:

<pre>sudo su
cut -d: -f1 /etc/passwd</pre>

### Interactive user creation

Create a new user named <ins class="usr-name">user</ins> with sudo privileges:

<pre>sudo su	
adduser <ins class="usr-name">user</ins>
</pre>

> You will be asked a few information
>
> <pre>Creating home directory `/home/<ins class="usr-name">user</ins>' ...
> Copying files from `/etc/skel' ...
> New password		<---type here
> Retype new password:	<---type here
> passwd: password updated successfully
> Changing the user information for gigi
> Enter the new value, or press ENTER for the default
>         Full Name []:
>         Room Number []:
>         Work Phone []:
>         Home Phone []:
>         Other []:
> Is the information correct? [Y/n] y
> </pre>

Bestow sudo privileges to the new user.

<pre>usermod -aG sudo <ins class="usr-name">user</ins>
</pre>

### Automated user creation

> Warning, this method might display your password on the screen and leak it through the command history.  
> Please follow the interactive method instead.

Create a new user named <ins class="usr-name">user</ins>:

<pre>useradd -m <ins class="usr-name">user</ins>
  echo "<ins class="usr-name">user</ins>:<ins class="usr-pswd">pswd</ins>" | chpasswd;history -d $(history 1)
#^ The leading space is important, don't remove 
clear
history | tail
</pre>

> Warning, if your password was displayed, change it immediately.
>
> <pre>183  useradd -m User
> 184  "<ins class="usr-name">user</ins>:<ins class="usr-pswd">pswd</ins>" | chpasswd
> 185  history | tail
> </pre>

Bestow sudo privileges to the new user.

<pre>usermod -aG sudo <ins class="usr-name">user</ins>
</pre>

### Remove default user

Delete the user "odroid" and remove their home directory:

<pre>sudo deluser --remove-home odroid</pre>

</div>

<div class="details"><summary>

## Security

</summary>

### SSH Configuration

Disable root login:

<pre>sed -i 's/PermitRootLogin yes/PermitRootLogin no/g'  /etc/ssh/sshd_config</pre>

<div>or</div>

<pre>sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin no/g'  /etc/ssh/sshd_config</pre>

Verify that root login is disabled:

<pre>cat  /etc/ssh/sshd_config | grep "PermitRootLogin "</pre>

> Output:
>
> <pre>PermitRootLogin no</pre>

Restart the SSH service:

<pre>sudo systemctl restart sshd </pre>

### Firewall Setup

> Make sure you are not logged in as root, as this will prevent remote connections to root

<pre>sudo su
apt install ufw
ufw allow OpenSSH
ufw enable
ufw status</pre>

> Output:
>
> <pre>
>
> <table>
>   
>
> <thead>
>     
>
> <tr>
>       
>
> <th>To</th>
>
>       
>
> <th>Action</th>
>
>       
>
> <th>From</th>
>
>     </tr>
>
>   </thead>
>
>   
>
> <tbody>
>     
>
> <tr>
>       
>
> <td>OpenSSH</td>
>
>       
>
> <td>ALLOW</td>
>
>       
>
> <td>Anywhere</td>
>
>     </tr>
>
>     
>
> <tr>
>       
>
> <td>OpenSSH (v6)</td>
>
>       
>
> <td>ALLOW</td>
>
>       
>
> <td>Anywhere (v6)</td>
>
>     </tr>
>
>   </tbody>
>
> </table>
>
> </pre>

</div>

<div class="details"><summary>

## Update OS

</summary>

### Update and Upgrade

Update the OS by running the following command in a terminal:

<pre>sudo apt update -y && sudo apt upgrade -y && sudo apt dist-upgrade -y && sudo apt autoremove -y</pre>

### Automated System Updates and Reboots

<div>Enable automatic system updates and reboots</div>

<pre>sudo su
sudo apt install unattended-upgrades </pre>

<div>Configure custom settings</div>

<pre>echo '
// CUSTOM SETTINGS
Unattended-Upgrade::Mail "<ins class="upd-mail">your@mail.com</ins>";
Unattended-Upgrade::Automatic-Reboot "true";
Unattended-Upgrade::Automatic-Reboot-Time <ins class="upd-hour">02:00</ins>";
Unattended-Upgrade::Remove-Unused-Kernel-Packages “true”;
Unattended-Upgrade::Remove-New-Unused-Dependencies “true”;
' >> /etc/apt/apt.conf.d/50unattended-upgrades
cat /etc/apt/apt.conf.d/50unattended-upgrades
</pre>

<div>Execute test run with verbose output</div>

<pre>sudo unattended-upgrade -v -d </pre>

*   `Unattended-Upgrade::Mail` option specifies the email address to receive notifications about upgrades.
*   `Unattended-Upgrade::Automatic-Reboot` enables automatic reboots after upgrades..
*   `Unattended-Upgrade::Automatic-Reboot-Time` option sets the time for the automatic reboot to occur
*   `Unattended-Upgrade::Remove-Unused-Kernel-Packages` and `Unattended-Upgrade::Remove-New-Unused-Dependencies` options remove any unused packages and dependencies after an upgrade.

</div>

<div class="details"><summary>

## Hardware Control

</summary>

### Onboard Led

<div>Set led brightness</div>

<pre>echo 250 >  /sys/class/leds/blue\:heartbeat/brightness
cat  /sys/class/leds/blue\:heartbeat/brightness</pre>

> <pre>250</pre>

<div>See available led modes</div>

<pre>cat /sys/devices/platform/pwmleds/leds/blue\:heartbeat/trigger</pre>

> <pre>none rc-feedback kbd-scrolllock kbd-numlock kbd-capslock kbd-kanalock kbd-shiftlock kbd-altgrlock kbd-ctrllock kbd-altlock kbd-shiftllock kbd-shiftrlock kbd-ctrlllock kbd-ctrlrlock mmc0 mmc1 timer oneshot heartbeat gpio cpu cpu0 cpu1 cpu2 cpu3 cpu4 cpu5 cpu6 cpu7 [default-on] transient rfkill-any rfkill-none phy0rx phy0tx phy0assoc phy0radio rfkill0</pre>

<div>Turn OFF, ON, Heartbeat (original state)</div>

<pre>echo none > /sys/class/leds/blue\:heartbeat/trigger
echo default-on > /sys/class/leds/blue\:heartbeat/trigger
echo heartbeat > /sys/class/leds/blue\:heartbeat/trigger</pre>

### GPIO Pins

### System Temperature

</div>

<div class="details"><summary>

## Performance Governor

</summary></div>

<div class="details"><summary>

## Onboard Fan

</summary>

### Control manually via pwm speed

<div>Disable automatic control and Write to pwm file</div>

<pre>echo 0 > /sys/devices/platform/pwm-fan/hwmon/hwmon0/automatic
echo 10 > /sys/devices/platform/pwm-fan/hwmon/hwmon0/pwm1</pre>

<div>Verify</div>

<pre>cat /sys/devices/platform/pwm-fan/hwmon/hwmon0/automatic
cat /sys/devices/platform/pwm-fan/hwmon/hwmon0/pwm1</pre>

> <pre>1
> 120</pre>

### Control automatically

<div>Enable automatic control</div>

<pre>echo 1 > /sys/devices/platform/pwm-fan/hwmon/hwmon0/automatic
cat /sys/devices/platform/pwm-fan/hwmon/hwmon0/automatic</pre>

> <pre>1</pre>

### Trip points

ODROID XU4 supports 3 cooling levels for thermal control, 0, 1, 2. Level 0, which is the lowest level for thermal control and comes with the slowest fan speed. And level 2, which is the highest level for thermal control and comes with the fastest fan speed. [Reference](https://wiki.odroid.com/odroid-xu4/application_note/manually_control_the_fan)

<div>Default temps:</div>

<table>

<thead>

<tr>

<th>Trip point</th>

<th>-</th>

<th>0</th>

<th>1</th>

<th>2</th>

</tr>

</thead>

<tbody>

<tr>

<td>Temperature</td>

<td>0</td>

<td>60°C</td>

<td>70°C</td>

<td>80°C</td>

</tr>

<tr>

<td>Fan speed</td>

<td>0</td>

<td>120</td>

<td>180</td>

<td>240</td>

</tr>

</tbody>

</table>

Get current trip temperatures

<pre>cat /sys/devices/virtual/thermal/thermal_zone{0,1,2,3}/trip_point_{0,1,2}_temp</pre>

> <pre>60000   <------ Zone 0
> 70000
> 80000
> 60000   <------ Zone 1
> 70000
> 80000
> 60000   <------ Zone 2
> 70000
> 80000
> 60000   <------ Zone 3
> 70000
> 80000
> </pre>

<div>Set trip point 1 to be activated at 30°C.</div>

<pre>echo 30000 | sudo tee /sys/devices/virtual/thermal/thermal_zone{0,1,2,3}/trip_point_0_temp
cat /sys/devices/virtual/thermal/thermal_zone{0,1,2,3}/trip_point_0_temp</pre>

> <pre>30000</pre>

<div>Set the fan speed for each trip point.</div>

> You always have to reboot to apply the changed fan speed for now.

<pre>echo "0 204 220 240" > /sys/devices/platform/pwm-fan/hwmon/hwmon0/fan_speed
cat /sys/devices/platform/pwm-fan/hwmon/hwmon0/fan_speed</pre>

> <pre>0 120 180 240</pre>

### Set fan temps and speed at boot

Write your settings in the /etc/rc.local file.

<pre>    # Target temperature: 30°C, 50°C, 70°C
    TRIP_POINT_0=30000
    TRIP_POINT_1=50000
    TRIP_POINT_2=70000

    echo "0 204 220 240" > /sys/devices/platform/pwm-fan/hwmon/hwmon0/fan_speed

    echo $TRIP_POINT_0 > /sys/devices/virtual/thermal/thermal_zone0/trip_point_0_temp
    echo $TRIP_POINT_0 > /sys/devices/virtual/thermal/thermal_zone1/trip_point_0_temp
    echo $TRIP_POINT_0 > /sys/devices/virtual/thermal/thermal_zone2/trip_point_0_temp
    echo $TRIP_POINT_0 > /sys/devices/virtual/thermal/thermal_zone3/trip_point_0_temp

    echo $TRIP_POINT_1 > /sys/devices/virtual/thermal/thermal_zone0/trip_point_1_temp
    echo $TRIP_POINT_1 > /sys/devices/virtual/thermal/thermal_zone1/trip_point_1_temp
    echo $TRIP_POINT_1 > /sys/devices/virtual/thermal/thermal_zone2/trip_point_1_temp
    echo $TRIP_POINT_1 > /sys/devices/virtual/thermal/thermal_zone3/trip_point_1_temp

    echo $TRIP_POINT_2 > /sys/devices/virtual/thermal/thermal_zone0/trip_point_2_temp
    echo $TRIP_POINT_2 > /sys/devices/virtual/thermal/thermal_zone1/trip_point_2_temp
    echo $TRIP_POINT_2 > /sys/devices/virtual/thermal/thermal_zone2/trip_point_2_temp
    echo $TRIP_POINT_2 > /sys/devices/virtual/thermal/thermal_zone3/trip_point_2_temp
    </pre>

<div>Reboot and check if the changes applied.</div>

<pre>cat /sys/devices/virtual/thermal/thermal_zone{0,1,2,3}/trip_point_{0,1,2}_temp</pre>

<pre>cat /sys/devices/platform/pwm-fan/hwmon/hwmon0/fan_speed</pre>

### Emulate temperature

Test fan speeds by covering real temperature values with emulated ones.

<div>Set temperature to 85°C:</div>

<pre>echo 85000 | sudo tee /sys/devices/virtual/thermal/thermal_zone{0,1,2,3}/emul_temp
cat /sys/devices/virtual/thermal/thermal_zone{0,1,2,3}/temp</pre>

> <pre>85000
> 85000
> 85000
> 85000
> 85000
> </pre>

<div>Reset real temperatures</div>

<pre>echo 0 | sudo tee /sys/devices/virtual/thermal/thermal_zone{0,1,2,3}/emul_temp
cat /sys/devices/virtual/thermal/thermal_zone{0,1,2,3}/temp</pre>

> <pre>51000
> 53000
> 56000
> 55000
> </pre>

</div>

<div class="details"><summary>

## System Benchmarks

</summary>

### Sysbench

<pre>apt install sysbench</pre>

<div>Example tests:</div>

*   `sysbench cpu help`
*   `sysbench memory help`
*   `sysbench fileio help`
*   `sysbench cpu --threads=8 --cpu-max-prime=10000 run`
*   `sysbench memory --threads=8 run`

> <pre>    CPU speed:
>     events per second:   500.60
>
>     General statistics:
>     total time:                          10.0139s
>     total number of events:              5016
>
>     Latency (ms):
>     min:                                    9.58
>     avg:                                   15.96
>     max:                                   44.14
>     95th percentile:                       28.16
>     sum:                                80042.91
>
>     Threads fairness:
>     events (avg/stddev):           627.0000/76.37
>     execution time (avg/stddev):   10.0054/0.01
>     </pre>

### systemd-analyze

<div>Will plot a detailed graphic with the boot sequence: kernel time, userspace time, time taken by each service.</div>

<pre>systemd-analyze plot > boot.svg</pre>

### speedtest-cli

<pre>sudo apt install speedtest-cli</pre>

<pre>speedtest-cli</pre>

> <pre>Retrieving speedtest.net server list...
> Selecting best server based on ping...
> Testing download speed...........................................................
> Download: 64.38 Mbit/s
> Testing upload speed.............................................................
> Upload: 18.60 Mbit/s
>     </pre>

</div>

<div class="details"><summary>

## Common Tools

</summary>

### Golang

Install the GO programming language:

<pre>sudo apt-get install -y golang-go</pre>

### Docker

EXAMPLE:

<pre>CODE BIP BOP</pre>

> Output:
>
> <pre>RESPONSE</pre>

INFO

<pre>GLOMP</pre>

### Python

### Java

</div>

<!--const displayInput = () => { data.forEach((row) => row.forEach(({ fieldName }) => { const el = document.getElementById(fieldName); [...document.getElementsByClassName(fieldName)].forEach((e) => (e.innerHTML = (el.value || el.placeholder).trim())); localStorage.setItem(fieldName, el.value); }) ); }; document.addEventListener("DOMContentLoaded", function () { const container = document.querySelector("#controlsTable"); data.forEach((row) => { const tr = container.appendChild(document.createElement("tr")); row.forEach((cell) => { let value = localStorage.getItem(cell.fieldName) || ""; tr.innerHTML += `<td>${cell.label}:<br><input type="text" value="${value}" placeholder="${cell.placeHolder}" id="${cell.fieldName}" oninput="displayInput()"></td>`; }); }); displayInput(); document.querySelectorAll(".details>summary").forEach((s, i) => { const details = s.parentNode; const key = `details-${i}`; details.tabIndex = 0 details.classList.add("notransition") setTimeout(() => details.classList.remove("notransition"), 10); details.classList.toggle("Close", localStorage.getItem(key) === "Close"); s.addEventListener("click", () => { details.classList.toggle("Close"); localStorage.setItem(key, details.classList.contains("Close") ? "Close" : ""); }); details.addEventListener("keyup", (event) => { if (event.code === 'Enter') { details.classList.toggle("Close"); localStorage.setItem(key, details.classList.contains("Close") ? "Close" : ""); } }); }); });--> <!--document.getElementById("scriptGenerated").style.display = "block"; document.addEventListener("DOMContentLoaded", function () { const tocList = document.getElementById("TOC"); const headers = document.querySelectorAll("h2, h3, h4, h5, h6"); let currentList = tocList; // start with the top-level list headers.forEach(header => { // create a new list item for this header const newListItem = document.createElement("li"); header.id = header.id || header.textContent.replace(/\W+/g, '-').toLowerCase(); const newLink = newListItem.appendChild(document.createElement("a")); newLink.textContent = header.textContent; newLink.href = `#${header.id}`; // determine the level of this header (e.g. h2 is level 2) const level = parseInt(header.tagName.charAt(1)); // find the appropriate parent list for this header while (currentList.getAttribute("data-level") >= level && currentList.parentNode.tagName === "OL") { currentList = currentList.parentNode; } const newSublist = document.createElement("ol"); newSublist.setAttribute("data-level", level); currentList.appendChild(newListItem); currentList.appendChild(newSublist); currentList = newSublist }); });--><style>body { background-color: #111; color: #ddd; font-family: sans-serif; margin: 1px; } .container { padding-left: 2ch; margin-bottom: 3ch; } .details { border: #6b6464 1px solid; margin-bottom: 2ch; border-radius: 1ch; overflow: hidden } .details > summary { user-select: none; transition: all 0.3s; cursor: pointer; z-index: 2; border: 4px solid transparent; outline: none; background: #444; color: white; position: relative; border-radius: 1ch; margin-bottom: 1ch; display: flex; align-items: center; } .details > summary:before { content: ''; border-width: 0.5rem; margin-left: 1ch; border-style: solid; border-color: transparent transparent transparent #fff; transform: rotate(90deg); transform-origin: 25% 50%; transition: .25s transform ease; } .details.Close > summary:before { transform: rotate(0deg); } .details summary ~ *,.details summary ~ * * { transition: all 0.3s linear; margin-left: 1ch; margin-right: 1ch; opacity: 1; } .details.Close summary ~ *,.details.Close summary ~ * *{ display: block; opacity: 0; margin: 0 0 0 1ch; padding-top: 0; padding-bottom: 0; max-height: 0; overflow: hidden; line-height: 0; } .notransition > * ,.notransition > * *{ -webkit-transition: none !important; -moz-transition: none !important; -o-transition: none !important; transition: none !important; } h2 { text-align: center; display: flex; align-content: center; align-items: center; white-space: nowrap; width: 100%; } h2::before, h2::after { content: ""; height: 2px; background: white; margin: 0 2rem; width: 100%; } summary p { padding-left: 3ch; margin-top: .5ch; margin-bottom: 0.5ch; } blockquote { background: rgba(0, 99, 132, 0.31); border-left: 3px solid darkturquoise; padding: 0.1em 0.1em 0.1em 0.5em; margin-left: 0 !important; margin-right: 0 !important; } blockquote.warn{ padding: 0.5em 0.5em 0.5em 1em; background: rgba(255, 178, 89, 0.11); border-left: 3px solid #ffa142; } code { background-color: #333; color: #fff; font-family: monospace; border-radius: 0.5em; padding: 0.8ch; } pre { background-color: #333; color: #fff; font-family: monospace; padding: 1ch; overflow: auto; border-radius: 0.5em; } th, td { padding: 0.5ch 1ch 0.5ch; background: #4b4b4b; } li { margin: 0 0 1ch 0; } .toc-container ol { counter-reset: item; list-style-type: none; } .toc-container ol>li { counter-increment: item; } .toc-container ol>li::before { content: counters(item, '.') ' - '; } .toc-container a, .toc-container a:visited { color: ghostwhite; transition: color 0.2s cubic-bezier(0,.7,.07,.42) } .toc-container a:hover { color: dodgerblue; }</style>