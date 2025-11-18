# OVPN Split Tunnel
Manually edit .ovpn files to enable split tunneling
## Prerequisites
- Valid `.ovpn` file.
- Connection credentials.
## Editing the Configuration File
1. Comment out `redirect-gateway def1`.
2. Add in the following lines, editing the route and subnet as needed.
    ```
    compat-mode 2.4.0
    pull-filter ignore redirect-gateway
    pull-filter ignore block-outside-dns
    route-nopull
    route 192.168.0.0 255.255.255.0
    ```
## Example
### Before
```
dev tun
client
proto tcp
<ca>
-----BEGIN CERTIFICATE-----
[cert]
-----END CERTIFICATE-----
</ca>
<cert>
-----BEGIN CERTIFICATE-----
[cert]
-----END CERTIFICATE-----
</cert>
<key>
-----BEGIN PRIVATE KEY-----
[key]
-----END PRIVATE KEY-----
</key>
remote-cert-eku "TLS Web Server Authentication"
remote [connection address]
remote [alternate connection address] [connection port]
redirect-gateway def1
persist-key
persist-tun
verb 3
mute 20
keepalive 10 60
cipher AES-256-CBC
auth SHA256
float
reneg-sec 28800
nobind
mute-replay-warnings
auth-user-pass
tls-version-min 1.2
;remember_connection 0
;auto_reconnect 1
```
### After
```
dev tun
client
proto tcp
<ca>
-----BEGIN CERTIFICATE-----
[cert]
-----END CERTIFICATE-----
</ca>
<cert>
-----BEGIN CERTIFICATE-----
[cert]
-----END CERTIFICATE-----
</cert>
<key>
-----BEGIN PRIVATE KEY-----
[key]
-----END PRIVATE KEY-----
</key>
remote-cert-eku "TLS Web Server Authentication"
remote [connection address]
remote [alternate connection address] [connection port]
# redirect-gateway def1
compat-mode 2.4.0
pull-filter ignore redirect-gateway
pull-filter ignore block-outside-dns
route-nopull
route 192.168.0.0 255.255.255.0
persist-key
persist-tun
verb 3
mute 20
keepalive 10 60
cipher AES-256-CBC
auth SHA256
float
reneg-sec 28800
nobind
mute-replay-warnings
auth-user-pass
tls-version-min 1.2
;remember_connection 0
;auto_reconnect 1
```