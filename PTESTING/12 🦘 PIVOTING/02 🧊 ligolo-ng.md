
on kali machine:

```bash
sudo ip tuntap add user kali mode tun ligolo
sudo ip link set ligolo up
sudo ip route add $TARGET_IP_2ND.0/24 dev ligolo
./ligolo-proxy -selfcert -laddr 0.0.0.0:443
```

On connection / machine revert
```bash
sudo ip link delete ligolo
```

```shell
session
#select session
start
```

on jump box:
```bash
./ligolo-agent -connect $KALI_IP:443 -ignore-cert
```

remove:
```bash
sudo ip link delete ligolo
```