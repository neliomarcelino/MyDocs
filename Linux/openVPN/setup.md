# Setup openVPN server on linux

## Installing openVPN

### get openvpn-install.sh
```bash
$ wget https://git.io/JeV07 -O openvpn-install.sh
```

### get your public ip
>[showmyipaddress.eu](https://www.showmyipaddress.eu/)

>[whatismypublicip.com](https://whatismypublicip.com/)
```bash
$ dig +short myip.opendns.com @resolver1.opendns.com
```

### create /etc/rc.local (if it doesn't exist)
```bash
$ nano /etc/rc.local
```
paste the following:
```bash
#!/bin/sh -e
#
# rc.local
#
# This script is executed at the end of each multiuser runlevel.
# Make sure that the script will "exit 0" on success or any other
# value on error.
#
# In order to enable or disable this script just change the execution
# bits.
#
# By default this script does nothing.

exit 0
```
then make it executable
```bash
$ sudo chmod +x /etc/rc.local
```

### execute file
```bash
$ sudo bash openvpn-install.sh
```
or
```bash
$ sudo chmod +x openvpn-install.sh
$ sudo ./openvpn-install.sh
 ```

### iptables
It should be already added automaticly iptables rules, if don't edit /etc/rc.local
```
$ sudo nano /etc/rc.local
```
and paste the following:
```bash
iptables -I FORWARD -m state --state RELATED,ESTABLISHED -j ACCEPT
iptables -I FORWARD -s 10.8.0.0/24 -j ACCEPT
iptables -I INPUT -p udp --dport 1194 -j ACCEPT
iptables -t nat -A POSTROUTING -s 10.8.0.0/24 -j SNAT --to 139.59.1.155
```
## Setting up easy-rsa

Setting up vars:
```bash
$ cd /etc/openvpn/server/easy-rsa
$ sudo cp vars.example vars
$ sudo chmod +x vars
```
edit vars:
```bash
$ sudo nano vars
```
add the following at the end of file:
```
set_var EASYRSA "$PWD"
set_var EASYRSA_OPENSSL         "openssl"
set_var EASYRSA_PKI             "$PWD/pki"
set_var EASYRSA_DN              "cn_only"
set_var EASYRSA_REQ_COUNTRY     "YOUR_COUNTRY"
set_var EASYRSA_REQ_PROVINCE    "YOUR_PROVINCE"
set_var EASYRSA_REQ_CITY        "YOUR_CITY"
set_var EASYRSA_REQ_ORG         "YOUR_ORGANIZATION"
set_var EASYRSA_REQ_EMAIL       "YOUR_EMAIL"
set_var EASYRSA_REQ_OU          "YOUR_ORGANIZATION_UNIT"
set_var EASYRSA_KEY_SIZE        2048
set_var EASYRSA_ALGO            rsa
set_var EASYRSA_CA_EXPIRE       3650
set_var EASYRSA_CERT_EXPIRE     3650
set_var EASYRSA_NS_SUPPORT      "no"
set_var EASYRSA_NS_COMMENT      "YOUR_NETSCAPE_COMMENT"
set_var EASYRSA_EXT_DIR         "$EASYRSA/x509-types"
set_var EASYRSA_SSL_CONF        "$EASYRSA/openssl-easyrsa.cnf"
set_var EASYRSA_DIGEST          "sha256"
```

**_Note:_**
>Increase the 'EASYRSA_KEY_SIZE' for better security.

>Change 'EASYRSA_CA_EXPIRE' and 'EASYRSA_CERT_EXPIRE' as you wish.

### Create server certificate and key

#### build CA [ca.crt]
```bash
$ ./easyrsa init-pki
$ ./easyrsa build-ca
```

#### build server key ["COMMON NAME".key]
```bash
$ ./easyrsa gen-req "COMMON NAME" nopass
```
**_Note:_**
>nopass = option to disable password on file key.

#### build server certificate ["COMMON NAME".crt]
```bash
$ ./easyrsa sign-req server "COMMON NAME"
```

#### check if crt was signed correctly
```bash
$ openssl verify -CAfile pki/ca.crt pki/issued/"COMMON NAME".crt
```

### (optional) Create client keys and certificates
**the file openvpn-install.sh will create clients more easily, but if you would like to create manualy do the next steps**

#### build client key ["CLIENT NAME".key]
```bash
$ ./easyrsa gen-req "CLIENT NAME" nopass
```

#### build client certificate ["CLIENT NAME".crt]
```bash
$ ./easyrsa sign-req client "CLIENT NAME"
```

#### check if key was signed with any errors
```bash
$ openssl verify -CAfile pki/ca.crt pki/issued/"CLIENT NAME".crt
```

### Build Diffie-Hellman Key [dh.key]
```bash
$ ./easyrsa gen-dh
```
*this may take a while*

### (optional) Build Certificate Revoking List
This next step is usefull if you will use multiple client certificates on your vpn server

```bash
$ ./easyrsa revoke "CLIENT NAME"
$ ./easyrsa gen-crl
```

## Setting openVPN
```bash
$ cd /etc/openvpn
$ sudo nano server.conf
```
if no file named "server.conf" at this directory create it and paste the following lines:
```
# OpenVPN Port, Protocol and the Tun
port 1194
proto udp
dev tun

# OpenVPN Server Certificate - CA, server key and certificate
ca /etc/openvpn/server/easy-rsa/pki/ca.crt
cert /etc/openvpn/server/easy-rsa/pki/issued/gitpy-server.crt
key /etc/openvpn/server/easy-rsa/pki/private/gitpy-server.key

#DH and CRL key
dh /etc/openvpn/server/easy-rsa/pki/dh.pem
crl-verify /etc/openvpn/server/easy-rsa/pki/crl.pem

# Network Configuration - Internal network
# Redirect all Connection through OpenVPN Server
server 10.8.0.0 255.255.255.0
push "redirect-gateway def1 bypass-dhcp"

# Using the DNS from https://dns.watch
push "dhcp-option DNS 1.1.1.1"
push "dhcp-option DNS 8.8.8.8"

#Enable multiple client to connect with same Certificate key
#duplicate-cn

# TLS Security
cipher AES-256-CBC
tls-auth /etc/openvpn/server/ta.key 0
auth SHA512
#auth-nocache

# Other Configuration
keepalive 10 120
persist-key
persist-tun
user openvpn
group openvpn
topology subnet
ifconfig-pool-persist ipp.txt

# OpenVPN Log
log-append /var/log/openvpn.log
verb 3
```

## Enable Port-Forwarding
```bash
$ cat /etc/sysctl.conf
```
if theresn't a line with `net.ipv4.ip_forward = 1` the run the following command:
```bash
$ echo 'net.ipv4.ip_forward = 1' >> /etc/sysctl.conf
```

## Create clients
Go back to the directory where openvpn-install.sh is and run it:
```bash
$ sudo bash openvpn-install.sh
```
or
```bash
$ sudo ./openvpn-install.sh
```

Now it will pop up a menu. press 1 to create a client.
Give it a name.
If the creation was complete sucessfuly, you should have a new file located on the same folder as the openvpn-install.sh named "CLIENT NAME".ovpn

## Client configuration
All the client needs to connect to your vpn server is the `"CLIENT NAME".crt` and `"CLIENT NAME".ovpn`
All the created certificates are located at `/etc/openvpn/server/easyrsa/pki/issued` but you allway need to use `sudo` in order to access this files, else you will allways get permission denied error.

```bash
$ sudo cp /etc/openvpn/server/easyrsa/pki/issued/"CLIENT NAME".crt .
```


*Credits:*
>https://www.cyberciti.biz/faq/howto-setup-openvpn-server-on-ubuntu-linux-14-04-or-16-04-lts/

>https://www.howtoforge.com/tutorial/how-to-install-openvpn-server-and-client-with-easy-rsa-3-on-centos-7/