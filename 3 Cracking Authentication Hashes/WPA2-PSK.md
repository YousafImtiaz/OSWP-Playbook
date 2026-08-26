> Once we have captured a handshake we can crack the key using the cap file:

```
sudo aircrack-ng -w /usr/share/john/password.lst -e <essid> -b <bssid> <capfile>
```

Also try using rockyou.txt as a backup.