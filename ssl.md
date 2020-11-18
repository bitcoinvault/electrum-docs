# How to configure SSL with Electrum

This page was written for Electrum 4.0 (currently in [development](https://github.com/spesmilo/electrum#development-version-git-clone))

You should have a TLS/SSL private key and a public certificate for
your domain set up already (signed by a CA, for example free [Letsencrypt](https://letsencrypt.org/))

## Add your SSL private key

Create a file that contains only the private key:

```
-----BEGIN PRIVATE KEY-----
your private key
-----END PRIVATE KEY-----
```

Please note that this is not your wallet key but a private key for the
matching TLS/SSL certificate.

Set the path to your the SSL private key file with setconfig:

```
electrum -o setconfig ssl_keyfile /path/to/ssl/privkey.pem
```

## Add your SSL certificate bundle

Create another file, file that contains your certificate,
and the list of certificates it depends on, up to the root
CA. Your certificate must be at the top of the list, and
the root CA at the end.

```
-----BEGIN CERTIFICATE-----
your cert
-----END CERTIFICATE-----
-----BEGIN CERTIFICATE-----
intermediate cert
-----END CERTIFICATE-----
-----BEGIN CERTIFICATE-----
root cert
-----END CERTIFICATE-----
```

Set the ssl_chain path with setconfig:

```
electrum -o setconfig ssl_certfile /path/to/ssl/fullchain.pem
```

## Check that your certificate was accepted by Electrum

Check that your SSL certificate correctly configured:

```
electrum -o get_ssl_domain
```

This should return the Common Name of your certificate.
