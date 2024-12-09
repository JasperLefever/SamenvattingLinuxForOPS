# Apache webserver

## Packages

- Enterpise Linux: `httpd`
- Debian-based Linux: `apache2`

- **Document Root**: `/var/www/html/`
- **Log** files: `/var/log/apache` or `/var/log/httpd`

## Debian

### Files and Directories

- `/etc/apache2/`: Main configuration directory.

- **Configuration Files**:
  - `/etc/apache2/apache2.conf`: Main configuration file.
  - `/etc/apache2/sites-available/`: Directory for site configuration files. Virtual hosts

### Virtual Hosts

> File must end with `.conf`
> Each virtual host must have a unique port number
> Document root must be specified

```bash
root@linux:~# cat /etc/apache2/sites-available/choochoo.conf
<VirtualHost *:7000>
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/choochoo
</VirtualHost>
root@linux:~# vi /etc/apache2/sites-available/chessclub42.conf
root@linux:~# cat /etc/apache2/sites-available/chessclub42.conf
<VirtualHost *:8000>
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/chessclub42
</VirtualHost>
root@linux:~# vi /etc/apache2/sites-available/hunter2.conf
root@linux:~# cat /etc/apache2/sites-available/hunter2.conf
<VirtualHost *:9000>
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/hunter2
</VirtualHost>
```

> ports have to be opened in `ports.conf`

```bash
root@linux:~# cat /etc/apache2/ports.conf
Listen 80
Listen 7000
Listen 8000
Listen 9000
```

> Make sure the document root exists and has a `index.html` file

> Enable the virtual host `a2ensite <site>.conf`
> this will create a symlink in `/etc/apache2/sites-enabled/`

> reload apache `systemctl reload apache2`

---

> What if we want to access the virtual hosts by name instead of port?

> In `/etc/apache2/sites-available/<site>.conf` add the following line in the virtual host block and make sure the port is set to 80:

```bash
ServerName <name>
```

> Run `a2ensite <site>.xxx` and `systemctl reload apache2`

> ONLY FOR LOCAL TESTING: Add the following line to `/etc/hosts`:

```bash
<IP> <domain.local>
```

### SSL

```bash
sudo apt install openssl
mkdir /etc/ssl/localcerts
openssl req -new -x509 -days 999 -nodes -out /etc/ssl/local\
certs/apache.pem -keyout /etc/ssl/localcerts/apache.key # 999 days valid
chmod 600 /etc/ssl/localcerts/*
apache ssl mod
service apache2 restart
```

> Add the following lines to the virtual host configuration file:

```bash
 SSLEngine On
 SSLCertificateFile /etc/ssl/localcerts/apache.pem
 SSLCertificateKeyFile /etc/ssl/localcerts/apache.key
```

## Enterprise Linux

### Files and Directories

- `/etc/httpd/`: Main configuration directory.

- **Configuration Files**:
  - `/etc/httpd/conf/httpd.conf`: Main configuration file.

### Virtual Hosts

> ALl virtual hosts are defined in `/etc/httpd/conf.d/`

> Each virtual host must have a unique port number

> Document root must be specified

```bash
root@linux:~# cat /etc/httpd/conf.d/choochoo.conf
<VirtualHost *:7000>
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/choochoo
</VirtualHost>
```

> Enable port in `/etc/httpd/conf/httpd.conf`

```bash
root@linux:~# cat /etc/httpd/conf/httpd.conf
Listen 80
Listen 7000
```

> restarting server wont work because of SELinux

```bash
semange port -m -t http_port_t -p tcp <port>
sudo systemctl restart httpd
```

> All files in `/conf.d/` are automatically loaded
> if you wish to disable a virtual host, rename the file to `*.conf.disabled` or move the file

> Firewall must be configured to allow traffic on the port

```bash
iptables -I INPUT -p tcp --dport <PORT> -j ACCEPT
...
service iptables save # save the configuration
```

---

> What if we want to access the virtual hosts by name instead of port?

> Enable Virtual hosts in `/etc/httpd/conf/httpd.conf`

```bash
...
NameVirtualHost *:80
...
```

> Create virtual hosts in `/etc/httpd/conf.d/`
> virtual host port must be same as the port in `httpd.conf`

```bash
root@linux:~# cat /etc/httpd/conf.d/choochoo.local.conf
<VirtualHost *:80>
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/choochoo
        ServerName choochoo.local
</VirtualHost>
```

> Edit `/etc/hosts` to add the domain

> Reload apache `systemctl reload httpd`

### SSL

```bash
sudo dnf install openssl mod_ssl
mkdir /etc/ssl/localcerts
cd localcerts
openssl genrsa -out ca.key 2048
openssl req -new -key ca.key -out ca.csr
cp ca.crt /etc/pki/tls/certs/ # copy the certificate to the correct directory
cp ca.key ca.csr /etc/pki/tls/private/ # copy the key and csr to the correct directory
```

> Change NameVirtualHost to `*:443` in `/etc/httpd/conf/httpd.conf`

> Add the following lines to `/etc/httpd/conf.d/ssl.conf`

```bash
SSLCertificateFile /etc/pki/tls/certs/ca.crt
SSLCertificateKeyFile /etc/pki/tls/private/ca.key
```

> add the following lines to the virtual host configuration file and change the port to 443:

````bash
<VirtualHost *:443>
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/choochoo
    ServerName choochoo.local
    SSLEngine on
    SSLCertificateFile /etc/pki/tls/certs/ca.crt
    SSLCertificateKeyFile /etc/pki/tls/private/ca.key
</VirtualHost>

> Restart apache `systemctl restart httpd`

## Password Protection

> Create a password file with `htpasswd -c /etc/apache2/.htpasswd <user>`

> Add `.htaccess` file to the directory you want to protect:

```bash
AuthType Basic
AuthName "Restricted Content"
AuthUserFile /etc/apache2/.htpasswd
Require valid-user
````

> If your site is a subdirectory of the document root, you need to add the following line to `/etc/apache2/apache2.conf`:

```bash
<Directory /var/www/>
    ...
    AllowOverride All # on debian
    AllowOverride AuthConfig # on enterprise linux
    ...
</Directory>
```
