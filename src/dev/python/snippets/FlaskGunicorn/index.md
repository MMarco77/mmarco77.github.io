# Simple HTTP(s) server with Gunicor and Flask

## Code

## Gunicorn HTTPs certificate and keys

```shell
$ gunicorn  wsgi:app \
            --bind 0.0.0.0:8080 \
            --log-level=debug \
            --workers=4 \
            --keyfile private_key.pem \
            --certfile certificate.pem
```

### Generate certificate and key for test

1. Creating a Private Key

```shell
# 3DES encrypted
$ openssl genrsa -des3 -out domain.key 2048
# unencrypted
$ openssl genrsa -out domain.key 2048
```
2. Creating a Certificate Signing Request (CSR)

```shell
$ openssl req -key domain.key -new -out domain.csr
```

An important field is “Common Name,” which should be the exact Fully Qualified Domain Name (FQDN) of our domain.

*“A challenge password” and “An optional company name” can be left empty.*

**One line Private Key and CSR**

```shell
# 3DES encrypted
$ openssl req -newkey rsa:2048 -keyout domain.key -out domain.csr
# unencrypted
$ openssl req -newkey rsa:2048 -nodes -keyout domain.key -out domain.csr
```

3. Creating a Self-Signed Certificate

```shell
$ openssl x509 -signkey domain.key -in domain.csr -req -days 365 -out domain.crt
```

Our certificate (domain.crt) is **an X.509 certificate that’s ASCII PEM-encoded.**

Here is the one line to create private key and a self-signed certificate (CSR tmp will be created)

```shell
$ openssl req -newkey rsa:2048 -keyout domain.key -x509 -days 365 -out domain.crt
```

- View certificat

```shell
$ openssl x509 -text -noout -in domain.crt
```