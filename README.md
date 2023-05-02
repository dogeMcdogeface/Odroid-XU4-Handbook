<div class="container">
<h1>XU4 Handbook 25/04/2023</h1>
<p>This is a collection of useful scripts, commands and procedures for setting up and controlling an Odroid XU4, an ARM based SBC</p>
<p>Some features might not be available if you are reading the markdown version, or without js enabled.</p>
</div>
<div id="scriptGenerated" style="display:none;">
<div  class="details toc-container notransition">
<summary><p>Table of Contents</p></summary>
<ol id="TOC"></ol>
</div>
<br>
<h3>Guide Settings</h3>
<p>Auto fill-in tutorial commands:</p>
<table id="controlsTable"></table>
<br>


</div>


<!-------------------------------------------- GUIDE BODY ------------------------------------------------------------->
<!--------------------------------------------- FLASH OS --------------------------------------------->
<div class="details">
<summary>
<h2>Flash OS</h2>
</summary>
<p><a href="https://wiki.odroid.com/odroid-xu4/getting_started/os_installation_guide#tab__odroid-xu4">Official OS Installation Guide</a>
Power supply minimum: 5V/2A recommended: 5V/6A</p>
<br>
<p><a href="https://odroid.in/ubuntu_22.04lts/XU3_XU4_MC1_HC1_HC2/ubuntu-22.04-5.4-mate-odroid-xu4-20220522.img.xz">Ubuntu MATE 22.04 desktop image.</a>
At least 8 GB is recommended <br><br> <code>user/passwd: odroid/odroid </code> <br> <code>root/odroid</code> <br></p>
<br>
<p><a href="https://odroid.in/ubuntu_22.04lts/XU3_XU4_MC1_HC1_HC2/ubuntu-22.04-5.4-minimal-odroid-xu4-20220721.img.xz">Ubuntu Minimal 22.04 image.</a>
At least 4 GB is recommended <br><br><code>user/passwd: root/odroid</code> <br></p>
<br>
<ol>
<li>Download an OS image
</li>
<li>Download Etcher</li>
<li>Open Etcher.</li>
<li>Select the downloaded OS image.</li>
<li>Select the inserted memory card. (Normally, the memory card is detected automatically.)</li>
<li>Click Flash</li>
</ol>

</div>
<!----------------------------------------------- WIFI ----------------------------------------------->
<div class="details">
<summary>
<h2>Connect to Wi-Fi</h2>
</summary>
<p>If you don't have access to an ethernet connection, you will need a wireless adaptor (e.g. an usb dongle) to ssh into the machine. <br>
The easiest way to interact with the machine without a network connection will be to connect a monitor and keyboard and use the terminal. <br>
This guide assumes a minimal installation with no gui, but network manager installed.</p>
<h3>Enable Wi-Fi</h3>

<pre><code>sudo su
nmcli radio wifi on
nmcli radio wifi

nmcli dev wifi list  </code></pre>
<blockquote>
<p>Output:</p>
<pre><code>enabled</code></pre>

<pre><code><table><thead><tr>
<th>IN-USE</th>
<th>BSSID</th>
<th>SSID</th>
<th>MODE</th>
<th>CHAN</th>
<th>RATE</th>
<th>SIGNAL</th>
<th>BARS</th>
<th>SECURITY</th>
</tr>
</thead>
<tbody>
<tr>
<td>*</td>
<td>ZZ:ZZ:QE:34:05:RQ</td>
<td class="net-ssid">Network-name</td>
<td>Infra</td>
<td>77</td>
<td>111 Mbit/s</td>
<td>95</td>
<td>****</td>
<td>WPA2</td>
</tr>
<tr>
<td></td>
<td>ZZ:ZZ:SS:CD:TT:FW</td>
<td>ANother-Ntwk</td>
<td>Infra</td>
<td>44</td>
<td>222 Mbit/s</td>
<td>92</td>
<td>****</td>
<td>WPA2</td>
</tr>
<tr>
<td></td>
<td>ZZ:ZZ:CC:RR:1S:BB</td>
<td>blipBloppers</td>
<td>Infra</td>
<td>63</td>
<td>333 Mbit/s</td>
<td>59</td>
<td>***</td>
<td>WPA2</td>
</tr></tbody></table></code></pre>
</blockquote>
<h3>Interactive network options</h3>
<p>Network Manager offers a tui interface:</p>
<pre><code>nmtui</code></pre>
<blockquote><pre><code>
┌─┤ NetworkManager TUI ├──┐        ┌───────────────────────────────────────────────┐
│                         │        │                                               │
│ Please select an option │        │                                               │
│                         │        │ ┌──────────────────────────────┐              │
│ Edit a connection       │        │ │ Wired                        │ [Activate]   │
│ Activate a connection   │        │ │ * Wired connection 1       ▒ │              │
│ Set system hostname     │        │ │                            ▒ │              │
│                         │        │ │ Wi-Fi                      ▒ │              │
│ Quit                    │        │ │   Network-wifi       ***   ▒ │              │
│                         │        │ │   Neighbr-ntwk32     ****  ▒ │              │
│                    [OK] │        │ │                            ▒ │              │
│                         │        │ │                            ▒ │              │
└─────────────────────────┘        │ │                            ▒ │              │

</code></pre></blockquote>



<h3>Command line network options</h3>
<blockquote class="warn"><p>Warning: these commands will display your Wi-Fi password in the terminal, and might leak it in the command history. Use the interactive tools if possible.</p></blockquote>
<p>Command line connection:</p>
<pre><code>nmcli device wifi connect <ins class="net-ssid">network_ssid</ins> password <ins class="net-pswd">network_pswd</ins></code></pre>
<p>Connect with a static ip:</p>
<pre><code>nmcli con add con-name "<ins class="net-ssid">network_ssid</ins>" type wifi ifname wlan0 ssid "<ins class="net-ssid">network_ssid</ins>" -- wifi-sec.key-mgmt wpa-psk wifi-sec.psk "<ins class="net-pswd">network_pswd</ins>" ipv4.method manual ipv4.address <ins class="net-ip">YOUR IP</ins>
nmcli con up "<ins class="net-ssid">network_ssid</ins>"</code></pre>
<blockquote><pre><code>Connection '<ins class="net-ssid">network_ssid</ins>' (12312323-3333-3333-3333-asd222sss2222) successfully added.
Connection successfully activated (D-Bus active path: /org/freedesktop/NetworkManager/ActiveConnection/4)</code></pre></blockquote>
<h3>Delete a connection</h3>
<pre><code>nmcli connection delete "<ins class="net-ssid">network_ssid</ins>"</code></pre>
<blockquote><pre><code>Connection '<ins class="net-ssid">network_ssid</ins>' (123123123-1233-3211-qwee-42124234d) successfully deleted.</code></pre></blockquote>
<h3>Change connection priority</h3>
<pre><code>nmcli connection modify "Wired connection 1" connection.autoconnect-priority 10
nmcli -f NAME,UUID,AUTOCONNECT,AUTOCONNECT-PRIORITY c</code></pre>
<blockquote><pre><code><table>
<tr>
<th>Name</th>
<th>UUID</th>
<th>Autoconnect</th>
<th>Autoconnect-priority</th>
</tr>
<tr>
<td>Wired connection 1</td>
<td>12312323-4444-4444-4444-123123123123</td>
<td>yes</td>
<td>10</td>
</tr>
</table></code></pre></blockquote>
<h3>Verify your connections</h3>
<pre><code>nmcli connection show
ip a |grep inet</code></pre>
<blockquote>
<p>Output:</p>

<pre><code><table>
<thead>
<tr>
<th>NAME</th>
<th>UUID</th>
<th>TYPE</th>
<th>DEVICE</th>
</tr>
</thead>
<tbody>
<tr>
<td>Wired connection 1</td>
<td>55555555-1111-2222-3333-444444444444</td>
<td>ethernet</td>
<td>eth0</td>
</tr>
<tr>
<td><ins class="net-ssid">network_ssid</ins></td>
<td>55555555-1111-2222-3333-444444444444</td>
<td>wlan0</td>
<td>Infra</td>
</tr></tbody></table></code></pre>
<pre><code>inet <mark>192.168.1.111/24</mark> brd 192.168.1.255 scope global <mark>dynamic</mark> noprefixroute eth0      <--Ethernet Adapter
inet <mark><ins class="net-ip">YOUR IP</ins></mark> brd 192.168.1.255 scope global noprefixroute wlan0              <--Wifi Adapter</code></pre>
</blockquote>
<blockquote class="warn"><p>If you see a connection marked as dynamic, the IP might change unpredictably, e.g. after reboots, and you will not be able to ssh in until you figure out the new IP!</p></blockquote>
<p>Verify internet access:</p>
<pre><code>ping google.com</code></pre>
<blockquote><pre><code>
64 bytes from mil04s43-in-f14.1e100.net (142.250.180.142): icmp_seq=1 ttl=115 time=7.66 ms
64 bytes from mil04s43-in-f14.1e100.net (142.250.180.142): icmp_seq=2 ttl=115 time=7.60 ms
...</code></pre></blockquote>
<h3>Set host name</h3>
<p>The system hostname is used to identify a machine in a network in a human-readable format.</p>
<p>View current hostname:</p>
<pre><code>hostname</code></pre>
<blockquote> <pre><code>hostname odroid</code></pre> </blockquote>
<p>Change hostname:</p>
<pre><code>sudo hostnamectl set-hostname <ins class="net-hnme">new_hostname</ins>
hostname</code></pre>
<blockquote><pre><code>hostname <ins class="net-hnme">new_hostname</ins></code></pre></blockquote>

</div>
<!-------------------------------------------- CONNECTIONS ------------------------------------------->
<div class="details">
<summary>
<h2>Network Info</h2>
</summary>
<h3>Network Managers</h3>
<p>View available network managers:</p>
<pre><code>systemctl --type=service | grep --ignore-case 'network'</code></pre>
<blockquote><pre><code>    NetworkManager.service                         loaded active running Network Manager
systemd-networkd-wait-online.service           loaded active exited  Wait for Network to be Configured
...</code></pre></blockquote>
<h3>ip</h3>
<p>Show addresses assigned to all network interfaces:</p>
<pre><code>ip a</code></pre>
<p>More concise output:</p>
<pre><code> ip a | grep "inet "</code></pre>
<blockquote><pre><code>inet 127.0.0.1/8 scope host lo
inet 192.168.1.111/24 brd 192.168.1.255 scope global dynamic noprefixroute eth0
inet 192.168.1.222/24 brd 192.168.1.255 scope global noprefixroute wlan0</code></pre></blockquote>
<p>Get connection statistics:</p>
<pre><code>ip -s link</code></pre>
<blockquote><pre><code><table>
<tr>
<th>Interface</th>
<th>Description</th>
<th>MTU</th>
<th>State</th>
<th>Mode</th>
<th>Group</th>
<th>Qlen</th>
<th>Link/ether</th>
<th>Broadcast</th>
</tr>
<tr>
</tr>
<tr>
<td>eth0</td>
<td>BROADCAST,MULTICAST,UP,LOWER_UP</td>
<td>1500</td>
<td>fq_codel state UP</td>
<td>DEFAULT</td>
<td>default</td>
<td>1000</td>
<td>00:1e:06:37:42:ff</td>
<td>ff:ff:ff:ff:ff:ff</td>
</tr>
<tr>
<td></td>
<td>RX:</td>
<td>bytes</td>
<td>packets</td>
<td>errors</td>
<td>dropped</td>
<td>missed</td>
<td>mcast</td>
<td></td>
</tr>
<tr>
<td></td>
<td></td>
<td>1062498</td>
<td>6196</td>
<td>0</td>
<td>57</td>
<td>0</td>
<td>0</td>
<td></td>
</tr>
<tr>
<td></td>
<td>TX:</td>
<td>bytes</td>
<td>packets</td>
<td>errors</td>
<td>dropped</td>
<td>carrier</td>
<td>collsns</td>
<td></td>
</tr>
<tr>
<td></td>
<td></td>
<td>3794910</td>
<td>32297</td>
<td>0</td>
<td>0</td>
<td>0</td>
<td>0</td>
<td></td>
<tr>
</tr>
<tr>
<td>wlan0</td>
<td>BROADCAST,MULTICAST,UP,LOWER_UP</td>
<td>1500</td>
<td>mq state UP</td>
<td>DORMANT</td>
<td>default</td>
<td>1000</td>
<td>1c:bf:ce:77:84:74</td>
<td>ff:ff:ff:ff:ff:ff</td>
</tr>
<tr>
<td></td>
<td>RX:</td>
<td>bytes</td>
<td>packets</td>
<td>errors</td>
<td>dropped</td>
<td>missed</td>
<td>mcast</td>
<td></td>
</tr>
<tr>
<td></td>
<td></td>
<td>3992249</td>
<td>33808</td>
<td>0</td>
<td>24</td>
<td>0</td>
<td>0</td>
<td></td>
</tr>
<tr>
<td></td>
<td>TX:</td>
<td>bytes</td>
<td>packets</td>
<td>errors</td>
<td>dropped</td>
<td>carrier</td>
<td>collsns</td>
<td></td>
</tr>
<tr>
<td></td>
<td></td>
<td>40978</td>
<td>416</td>
<td>0</td>
<td>0</td>
<td>0</td>
<td>0</td>
<td></td>
</tr></table></code></pre></blockquote>
<h3>nmcli</h3>
<p>nmcli is a command-line tool for controlling NetworkManager and reporting network status.</p>
<p>Get information on devices and connections:</p>
<pre><code>nmcli</code></pre>
<p>Get information on active connections:</p>
<pre><code>nmcli -p device</code></pre>
<blockquote><pre><code>=====================
Status of devices
=====================
DEVICE  TYPE      STATE      CONNECTION
-----------------------------------------------------------------------
eth0    ethernet  connected  Wired connection 1
wlan0   wifi      connected  <ins class="net-ssid">network_ssid</ins></code></pre></blockquote>
<p>See connection priorities:</p>
<pre><code>nmcli -f NAME,UUID,AUTOCONNECT,AUTOCONNECT-PRIORITY c</code></pre>
<blockquote><pre><code><table>
<tr>
<th>Name</th>
<th>UUID</th>
<th>Autoconnect</th>
<th>Autoconnect-priority</th>
</tr>
<tr>
<td>Wired connection 1</td>
<td>12312323-4444-4444-4444-123123123123</td>
<td>yes</td>
<td>-999</td>
</tr>
<tr>
<td><ins class="net-ssid">network_ssid</ins></td>
<td>12312323-4444-4444-4444-123123123123</td>
<td>yes</td>
<td>0</td>
</tr>
</table></code></pre></blockquote>
<h3>iwconfig</h3>
<p>Iwconfig may be used to display the parameters of the network interface, and the wireless statistics. </p>
<pre><code>iwconfig</code></pre>
<blockquote><pre><code>
lo        no wireless extensions.
wlan0     IEEE 802.11  ESSID:"<ins class="net-ssid">network_ssid</ins>"
Mode:Managed  Frequency:2.462 GHz  Access Point: 11:22:33:44:55:66
Bit Rate=65 Mb/s   Tx-Power=1744 dBm
Retry short limit:4   RTS thr:off   Fragment thr:off
Encryption key:on
Power Management:off
Link Quality=67/70  Signal level=43 dBm
Rx invalid nwid:0  Rx invalid crypt:0  Rx invalid frag:0
Tx excessive retries:1  Invalid misc:199   Missed beacon:0
eth0      no wireless extensions.</code></pre></blockquote>



</div>


<!-------------------------------------------- GUIDE BODY -------------------------------------------->
<div class="details">
<summary>
<h2>User Management</h2>
</summary>
<p>List all users:</p>
<pre><code>sudo su
cut -d: -f1 /etc/passwd</code></pre>


<h3>Interactive user creation</h3>
<p>Create a new user named
<ins class="usr-name">user</ins>
with sudo privileges:
</p>
<pre><code>sudo su
adduser <ins class="usr-name">user</ins>
</code></pre>

<blockquote>
<p>You will be asked a few information</p>

<pre><code>Creating home directory `/home/<ins class="usr-name">user</ins>' ...
Copying files from `/etc/skel' ...
New password		<---type here
Retype new password:	<---type here
passwd: password updated successfully
Changing the user information for gigi
Enter the new value, or press ENTER for the default
Full Name []:
Room Number []:
Work Phone []:
Home Phone []:
Other []:
Is the information correct? [Y/n] y
</code></pre>
</blockquote>
<p>Bestow sudo privileges to the new user.</p>
<pre><code>usermod -aG sudo <ins class="usr-name">user</ins></code></pre>


<h3>Automated user creation</h3>

<blockquote class="warn">Warning, this method might display your password on the screen and leak it through the command history.<br>
Please follow the interactive method instead.</blockquote>
<p>Create a new user named
<ins class="usr-name">user</ins>:
</p>
<pre><code>useradd -m <ins class="usr-name">user</ins>
echo "<ins class="usr-name">user</ins>:<ins class="usr-pswd">pswd</ins>" | chpasswd;history -d $(history 1)
#↑ The leading space is important, don't remove it!
clear
history | tail
</code></pre>

<blockquote class="warn">
<p>Warning, if your password was displayed, change it immediately.</p>

<pre><code>183  useradd -m User
184  "<ins class="usr-name">user</ins>:<ins class="usr-pswd">pswd</ins>" | chpasswd
185  history | tail
</code></pre>
</blockquote>

<p>Bestow sudo privileges to the new user.</p>
<pre><code>usermod -aG sudo <ins class="usr-name">user</ins></code></pre>


<h3>Remove default user </h3>
<p>Delete the user "odroid" and remove their home directory:</p>
<pre><code>sudo deluser --remove-home odroid</code></pre>
</div>
<!-------------------------------------------- SSH CONFIG -------------------------------------------->
<div class="details">
<summary>
<h2>Security</h2>
</summary>
<h3>SSH Configuration</h3>
<p>Disable root login:</p>
<pre><code>sed -i 's/PermitRootLogin yes/PermitRootLogin no/g'  /etc/ssh/sshd_config</code></pre>
<div>or</div>
<pre><code>sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin no/g'  /etc/ssh/sshd_config</code></pre>
<p>Verify that root login is disabled:</p>
<pre><code>cat  /etc/ssh/sshd_config | grep "PermitRootLogin "</code></pre>
<blockquote>
<p>Output:</p>
<pre><code>PermitRootLogin no</code></pre>
</blockquote>
<p>Restart the SSH service:</p>
<pre><code>sudo systemctl restart sshd </code></pre>
<h3>Firewall Setup</h3>
<blockquote class="warn">Make sure you are not logged in as root, as this will prevent remote connections to root</blockquote>
<pre><code>sudo su
apt install ufw
ufw allow OpenSSH
ufw enable
ufw status</code></pre>
<blockquote>
<p>Output:</p>
<pre><code><table>
<thead>
<tr>
<th>To</th>
<th>Action</th>
<th>From</th>
</tr>
</thead>
<tbody>
<tr>
<td>OpenSSH</td>
<td>ALLOW</td>
<td>Anywhere</td>
</tr>
<tr>
<td>OpenSSH (v6)</td>
<td>ALLOW</td>
<td>Anywhere (v6)</td>
</tr>
</tbody>
</table></code></pre></blockquote>

</div>
<!-------------------------------------------- UPDATE OS --------------------------------------------->
<div class="details">
<summary>
<h2>Update OS</h2>
</summary>
<h3>Update and Upgrade</h3>
<p>Update the OS by running the following command in a terminal:</p>
<pre><code>sudo apt update -y &amp;&amp; sudo apt upgrade -y &amp;&amp; sudo apt dist-upgrade -y &amp;&amp; sudo apt autoremove -y</code></pre>
<h3>Automated System Updates and Reboots</h3>
<div>Enable automatic system updates and reboots</div>
<pre><code>sudo su
sudo apt install unattended-upgrades </code></pre>
<div>Configure custom settings</div>
<pre><code>echo '
// CUSTOM SETTINGS
Unattended-Upgrade::Mail "<ins class="upd-mail">your@mail.com</ins>";
Unattended-Upgrade::Automatic-Reboot "true";
Unattended-Upgrade::Automatic-Reboot-Time <ins class="upd-hour">02:00</ins>";
Unattended-Upgrade::Remove-Unused-Kernel-Packages “true”;
Unattended-Upgrade::Remove-New-Unused-Dependencies “true”;
' &gt;&gt; /etc/apt/apt.conf.d/50unattended-upgrades
cat /etc/apt/apt.conf.d/50unattended-upgrades
</code></pre>
<div>Execute test run with verbose output</div>
<pre><code>sudo unattended-upgrade -v -d </code></pre>
<ul>
<li><code>Unattended-Upgrade::Mail</code> option specifies the email address to receive notifications about
upgrades.
</li>
<li><code>Unattended-Upgrade::Automatic-Reboot</code> enables automatic reboots after upgrades..</li>
<li><code>Unattended-Upgrade::Automatic-Reboot-Time</code> option sets the time for the automatic reboot to
occur
</li>
<li><code>Unattended-Upgrade::Remove-Unused-Kernel-Packages</code> and <code>Unattended-Upgrade::Remove-New-Unused-Dependencies</code>
options remove any unused packages and dependencies after an upgrade.
</li>
</ul>
</div>
<!--------------------------------------------- HARDWARE --------------------------------------------->
<div class="details">
<summary>
<h2>Hardware Control</h2>
</summary>
<h3>Onboard Led</h3>
<div>Set led brightness</div>
<pre><code>echo 250 >  /sys/class/leds/blue\:heartbeat/brightness
cat  /sys/class/leds/blue\:heartbeat/brightness</code></pre>
<blockquote><pre><code>250</code></pre></blockquote>
<div>See available led modes</div>
<pre><code>cat /sys/devices/platform/pwmleds/leds/blue\:heartbeat/trigger</code></pre>
<blockquote><pre><code>none rc-feedback kbd-scrolllock kbd-numlock kbd-capslock kbd-kanalock kbd-shiftlock kbd-altgrlock kbd-ctrllock kbd-altlock kbd-shiftllock kbd-shiftrlock kbd-ctrlllock kbd-ctrlrlock mmc0 mmc1 timer oneshot heartbeat gpio cpu cpu0 cpu1 cpu2 cpu3 cpu4 cpu5 cpu6 cpu7 [default-on] transient rfkill-any rfkill-none phy0rx phy0tx phy0assoc phy0radio rfkill0</code></pre></blockquote>
<div>Turn OFF, ON, Heartbeat (original state)</div>
<pre><code>echo none > /sys/class/leds/blue\:heartbeat/trigger
echo default-on > /sys/class/leds/blue\:heartbeat/trigger
echo heartbeat > /sys/class/leds/blue\:heartbeat/trigger</code></pre>
<h3>GPIO Pins</h3>
<h3>System Temperature</h3>



</div>
<!--------------------------------------------- GOVERNOR --------------------------------------------->
<div class="details">
<summary>
<h2>Performance Governor</h2>
</summary>

</div>
<!-------------------------------------------- ONBOARD FAN ------------------------------------------->
<div class="details">
<summary>
<h2>Onboard Fan</h2>
</summary>
<h3>Control manually via pwm speed</h3>
<div>Disable automatic control and Write to pwm file</div>

<pre><code>echo 0 > /sys/devices/platform/pwm-fan/hwmon/hwmon0/automatic
echo 10 > /sys/devices/platform/pwm-fan/hwmon/hwmon0/pwm1</code></pre>
<div>Verify</div>

<pre><code>cat /sys/devices/platform/pwm-fan/hwmon/hwmon0/automatic
cat /sys/devices/platform/pwm-fan/hwmon/hwmon0/pwm1</code></pre>

<blockquote><pre><code>1
120</code></pre></blockquote>
<h3>Control automatically</h3>
<div>Enable automatic control</div>

<pre><code>echo 1 > /sys/devices/platform/pwm-fan/hwmon/hwmon0/automatic
cat /sys/devices/platform/pwm-fan/hwmon/hwmon0/automatic</code></pre>
<blockquote><pre><code>1</code></pre></blockquote>
<h3>Trip points</h3>
<p>ODROID XU4 supports 3 cooling levels for thermal control, 0, 1, 2.
Level 0, which is the lowest level for thermal control and comes with the slowest fan speed.
And level 2, which is the highest level for thermal control and comes with the fastest fan speed. <a href="https://wiki.odroid.com/odroid-xu4/application_note/manually_control_the_fan">Reference</a></p>
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
<br>
<p>Get current trip temperatures</p>
<pre><code>cat /sys/devices/virtual/thermal/thermal_zone{0,1,2,3}/trip_point_{0,1,2}_temp</code></pre>
<blockquote><pre><code>60000   <------ Zone 0
70000
80000
60000   <------ Zone 1
70000
80000
60000   <------ Zone 2
70000
80000
60000   <------ Zone 3
70000
80000
</code></pre></blockquote>
<br>
<div>Set trip point 1 to be activated at 30°C.</div>
<pre><code>echo 30000 | sudo tee /sys/devices/virtual/thermal/thermal_zone{0,1,2,3}/trip_point_0_temp
cat /sys/devices/virtual/thermal/thermal_zone{0,1,2,3}/trip_point_0_temp</code></pre>
<blockquote><pre><code>30000</code></pre></blockquote>
<br>
<div> Set the fan speed for each trip point. </div>
<blockquote class="warn">You always have to reboot to apply the changed fan speed for now.</blockquote>

<pre><code>echo "0 204 220 240" > /sys/devices/platform/pwm-fan/hwmon/hwmon0/fan_speed
cat /sys/devices/platform/pwm-fan/hwmon/hwmon0/fan_speed</code></pre>
<blockquote><pre><code>0 120 180 240</code></pre></blockquote>
<h3>Set fan temps and speed at boot</h3>
<p>Write your settings in the /etc/rc.local file.</p>
<pre><code>
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
</code></pre>
<div>Reboot and check if the changes applied.</div>
<pre><code>cat /sys/devices/virtual/thermal/thermal_zone{0,1,2,3}/trip_point_{0,1,2}_temp</code></pre>
<pre><code>cat /sys/devices/platform/pwm-fan/hwmon/hwmon0/fan_speed</code></pre>
<h3>Emulate temperature</h3>
<p>Test fan speeds by covering real temperature values with emulated ones.</p>
<div>Set temperature to 85°C:</div>
<pre><code>echo 85000 | sudo tee /sys/devices/virtual/thermal/thermal_zone{0,1,2,3}/emul_temp
cat /sys/devices/virtual/thermal/thermal_zone{0,1,2,3}/temp</code></pre>
<blockquote><pre><code>85000
85000
85000
85000
85000
</code></pre></blockquote>
<br>
<div>Reset real temperatures</div>
<pre><code>echo 0 | sudo tee /sys/devices/virtual/thermal/thermal_zone{0,1,2,3}/emul_temp
cat /sys/devices/virtual/thermal/thermal_zone{0,1,2,3}/temp</code></pre>
<blockquote><pre><code>51000
53000
56000
55000
</code></pre></blockquote>


</div>
<!-------------------------------------------- BENCHMARKS -------------------------------------------->
<div class="details">
<summary>
<h2>System Benchmarks</h2>
</summary>
<h3>Sysbench</h3>
<pre><code>apt install sysbench</code></pre>
<div>Example tests:</div>
<ul>
<li><code>sysbench cpu help</code></li>
<li><code>sysbench memory help</code></li>
<li><code>sysbench fileio help</code></li>
<li><code>sysbench cpu --threads=8 --cpu-max-prime=10000 run</code></li>
<li><code>sysbench memory --threads=8 run </code></li>
</ul>
<blockquote><pre><code>
CPU speed:
events per second:   500.60

General statistics:
total time:                          10.0139s
total number of events:              5016

Latency (ms):
min:                                    9.58
avg:                                   15.96
max:                                   44.14
95th percentile:                       28.16
sum:                                80042.91

Threads fairness:
events (avg/stddev):           627.0000/76.37
execution time (avg/stddev):   10.0054/0.01
</code></pre></blockquote>
<h3>systemd-analyze</h3>
<div>Will plot a detailed graphic with the boot sequence: kernel time, userspace time, time taken by each service.
</div>
<pre><code>systemd-analyze plot > boot.svg</code></pre>
<h3>speedtest-cli</h3>
<pre><code>sudo apt install speedtest-cli</code></pre>
<pre><code>speedtest-cli</code></pre>
<blockquote><pre><code>Retrieving speedtest.net server list...
Selecting best server based on ping...
Testing download speed...........................................................
Download: 64.38 Mbit/s
Testing upload speed.............................................................
Upload: 18.60 Mbit/s
</code></pre> </blockquote>

</div>
<!------------------------------------------- COMMON TOOLS ------------------------------------------->
<div class="details">
<summary>
<h2>Common Tools</h2>
</summary>
<h3>Golang</h3>
<p>Install the GO programming language:</p>
<pre><code>sudo apt-get install -y golang-go</code></pre>
<h3>Docker</h3>
<p>EXAMPLE:</p>
<pre><code>CODE BIP BOP</code></pre>
<blockquote>
<p>Output:</p>
<pre><code>RESPONSE</code></pre>
</blockquote>
<p>INFO</p>
<pre><code>GLOMP</code></pre>
<h3>Python</h3>
<h3>Java</h3>

</div>