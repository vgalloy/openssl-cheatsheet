# Open ssl cheatsheet

`
$ openssl version
OpenSSL 1.1.0g  2 Nov 2017
`

## Lexicon
* `.pem` : Base 64 format
* `.der` : Non Base 64 format

* `.csr` : Certificate Signing Request.
* `.crt` : Certificate



## Command

Create a key : `openssl genrsa -out private.key.pem 4096`

Create a `.csr` from key : `openssl req -new -key private.key.pem -out certificate_request.csr.pem`

Create a `.csr` from key (non-interactive) :
```
openssl req -new \
    -key private.key.pem \
    -out certificate_request.csr.pem \
    -subj "/C=$COUNTRY/ST=$STATE/L=$CITY/O=$ORGANIZATION/OU=$ORGANIZATION_UNIT/CN=$COMMON_NAME
```

Sign certificate :
```
openssl x509 -req \
    -in certificate_request.csr.pem \
    -days 1825 \
    -CA ca.crt.pem \
    -CAkey ca.key.pem \
    -set_serial 01 \
    -out my_public_certificate.crt.pem
```

Self signed certificate :
```
openssl req -x509 \
    -newkey rsa:4096 \
    -keyout ca.key.pem \
    -out ca.crt.pem \
    -days 1825 \
    -subj "/CN=$COMMON_NAME" \
    -passout pass:my_great_password
```

Convert key to pkc8 and remove password :
```
openssl pkcs8 -topk8 \
    -inform PEM \
    -outform PEM \
    -in private.key.pem \
    -out private.key.pkcs8.pem -nocrypt
```

Update certificate :
`sudo dpkg-reconfigure ca-certificates` `sudo update-ca-certificates`

Create ks from crt :
`keytool -import -alias ca -file ca.crt.pem -keystore ca.ks`

