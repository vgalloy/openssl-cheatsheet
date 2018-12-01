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

Create a key :

`openssl genrsa -out private.key.pem 4096`

Create a `.csr` from key :

`openssl req -new -key private.key.pem -out certificate_request.csr.pem`

Create a `.csr` from key (non-interactive) :

`openssl req -new -key private.key.pem -out certificate_request.csr.pem -subj "/C=$COUNTRY/ST=$STATE/L=$CITY/O=$ORGANIZATION/OU=$ORGANIZATION_UNIT/CN=$COMMON_NAME`

Sign certificate

`openssl x509 -req -in certificate_request.csr.pem -days 1825 -CA ca.crt.pem -CAkey ca.key.pem -set_serial 01 -out my_public_certificate.crt.pem`


