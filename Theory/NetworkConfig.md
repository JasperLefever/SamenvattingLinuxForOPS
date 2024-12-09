# Network config

- `journalctl -xu <service>` - show logs for a service (-e to jump to the end, -f to follow logs)

- `ifconfig` - show network interface configuration (deprecated)
- `ip address` - show network interface configuration(NO-CARRIER means cable is not connected)
- `ip route` - show routing table
- `ss` - show socket statistics
- `ping` - send ICMP ECHO_REQUEST to network hosts (-c limit number of echo requests)
- `traceroute` - print the route packets take to network host (-I use ICMP ECHO for probes, -T use TCP SYN for probes) has become less reliable
- `tracepath` - similar to traceroute but uses UDP probes (no root required)
- `dig` - DNS lookup utility
  - `getent ahosts <hostname>` - get address information for a host name to check if DNS resolution is working
- `ip neighbour` - show ARP cache(REACHABLE: resolved, STALE: not resolved, DELAY: waiting for ARP reply)
- `ip n get <ip>` - get MAC address of an IP
- `ip n del <ip>` - delete ARP entry
- `ip n flush` - flush ARP cache
- `curl ipinfo.io` - get public IP address

## Network config debian

- `/etc/network/interfaces` - network interface configuration file

```bash
auto eth0
iface eth0 inet static
  address
    netmask
    gateway
```

After making changes to `/etc/network/interfaces` run `ifdown eth0 && ifup eth0` to apply changes

### Netplan

- `/etc/netplan/01-netcfg.yaml` - network configuration file

```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    eth0:
      dhcp4: no
      addresses:
        - <ip>/<subnet>
      gateway4: <gateway>
      nameservers:
        addresses:
          - <dns>
```

> After making changes to `/etc/netplan/01-netcfg.yaml` run `sudo netplan apply` to apply changes

## Network config enterprise linux

- `/etc/sysconfig/network-scripts/ifcfg-<interface>` - network interface configuration file

```bash
NAME=eth0  # name of the interface
DEVICE=eth0 # device name
BOOTPROTO=none # none(static) or dhcp
ONBOOT=yes
IPADDR= # IP address
NETMASK= # subnet mask
GATEWAY= # default gateway
```

After making changes to `/etc/sysconfig/network-scripts/ifcfg-<interface>` run `nmcli con reload` to apply changes

### NetworkManager

- `nmcli` - NetworkManager command-line tool
  - `nmcli connection show` - show all connections
  - `nmcli connection show <connection>` - show connection details
  - `nmcli connection modify <connection> connection.id <new_name>` - rename connection
  - `nmcli connection modify <connection> ipv4.method manual ipv4.addresses <ip>/<subnet> ipv4.gateway <gateway> ipv4.dns <ip>` - set static IP
  - `nmcli connection modify <connection> ipv4.method auto` - set DHCP
  - `nmcli connection modify <connection> ipv6.method auto` - set DHCPv6
  - `nmcli connection modify <connection> ipv6.method manual ipv6.addresses <ip>/<subnet> ipv6.gateway <gateway> ipv6.dns <ip>` - set static IPv6

> after making changes to NetworkManager configuration files, run `sudo nmcli connection up <interface>` to apply changes

## Hostname

- `hostnamectl set-hostname <hostname>` - set hostname

## DHCP

### Install DHCP server debian

- Package name: `isc-dhcp-server`
- Configuration file: `/etc/dhcp/dhcpd.conf`

CONFIG

> `dhcpd -t -cf <config-file>` - check configuration file for errors

> disable IPv6 in `/etc/default/isc-dhcp-server`
>
> ```bash
> INTERFACESv4="eth1"
> INTERFACESv6=""
> ```

> `systemctl start isc-dhcp-server.service`

### Install DHCP server enterprise linux

- Package name: `dhcp-server`
- Configuration file: `/etc/dhcp/dhcpd.conf`

CONFIG

> `dhcpd -t -cf <config-file>` - check configuration file for errors

> `systemctl start dhcpd.service`

### CONFIG EXAMPLE

-

```bash
# Global dhcp settings
option domain-name "example.com"; # domain name
option domain-name-servers ns1.example.com, ns2.example.com; # DNS servers
option routers 192.168.42.254; # default gateway
default-lease-time 600; # 10 minutes
max-lease-time 7200; # 2 hours
authoritative; # server is authoritative for the network

# Subnet declaration
subnet 192.168.42.0 netmask 255.255.255.0 {
  range 192.168.42.101 192.168.42.252; # IP range
  option routers 192.168.42.254; # default gateway
}

# Client reservations
host pc42 {
    hardware ethernet 08:00:27:d2:26:79;
    fixed-address 192.168.42.42;
#    option domain-name-servers ns1.example.com;
#    option domain-name "example.com";
#    option routers 192.168.42.254
}
```
