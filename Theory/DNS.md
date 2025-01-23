# DNS

## Bind

### Enterprise linux

Files:

- `/etc/named.conf`
- `/var/named`

```bash
sudo dnf install bind bind-utils
sudo systemctl enable --now named
```

#### Configuration

#### Primary DNS Server

```conf
//
// named.conf
//
// Provided by Red Hat bind package to configure the ISC BIND named(8) DNS
// server as a caching only nameserver (as a localhost DNS resolver only).
//
// See /usr/share/doc/bind*/sample/ for example named configuration files.
//

options {
        listen-on port 53 { 127.0.0.1; }; # Change this the ip addresses you want bind to listen on 127.0.0.1 is only available on the local machine
        listen-on-v6 port 53 { ::1; }; # Change this the ip addresses you want bind to listen on ::1 is only available on the local machine
        directory       "/var/named"; # where the zone files are stored
        dump-file       "/var/named/data/cache_dump.db";
        statistics-file "/var/named/data/named_stats.txt";
        memstatistics-file "/var/named/data/named_mem_stats.txt";
        secroots-file   "/var/named/data/named.secroots";
        recursing-file  "/var/named/data/named.recursing";
        allow-query     { localhost; }; # Must change to allow other machines to querry the dns server (eg. allow-query { any; };)

        /*
         - If you are building an AUTHORITATIVE DNS server, do NOT enable recursion.
         - If you are building a RECURSIVE (caching) DNS server, you need to enable
           recursion.
         - If your recursive DNS server has a public IP address, you MUST enable access
           control to limit queries to your legitimate users. Failing to do so will
           cause your server to become part of large scale DNS amplification
           attacks. Implementing BCP38 within your network would greatly
           reduce such attack surface
        */
        recursion yes; # if you are setting up a authoritative dns server set this to no
#        allow-recursion { localhost; }; # specifies the ip addresses that recursion is allowed on
#        allow-recursion-on { localhost; }; # specifies the interfaces that recursion is allowed on

#        forward first; # only or first; first will try and resolve the query itself if the forwarder fails, only will only use the forwarder
#        forwarders {
#           <ip address>;
#        };


        dnssec-validation yes;

        managed-keys-directory "/var/named/dynamic";
        geoip-directory "/usr/share/GeoIP";

        pid-file "/run/named/named.pid";
        session-keyfile "/run/named/session.key";

        /* https://fedoraproject.org/wiki/Changes/CryptoPolicy */
        include "/etc/crypto-policies/back-ends/bind.config";
};

logging {
        channel default_debug {
                file "data/named.run";
                severity dynamic;
        };
};

zone "." IN {
        type hint;
        file "named.ca";
};

## zone added in different file see below
zone "example.com" IN {
  type primary;
  file "example.com";
#  allow-transfer { <ip address>; }; # specifies the ip addresses that are allowed to transfer the zone
#  notify yes; # specifies if the server should notify the slave servers when the zone has been updated
#  allow-update { <ip address>; }; # specifies the ip addresses that are allowed to update the zone
};

## reverse zone added in different file see below
# zone for network 192.0.2.x
zone "2.0.192.in-addr.arpa" IN {
  type master;
  file "2.0.192.in-addr.arpa";
};

include "/etc/named.rfc1912.zones";
include "/etc/named.root.key";
```

```conf
# zone file example
; base zone file for example.com
$TTL 2d    ; default TTL for zone
$ORIGIN example.com. ; base domain-name
; Start of Authority RR defining the key characteristics of the zone (domain)
@         IN      SOA   ns1.example.com. hostmaster.example.com. (
                                2003080800 ; serial number
                                12h        ; refresh when the serial has changed update the zone transfer of secondary servers
                                15m        ; update retry for zone transfer
                                3w         ; expiry time a slave may use data before it must be discarded
                                2h         ; minimum
                                )
; name server RR for the domain
           IN      NS      ns1.example.com.
; the second name server is external to this zone (domain)
           IN      NS      ns2.example.net.
; mail server RRs for the zone (domain)
        3w IN      MX  10  mail.example.com.
; the second  mail servers is  external to the zone (domain)
           IN      MX  20  mail.example.net.
; domain hosts includes NS and MX records defined above
; plus any others required
; for instance a user query for the A RR of joe.example.com will
; return the IPv4 address 192.168.254.6 from this zone file
ns1        IN      A       192.168.254.2
mail       IN      A       192.168.254.4
joe        IN      A       192.168.254.6
www        IN      A       192.168.254.7
; aliases ftp (ftp server) to an external domain
ftp        IN      CNAME   ftp.example.net.
```

> REVERSE ZONE FILE

```conf
;; Zone file for reverse lookup zone 2.0.192.in-addr.arpa.
$ORIGIN 2.0.192.in-addr.arpa.
$TTL 1W

@ IN SOA ns1.example.com. hostmaster.example.com. (
         24061601  ; Serial
         1D        ; Refresh time
         1H        ; Retry time
         1W        ; Expiry time
         1D )      ; Negative cache TTL

; Name servers

       IN  NS     ns1.example.com.
       IN  NS     ns2.example.com.

; Reverse lookup records

1    IN  PTR    ns1.example.com.
2    IN  PTR    ns2.example.com.
10   IN  PTR    srv001.example.com.
20   IN  PTR    srv002.example.com.
```

##### Secondary DNS Server

```conf
# Zone definitions on the secondary server

zone "example.com" IN {
    type secondary;
    primaries { 192.168.56.11; };
    file "slaves/example.com";
};

zone "2.0.192.in-addr.arpa" IN {
    type secondary;
    primaries { 192.168.56.11; };
    file "slaves/2.0.192.in-addr.arpa";
};
```

> force zone refresh

```bash
sudo rndc retransfer example.com
```

#### Troubleshooting

- `journalctl -xe`
- `sudo named-checkconf <zone> <configfile>`

```

```
