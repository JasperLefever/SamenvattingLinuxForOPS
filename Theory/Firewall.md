# Firewall

## Debian based systems

- `ufw` is the default firewall management tool

## Enterprise Linux based systems

- `firewalld` is the default firewall management tool

- client: `firewall-cmd`

#### List all rules

```bash
sudo firewall-cmd --list-all
```

Example output:

```bash
[student@el ~]$ sudo firewall-cmd --list-all
public (active) # active zone
  target: default
  icmp-block-inversion: no
  interfaces: eth0 eth1 # interfaces the zone applies to
  sources:
  services: cockpit dhcpv6-client ssh # allowed services
  ports: # allowed ports
  protocols: # allowed protocols
  forward: yes
  masquerade: no # to enable NAT
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:
```

> **Note**: Do not add double entries like adding a port 22 and adding a service ssh. The service ssh already includes port 22.

#### Zones

- `firewalld` uses zones to group rules together

- Zones are predefined sets of rules that are applied to interfaces.

For example, the `public` zone could be used for interfaces that are connected to the internet, while the `internal` zone could be used for interfaces that are connected to the internal network. like for services that are only available on the internal network.

> get predefined zones

```bash
sudo firewall-cmd --get-zones
```

> get the default zone

```bash
sudo firewall-cmd --get-default-zone
```

> get the active zones

```bash
sudo firewall-cmd --get-active-zones
```

#### Services

> get predefined services

```bash
sudo firewall-cmd --get-services
```

> get port of a service

```bash
sudo firewall-cmd --info-service=ssh
```

#### Adding rules

- `--add-service` adds a service to the zone

```bash
sudo firewall-cmd --zone=public --add-service=ssh --permanent
```

- Note: `--permanent` makes the rule persistent across reboots if not specified the rule will be lost after a reboot.

- `--add-port` adds a port to the zone

```bash
sudo firewall-cmd --zone=public --add-port=80/tcp --permanent
```

#### Removing rules

- `--remove-service` removes a service from the zone

```bash
sudo firewall-cmd --zone=public --remove-service=ssh --permanent
```

- `--remove-port` removes a port from the zone

```bash
sudo firewall-cmd --zone=public --remove-port=80/tcp --permanent
```

#### Reload the firewall

```bash
sudo firewall-cmd --reload
```

#### Assigning a zone to an interface

- `--add-interface` assigns an interface to a zone
- `--remove-interface` removes an interface from a zone

```bash
sudo firewall-cmd --zone=public --add-interface=eth0 --permanent

sudo firewall-cmd --zone=public --remove-interface=eth0 --permanent
```

#### Panic mode

- `--panic-on` enables panic mode which blocks all incoming and outgoing traffic

```bash
# to enable panic mode
sudo firewall-cmd --panic-on
# to disable panic mode
sudo firewall-cmd --panic-off
```

### Firewalld NAT routing

- `firewalld` can be used to configure NAT routing

```bash
sudo nano /etc/sysctl.conf <<< "net.ipv4.ip_forward = 1" # enable ip forwarding

# reload the sysctl configuration
sudo sysctl -p

sysctl net.ipv4.ip_forward # check if ip forwarding is enabled

sudo firewall-cmd --zone=public --add-masquerade --permanent # enable masquerade on internet facing interface

# enable forwarding from internal to external
# sudo firewall-cmd --permanent --new-policy=internal-external

sudo firewall-cmd --zone=external --change-interface=eth0 --permanent

sudo firewall-cmd --zone=internal --change-interface=eth1 --permanent


sudo firewall-cmd --permanent --policy=internal-external --set-target=ACCEPT

sudo firewall-cmd --permanent --policy=internal-external --add-masquerade

sudo firewall-cmd --permanent --policy=internal-external --add-ingress-zone=internal

sudo firewall-cmd --permanent --policy=internal-external --add-egress-zone=external

sudo firewall-cmd --reload
```
