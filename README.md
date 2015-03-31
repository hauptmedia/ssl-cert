# ssl-cert

# create-ca

Creates a certificate authority.

```bash
# Usage: bin/create-ca [-d depot directory] [-v validity in days]

bin/create-ca -d /path/to/your/ssl/files -v 3650
```

# create-etcd-cert

Created SSL certificates which can be used in etcd. It uses the provided etcd ssl extensions defined in `etc/openssl.cnf`

```bash
# Usage: bin/create-etcd-cert -t server|client|peer -c CommonName [-d depot directory] [-i ip address to embed in SAN] [-i multiple ips may be specified]
# Example: bin/create-etcd-cert -t server -d /path/to/your/ssl/depot -c coreos-1.example.com -i 8.8.8.8 -i 192.168.3.2

bin/create-etcd-cert -t server -c coreos-1.skydns.io -i 192.168.1.2 
bin/create-etcd-cert -t client -c coreos-1.skydns.io -i 192.168.1.2
bin/create-etcd-cert -t peer -c coreos-1.skydns.io -i 192.168.1.2
```

## etcd ssl certificate requirements in detail

#### Client certificate requirements
* IP address of the client has to be included as *subjectAltName* on the certificate. In order to get *subjectAltName* you need to enable relevant *X509.3* extension
* Certificate has to have *Extended key usage* extension enabled and allow *TLS Web Client Authentication*.

#### Peer certificate requirements
* Similarly to client certificate, the IP address has to be included in *SAN*. See above for details.
* Certificate has to have *Extended key usage* extension enabled and allow *TLS Web Server Authentication*.

## References
* https://coreos.com/docs/distributed-configuration/etcd-security/
* http://blog.skrobul.com/securing_etcd_with_tls/
* https://github.com/kelseyhightower/etcd-production-setup
* http://www.g-loaded.eu/2005/11/10/be-your-own-ca/
