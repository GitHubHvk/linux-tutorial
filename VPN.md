# Set L2TP Client In Debian

run the following command in terminal:

```
apt-get install network-manager-l2tp  network-manager-l2tp-gnome
```



then go to (( settings/network manager )) do following steps:

1. click on plus button above ((VPN)) section.
2. in the opened dialog select ((Layer 2 Tunnel Protocol (L2TP) )).
3. in (( Add VPN )) dialog do the following:
   - enter whatever name you want in (( Name )) section.
   -  in (( Gateway )) section enter the IP or domain address of your VPN server.
   - in (( Username )) and (( Password )) sections enter corresponding info of your VPN.
   - then click on (( IPSec Settings )) button, and in the opened dialog activate checkbox (( Enable IPSec tunnel to L2TP host )), then enter your (( Pre-shared key)) and finally click on (( Apply )) button.
4. finally click on (( Apply )) button.
