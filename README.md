# Open ssl cheatsheet

```bash
$ openssl version
OpenSSL 1.1.0g  2 Nov 2017
```

## Lexicon
* `.pem` : Base 64 format
* `.der` : Binary format
* `.csr` : Certificate Signing Request
* `.crt` or `.cer` : Certificate
* `.p12` : Binary format wrapping a key and a certificate in one file

## Commands

### Root CA
Self signed certificate:
```bash
openssl req -x509 \
    -newkey rsa:4096 \
    -keyout ca.key.pem \
    -out ca.crt.pem \
    -days 1825 \
    -subj "/CN=$COMMON_NAME" \
    -passout pass:my_great_password
```

### New certificate

Create a key : `openssl genrsa -out private.key.pem 4096`

Create a `.csr` from key : `openssl req -new -key private.key.pem -out certificate_request.csr.pem`

Create a `.csr` from key (non-interactive) :
```bash
openssl req -new \
    -key private.key.pem \
    -out certificate_request.csr.pem \
    -subj "/C=$COUNTRY/ST=$STATE/L=$CITY/O=$ORGANIZATION/OU=$ORGANIZATION_UNIT/CN=$COMMON_NAME"
```

Sign certificate :
```bash
openssl x509 -req \
    -in certificate_request.csr.pem \
    -days 1825 \
    -CA ca.crt.pem \
    -CAkey ca.key.pem \
    -set_serial 01 \
    -out my_public_certificate.crt.pem
```


### Convert PEM to DER :

* Crt
```bash
openssl x509 -outform der -in my_public_certificate.crt.pem -out my_public_certificate.crt.der
```
* Key
```bash
openssl rsa -outform der -in private.key.pem -out private.key.der
```


### Other

Convert key to pkc8 without password :
```bash
openssl pkcs8 -topk8 \
    -inform pem \
    -outform pem \
    -in private.key.pem \
    -out private.key.pkcs8.pem -nocrypt
```

Create `.p12` (interactive) :
```bash
openssl pkcs12 -export -out keyStore.p12 -inkey private.key.pem -in my_public_certificate.crt.pem
```

Update certificate :
```bash
sudo dpkg-reconfigure ca-certificates
sudo update-ca-certificates
```

Create ks from crt :
`keytool -import -alias ca -file ca.crt.pem -keystore ca.ks`

