# ubuntu-server-Step-by-step-to-assign-the-static-IP
static ip assign


 Identify the network interface name
   ```
   ip link show
   ip addr

   ```

Edit your Netplan configuration

Open (or create) the file /etc/netplan/01-static-ip.yaml:

we need to orginal config file backup ....... 00-installer-config.yaml

```
network:
  version: 2
  ethernets:
    ens33:
      addresses: [192.168.100.16/24]
      gateway4: 192.168.100.1
      nameservers:
        addresses: [8.8.8.8]
```
2. Test before applying permanently
```
sudo netplan try
```
If the network comes up and the IP works, press ENTER to accept.

If something goes wrong and you don’t confirm, it will auto-revert after ~120 seconds.
3. Apply the configuration

```
sudo netplan apply
```
Verify the result

Check the new IP:
```
ip addr show ens33
```
You should now see:
```
inet 192.168.100.16/24 scope global ens33
```
Check the default route:
```
ip route
```
It should show:
```
default via 192.168.100.1 dev ens33
```
Ping tests:
```
ping -c 3 192.168.100.1      # gateway
ping -c 3 8.8.8.8            # external IP
ping -c 3 google.com         # DNS check
```


