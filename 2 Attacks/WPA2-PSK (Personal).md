> Once we are doing the targeted sniff, we can do a deauth attack in another terminal session:

```
sudo aireplay-ng -0 1 -e <essid> -a <bssid> -c <station> wlan0mon
```

Once Aireplay has checked the BSSID and sends the deauth packets, we'll capture a handshake once the client reconnects to the AP. We will see WPA handshake in the top right of Airodump where we are running the targeted sniff.

If we dont get a handshake we can retry the command without -c to send a broadcast deauth. We can also try without -a as well if needed.

Once we have captured the handshake we can stop the targeted sniff and move on to cracking the key via the cap file.