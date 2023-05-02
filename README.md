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

`user/passwd: odroid/odroid`  
`root/odroid`

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

    sudo su
    nmcli radio wifi on
    nmcli radio wifi

    nmcli dev wifi list  

> Output:
>
>     enabled
>
>     
>                 IN-USE
>                 BSSID
>                 SSID
>                 MODE
>                 CHAN
>                 RATE
>                 SIGNAL
>                 BARS
>                 SECURITY
>             
>             
>             
>             
>                 *
>                 ZZ:ZZ:QE:34:05:RQ
>                 Network-name
>                 Infra
>                 77
>                 111 Mbit/s
>                 95
>                 ****
>                 WPA2
>             
>             
>                 
>                 ZZ:ZZ:SS:CD:TT:FW
>                 ANother-Ntwk
>                 Infra
>                 44
>                 222 Mbit/s
>                 92
>                 ****
>                 WPA2
>             
>             
>                 
>                 ZZ:ZZ:CC:RR:1S:BB
>                 blipBloppers
>                 Infra
>                 63
>                 333 Mbit/s
>                 59
>                 ***
>                 WPA2

### Interactive network options

Network Manager offers a tui interface:

    nmtui

> ┌─┤ NetworkManager TUI ├──┐        ┌───────────────────────────────────────────────┐
>         │                         │        │                                               │
>         │ Please select an option │        │                                               │
>         │                         │        │ ┌──────────────────────────────┐              │
>         │ Edit a connection       │        │ │ Wired                        │ [Activate]   │
>         │ Activate a connection   │        │ │ * Wired connection 1       ▒ │              │
>         │ Set system hostname     │        │ │                            ▒ │              │
>         │                         │        │ │ Wi-Fi                      ▒ │              │
>         │ Quit                    │        │ │   Network-wifi       ***   ▒ │              │
>         │                         │        │ │   Neighbr-ntwk32     ****  ▒ │              │
>         │                    [OK] │        │ │                            ▒ │              │
>         │                         │        │ │                            ▒ │              │
>         └─────────────────────────┘        │ │                            ▒ │              │

### Command line network options

> Warning: these commands will display your Wi-Fi password in the terminal, and might leak it in the command history. Use the interactive tools if possible.

Command line connection:

    nmcli device wifi connect network_ssid password network_pswd

Connect with a static ip:

    nmcli con add con-name "network_ssid" type wifi ifname wlan0 ssid "network_ssid" -- wifi-sec.key-mgmt wpa-psk wifi-sec.psk "network_pswd" ipv4.method manual ipv4.address YOUR IP
    nmcli con up "network_ssid"

> Connection 'network_ssid' (12312323-3333-3333-3333-asd222sss2222) successfully added.
>     Connection successfully activated (D-Bus active path: /org/freedesktop/NetworkManager/ActiveConnection/4)

### Delete a connection

    nmcli connection delete "network_ssid"

> Connection 'network_ssid' (123123123-1233-3211-qwee-42124234d) successfully deleted.

### Change connection priority

    nmcli connection modify "Wired connection 1" connection.autoconnect-priority 10
    nmcli -f NAME,UUID,AUTOCONNECT,AUTOCONNECT-PRIORITY c

> Name
>         UUID
>         Autoconnect
>         Autoconnect-priority
>
>
>         Wired connection 1
>         12312323-4444-4444-4444-123123123123
>         yes
>         10

### Verify your connections

    nmcli connection show
    ip a |grep inet

> Output:
>
>
>
>
>                 NAME
>                 UUID
>                 TYPE
>                 DEVICE
>             
>             
>             
>             
>                 Wired connection 1
>                 55555555-1111-2222-3333-444444444444
>                 ethernet
>                 eth0
>             
>             
>                 network_ssid
>                 55555555-1111-2222-3333-444444444444
>                 wlan0
>                 Infra
>                               
>
>     inet 192.168.1.111/24 brd 192.168.1.255 scope global dynamic noprefixroute eth0      <--Ethernet Adapter
>     inet YOUR IP brd 192.168.1.255 scope global noprefixroute wlan0              <--Wifi Adapter

> If you see a connection marked as dynamic, the IP might change unpredictably, e.g. after reboots, and you will not be able to ssh in until you figure out the new IP!

Verify internet access:

    ping google.com

> 64 bytes from mil04s43-in-f14.1e100.net (142.250.180.142): icmp_seq=1 ttl=115 time=7.66 ms
>         64 bytes from mil04s43-in-f14.1e100.net (142.250.180.142): icmp_seq=2 ttl=115 time=7.60 ms
>         ...

### Set host name

The system hostname is used to identify a machine in a network in a human-readable format.

View current hostname:

    hostname

> hostname odroid

Change hostname:

    sudo hostnamectl set-hostname new_hostname
    hostname

> hostname new_hostname

</div>

<div class="details"><summary>

## Network Info

</summary>

### Network Managers

View available network managers:

    systemctl --type=service | grep --ignore-case 'network'

> NetworkManager.service                         loaded active running Network Manager
>         systemd-networkd-wait-online.service           loaded active exited  Wait for Network to be Configured
>         ...

### ip

Show addresses assigned to all network interfaces:

    ip a

More concise output:

     ip a | grep "inet "

> inet 127.0.0.1/8 scope host lo
>         inet 192.168.1.111/24 brd 192.168.1.255 scope global dynamic noprefixroute eth0
>         inet 192.168.1.222/24 brd 192.168.1.255 scope global noprefixroute wlan0

Get connection statistics:

    ip -s link

> Interface
>         Description
>         MTU
>         State
>         Mode
>         Group
>         Qlen
>         Link/ether
>         Broadcast
>
>
>
>
>         eth0
>         BROADCAST,MULTICAST,UP,LOWER_UP
>         1500
>         fq_codel state UP
>         DEFAULT
>         default
>         1000
>         00:1e:06:37:42:ff
>         ff:ff:ff:ff:ff:ff
>       
>       
>         
>         RX:
>         bytes
>         packets
>         errors
>         dropped
>         missed
>         mcast
>         
>       
>       
>         
>         
>         1062498
>         6196
>         0
>         57
>         0
>         0
>         
>       
>       
>         
>         TX:
>         bytes
>         packets
>         errors
>         dropped
>         carrier
>         collsns
>         
>       
>       
>         
>         
>         3794910
>         32297
>         0
>         0
>         0
>         0
>         
>           
>       
>       
>         wlan0
>         BROADCAST,MULTICAST,UP,LOWER_UP
>         1500
>         mq state UP
>         DORMANT
>         default
>         1000
>         1c:bf:ce:77:84:74
>         ff:ff:ff:ff:ff:ff
>       
>       
>         
>         RX:
>         bytes
>         packets
>         errors
>         dropped
>         missed
>         mcast
>         
>       
>       
>         
>         
>         3992249
>         33808
>         0
>         24
>         0
>         0
>         
>       
>       
>         
>         TX:
>         bytes
>         packets
>         errors
>         dropped
>         carrier
>         collsns
>         
>       
>       
>         
>         
>         40978
>         416
>         0
>         0
>         0
>         0

### nmcli

nmcli is a command-line tool for controlling NetworkManager and reporting network status.

Get information on devices and connections:

    nmcli

Get information on active connections:

    nmcli -p device

> =====================
>          Status of devices
>         =====================
>         DEVICE  TYPE      STATE      CONNECTION
>         -----------------------------------------------------------------------
>         eth0    ethernet  connected  Wired connection 1
>         wlan0   wifi      connected  network_ssid

See connection priorities:

    nmcli -f NAME,UUID,AUTOCONNECT,AUTOCONNECT-PRIORITY c

> Name
>             UUID
>             Autoconnect
>             Autoconnect-priority
>
>
>             Wired connection 1
>             12312323-4444-4444-4444-123123123123
>             yes
>             -999
>           
>           
>             network_ssid
>             12312323-4444-4444-4444-123123123123
>             yes
>             0

### iwconfig

Iwconfig may be used to display the parameters of the network interface, and the wireless statistics.

    iwconfig

> lo        no wireless extensions.
>
>         wlan0     IEEE 802.11  ESSID:"network_ssid"
>         Mode:Managed  Frequency:2.462 GHz  Access Point: 11:22:33:44:55:66
>         Bit Rate=65 Mb/s   Tx-Power=1744 dBm
>         Retry short limit:4   RTS thr:off   Fragment thr:off
>         Encryption key:on
>         Power Management:off
>         Link Quality=67/70  Signal level=43 dBm
>         Rx invalid nwid:0  Rx invalid crypt:0  Rx invalid frag:0
>         Tx excessive retries:1  Invalid misc:199   Missed beacon:0
>     
>         eth0      no wireless extensions.

</div>

<div class="details"><summary>

## User Management

</summary>

List all users:

    sudo su
    cut -d: -f1 /etc/passwd

### Interactive user creation

Create a new user named <ins class="usr-name">user</ins> with sudo privileges:

    sudo su	
    adduser user

> You will be asked a few information
>
>     Creating home directory `/home/user' ...
>     Copying files from `/etc/skel' ...
>     New password		<---type here
>     Retype new password:	<---type here
>     passwd: password updated successfully
>     Changing the user information for gigi
>     Enter the new value, or press ENTER for the default
>             Full Name []:
>             Room Number []:
>             Work Phone []:
>             Home Phone []:
>             Other []:
>     Is the information correct? [Y/n] y

Bestow sudo privileges to the new user.

    usermod -aG sudo user

### Automated user creation

> Warning, this method might display your password on the screen and leak it through the command history.  
> Please follow the interactive method instead.

Create a new user named <ins class="usr-name">user</ins>:

    useradd -m user
      echo "user:pswd" | chpasswd;history -d $(history 1)
    #↑ The leading space is important, don't remove it!
    clear
    history | tail

> Warning, if your password was displayed, change it immediately.
>
>     183  useradd -m User
>     184  "user:pswd" | chpasswd
>     185  history | tail

Bestow sudo privileges to the new user.

    usermod -aG sudo user

### Remove default user

Delete the user "odroid" and remove their home directory:

    sudo deluser --remove-home odroid

</div>

<div class="details"><summary>

## Security

</summary>

### SSH Configuration

Disable root login:

    sed -i 's/PermitRootLogin yes/PermitRootLogin no/g'  /etc/ssh/sshd_config

<div>or</div>

    sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin no/g'  /etc/ssh/sshd_config

Verify that root login is disabled:

    cat  /etc/ssh/sshd_config | grep "PermitRootLogin "

> Output:
>
>     PermitRootLogin no

Restart the SSH service:

    sudo systemctl restart sshd 

### Firewall Setup

> Make sure you are not logged in as root, as this will prevent remote connections to root

    sudo su
    apt install ufw
    ufw allow OpenSSH
    ufw enable
    ufw status

> Output:
>
>
>
>
>           To
>           Action
>           From
>         
>       
>       
>         
>           OpenSSH
>           ALLOW
>           Anywhere
>         
>         
>           OpenSSH (v6)
>           ALLOW
>           Anywhere (v6)

</div>

<div class="details"><summary>

## Update OS

</summary>

### Update and Upgrade

Update the OS by running the following command in a terminal:

    sudo apt update -y && sudo apt upgrade -y && sudo apt dist-upgrade -y && sudo apt autoremove -y

### Automated System Updates and Reboots

<div>Enable automatic system updates and reboots</div>

    sudo su
    sudo apt install unattended-upgrades 

<div>Configure custom settings</div>

    echo '
    // CUSTOM SETTINGS
    Unattended-Upgrade::Mail "your@mail.com";
    Unattended-Upgrade::Automatic-Reboot "true";
    Unattended-Upgrade::Automatic-Reboot-Time 02:00";
    Unattended-Upgrade::Remove-Unused-Kernel-Packages “true”;
    Unattended-Upgrade::Remove-New-Unused-Dependencies “true”;
    ' >> /etc/apt/apt.conf.d/50unattended-upgrades
    cat /etc/apt/apt.conf.d/50unattended-upgrades

<div>Execute test run with verbose output</div>

    sudo unattended-upgrade -v -d 

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

    echo 250 >  /sys/class/leds/blue\:heartbeat/brightness
    cat  /sys/class/leds/blue\:heartbeat/brightness

> 250

<div>See available led modes</div>

    cat /sys/devices/platform/pwmleds/leds/blue\:heartbeat/trigger

> none rc-feedback kbd-scrolllock kbd-numlock kbd-capslock kbd-kanalock kbd-shiftlock kbd-altgrlock kbd-ctrllock kbd-altlock kbd-shiftllock kbd-shiftrlock kbd-ctrlllock kbd-ctrlrlock mmc0 mmc1 timer oneshot heartbeat gpio cpu cpu0 cpu1 cpu2 cpu3 cpu4 cpu5 cpu6 cpu7 [default-on] transient rfkill-any rfkill-none phy0rx phy0tx phy0assoc phy0radio rfkill0

<div>Turn OFF, ON, Heartbeat (original state)</div>

    echo none > /sys/class/leds/blue\:heartbeat/trigger
    echo default-on > /sys/class/leds/blue\:heartbeat/trigger
    echo heartbeat > /sys/class/leds/blue\:heartbeat/trigger

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

    echo 0 > /sys/devices/platform/pwm-fan/hwmon/hwmon0/automatic
    echo 10 > /sys/devices/platform/pwm-fan/hwmon/hwmon0/pwm1

<div>Verify</div>

    cat /sys/devices/platform/pwm-fan/hwmon/hwmon0/automatic
    cat /sys/devices/platform/pwm-fan/hwmon/hwmon0/pwm1

> 1
>     120

### Control automatically

<div>Enable automatic control</div>

    echo 1 > /sys/devices/platform/pwm-fan/hwmon/hwmon0/automatic
    cat /sys/devices/platform/pwm-fan/hwmon/hwmon0/automatic

> 1

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

    cat /sys/devices/virtual/thermal/thermal_zone{0,1,2,3}/trip_point_{0,1,2}_temp

> 60000   <------ Zone 0
>     70000
>     80000
>     60000   <------ Zone 1
>     70000
>     80000
>     60000   <------ Zone 2
>     70000
>     80000
>     60000   <------ Zone 3
>     70000
>     80000

<div>Set trip point 1 to be activated at 30°C.</div>

    echo 30000 | sudo tee /sys/devices/virtual/thermal/thermal_zone{0,1,2,3}/trip_point_0_temp
    cat /sys/devices/virtual/thermal/thermal_zone{0,1,2,3}/trip_point_0_temp

> 30000

<div>Set the fan speed for each trip point.</div>

> You always have to reboot to apply the changed fan speed for now.

    echo "0 204 220 240" > /sys/devices/platform/pwm-fan/hwmon/hwmon0/fan_speed
    cat /sys/devices/platform/pwm-fan/hwmon/hwmon0/fan_speed

> 0 120 180 240

### Set fan temps and speed at boot

Write your settings in the /etc/rc.local file.

        # Target temperature: 30°C, 50°C, 70°C
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

<div>Reboot and check if the changes applied.</div>

    cat /sys/devices/virtual/thermal/thermal_zone{0,1,2,3}/trip_point_{0,1,2}_temp

    cat /sys/devices/platform/pwm-fan/hwmon/hwmon0/fan_speed

### Emulate temperature

Test fan speeds by covering real temperature values with emulated ones.

<div>Set temperature to 85°C:</div>

   <pre><code> echo 85000 | sudo tee /sys/devices/virtual/thermal/thermal_zone{0,1,2,3}/emul_temp
    cat /sys/devices/virtual/thermal/thermal_zone{0,1,2,3}/temp </code></pre>

>   <pre><code>  85000
>     85000
>     85000
>     85000
>     85000   </code></pre>

<div>Reset real temperatures</div>

    echo 0 | sudo tee /sys/devices/virtual/thermal/thermal_zone{0,1,2,3}/emul_temp
    cat /sys/devices/virtual/thermal/thermal_zone{0,1,2,3}/temp

>     ``` 51000
>     53000
>     56000
>     55000  ```

</div>

<div class="details"><summary>

## System Benchmarks

</summary>

### Sysbench

    apt install sysbench

<div>Example tests:</div>

*   `sysbench cpu help`
*   `sysbench memory help`
*   `sysbench fileio help`
*   `sysbench cpu --threads=8 --cpu-max-prime=10000 run`
*   `sysbench memory --threads=8 run`

> CPU speed:
>         events per second:   500.60
>
>         General statistics:
>         total time:                          10.0139s
>         total number of events:              5016
>     
>         Latency (ms):
>         min:                                    9.58
>         avg:                                   15.96
>         max:                                   44.14
>         95th percentile:                       28.16
>         sum:                                80042.91
>     
>         Threads fairness:
>         events (avg/stddev):           627.0000/76.37
>         execution time (avg/stddev):   10.0054/0.01

### systemd-analyze

<div>Will plot a detailed graphic with the boot sequence: kernel time, userspace time, time taken by each service.</div>

    systemd-analyze plot > boot.svg

### speedtest-cli

    sudo apt install speedtest-cli

    speedtest-cli

> Retrieving speedtest.net server list...
>     Selecting best server based on ping...
>     Testing download speed...........................................................
>     Download: 64.38 Mbit/s
>     Testing upload speed.............................................................
>     Upload: 18.60 Mbit/s

</div>

<div class="details"><summary>

## Common Tools

</summary>

### Golang

Install the GO programming language:

    sudo apt-get install -y golang-go

### Docker

EXAMPLE:

    CODE BIP BOP

> Output:
>
>     RESPONSE

INFO

    GLOMP

### Python

### Java

</div>

<!--const displayInput = () => { data.forEach((row) => row.forEach(({ fieldName }) => { const el = document.getElementById(fieldName); [...document.getElementsByClassName(fieldName)].forEach((e) => (e.innerHTML = (el.value || el.placeholder).trim())); localStorage.setItem(fieldName, el.value); }) ); }; document.addEventListener("DOMContentLoaded", function () { const container = document.querySelector("#controlsTable"); data.forEach((row) => { const tr = container.appendChild(document.createElement("tr")); row.forEach((cell) => { let value = localStorage.getItem(cell.fieldName) || ""; tr.innerHTML += `<td>${cell.label}:<br><input type="text" value="${value}" placeholder="${cell.placeHolder}" id="${cell.fieldName}" oninput="displayInput()"></td>`; }); }); displayInput(); document.querySelectorAll(".details>summary").forEach((s, i) => { const details = s.parentNode; const key = `details-${i}`; details.tabIndex = 0 details.classList.add("notransition") setTimeout(() => details.classList.remove("notransition"), 10); details.classList.toggle("Close", localStorage.getItem(key) === "Close"); s.addEventListener("click", () => { details.classList.toggle("Close"); localStorage.setItem(key, details.classList.contains("Close") ? "Close" : ""); }); details.addEventListener("keyup", (event) => { if (event.code === 'Enter') { details.classList.toggle("Close"); localStorage.setItem(key, details.classList.contains("Close") ? "Close" : ""); } }); }); });--> <!--document.getElementById("scriptGenerated").style.display = "block"; document.addEventListener("DOMContentLoaded", function () { const tocList = document.getElementById("TOC"); const headers = document.querySelectorAll("h2, h3, h4, h5, h6"); let currentList = tocList; // start with the top-level list headers.forEach(header => { // create a new list item for this header const newListItem = document.createElement("li"); header.id = header.id || header.textContent.replace(/\W+/g, '-').toLowerCase(); const newLink = newListItem.appendChild(document.createElement("a")); newLink.textContent = header.textContent; newLink.href = `#${header.id}`; // determine the level of this header (e.g. h2 is level 2) const level = parseInt(header.tagName.charAt(1)); // find the appropriate parent list for this header while (currentList.getAttribute("data-level") >= level && currentList.parentNode.tagName === "OL") { currentList = currentList.parentNode; } const newSublist = document.createElement("ol"); newSublist.setAttribute("data-level", level); currentList.appendChild(newListItem); currentList.appendChild(newSublist); currentList = newSublist }); });--><style>body { background-color: #111; color: #ddd; font-family: sans-serif; margin: 1px; } .container { padding-left: 2ch; margin-bottom: 3ch; } .details { border: #6b6464 1px solid; margin-bottom: 2ch; border-radius: 1ch; overflow: hidden } .details > summary { user-select: none; transition: all 0.3s; cursor: pointer; z-index: 2; border: 4px solid transparent; outline: none; background: #444; color: white; position: relative; border-radius: 1ch; margin-bottom: 1ch; display: flex; align-items: center; } .details > summary:before { content: ''; border-width: 0.5rem; margin-left: 1ch; border-style: solid; border-color: transparent transparent transparent #fff; transform: rotate(90deg); transform-origin: 25% 50%; transition: .25s transform ease; } .details.Close > summary:before { transform: rotate(0deg); } .details summary ~ *,.details summary ~ * * { transition: all 0.3s linear; opacity: 1; } .details.Close summary ~ *,.details.Close summary ~ * *{ display: block; opacity: 0; margin: 0 0 0 1ch; padding-top: 0; padding-bottom: 0; max-height: 0; overflow: hidden; line-height: 0; } .notransition > * ,.notransition > * *{ -webkit-transition: none !important; -moz-transition: none !important; -o-transition: none !important; transition: none !important; } h2 { text-align: center; display: flex; align-content: center; align-items: center; white-space: nowrap; width: 100%; } h2::before, h2::after { content: ""; height: 2px; background: white; margin: 0 2rem; width: 100%; } summary p { padding-left: 3ch; margin-top: .5ch; margin-bottom: 0.5ch; } blockquote { background: rgba(0, 99, 132, 0.31); border-left: 3px solid darkturquoise; padding: 0.1em 0.1em 0.1em 0.5em; margin-left: 0 !important; margin-right: 0 !important; } blockquote.warn{ padding: 0.5em 0.5em 0.5em 1em; background: rgba(255, 178, 89, 0.11); border-left: 3px solid #ffa142; } code { background-color: #333; color: #fff; font-family: monospace; border-radius: 0.5em; padding: 0.8ch; line-height: 4ch; } pre code { background: none; color: #fff; font-family: monospace; border: none; padding: 0ch; line-height: 1ch; } pre { background-color: #333; color: #fff; font-family: monospace; padding: 1ch; overflow: auto; border-radius: 0.5em; } th, td { padding: 0.5ch 1ch 0.5ch; background: #4b4b4b; } li { margin: 0 0 1ch 0; } .toc-container ol { counter-reset: item; list-style-type: none; } .toc-container ol>li { counter-increment: item; } .toc-container ol>li::before { content: counters(item, '.') ' - '; } .toc-container a, .toc-container a:visited { color: ghostwhite; transition: color 0.2s cubic-bezier(0,.7,.07,.42) } .toc-container a:hover { color: dodgerblue; }</style>