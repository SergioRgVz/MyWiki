# Apagar VPN:
```
sudo wg-quick up wg0
```

# Activar VPN: 
```
sudo wg-quick up wg0
```

# Fichero config /etc/wireguard/wg0.conf
```shell
[Interface]
PrivateKey = INQIC97KMLVED/jqrYAeTiVJk4YpVRrOrVy2Gf+CHGE=
Address = 192.168.5.35/32
DNS = 192.168.168.1



[Peer]
PublicKey = gLki/nYIltSyE36M2lyTRa1l13xecprv2aZCtszcbXM=
# AllowedIPs = 0.0.0.0/0
AllowedIPs = 192.168.5.0/24, 192.168.168.0/24
Endpoint = 2.136.238.187:13231
PersistentKeepalive = 10
```
