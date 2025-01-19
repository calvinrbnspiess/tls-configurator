# TLS Configurator

This project is aimed to provide a simple way to configure a TLS certificate for web servers. Certificate pairs can be provided and be fed into the web server configuration.

## Testing

The project is tested against a docker compoose environment running apache2. The server will spin up without tls/ssl configuration and serve a basic web page available on 127.0.0.1:8080. The server can be configured to use the provided certificate pair.

```bash
sudo docker compose up -d --build --force-recreate
```

Generating a sample certificate pair can be done with the following command:

```bash
openssl req -x509 -newkey rsa:4096 -keyout demo-crt/key.pem -out demo-crt/cert.pem  -sha256 -days 3650 -nodes -subj "/C=DE/ST=Rhineland-Palatinate/L=SampleCity/O=SampleOrganization/CN=localhost" -addext "subjectAltName=DNS:localhost,DNS:*.localhost,IP:127.0.0.1"
```

We need to make sure [Subject Alternative Names](https://stackoverflow.com/a/66839523/13353068) are given.

### Common file types

![Application to X.509 Certificate](https://i.sstatic.net/Ku0lg.png)

```plaintext
- .der -> binary encoded
- .pem -> base64 encoded
- .crt -> could be DER or PEM, means it is the public key (difference might not be clear by just checking file extension)
- .key / key.pem -> private key
```

[Read more at the Apache HTTP Server documentation](https://httpd.apache.org/docs/2.4/ssl/ssl_faq.html#aboutcerts).

Make sure to run the `install.sh` script inside the container using `docker exec -it tls-configurator-apache /usr/local/apache2/install.sh`.

You can enter the container using `docker exec -it tls-configurator-apache /bin/bash`.

## macOS: Add certificate to system keychain

```bash
sudo security import demo-crt/cert.pem  -k ~/Library/Keychains/login.keychain-db
```
