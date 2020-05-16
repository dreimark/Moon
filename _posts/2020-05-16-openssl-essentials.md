---
layout: post
title: "Mastering OpenSSL"
date: 2020-05-16 14:30:31
tags: [tech, cli, cheatsheets, openssl, certificate]
description: "Mastering SSL certificates, private keys and CSR using openSSL."
excerpt: "Guide for obtaining SSL certificates from a certificate authority"
#image: ""
categories:
    - cheatsheets
---

## About Certificate Signing Requests (CSRs)
Obtaining an SSL certificate from a certificate authority (CA) requires to generate a certificate signing request (CSR). Provide information u might be prompted, especially the Common Name (CN) has to be correct and should be the Fully Qualified Domain Name (FQDN).

# Generating CSRs
## Generate a Private Key and a CSR
Use this method if you want to use HTTPS (HTTP over TLS). Create a new folder, dir into it and build a 2048-bit private key (domain.key) and a CSR (domain.csr):
```bash
openssl req -newkey rsa:2048 -nodes -keyout domain.key -out domain.csr
```

## Generate a CSR from an Existing Private Key
```bash
openssl x509 -in domain.crt -signkey domain.key -x509toreq -out domain.csr
```
# Generating SSL Certificates
## Generate a Self-Signed Certificate
This is for use without a CA
```bash
openssl req -newkey rsa:2048 -nodes -keyout domain.key -x509 -days 365 -out domain.crt
```

## Generate a Self-Signed Certificate from an Existing Private Key
```bash
openssl req -key domain.key -new -x509 -days 365 -out domain.crt
```

## Generate a Self-Signed Certificate from an Existing Private Key and CSR 
```bash
openssl x509 -signkey domain.key -in domain.csr -req -days 365 -out domain.crt  openssl x509 -signkey domain.key -in domain.csr -req -days 365 -out domain.crt
```
# View Certificates
## View CSR Entries
```bash
openssl req -text -noout -verify -in domain.csr
```

## View Certificate Entries
```bash
openssl x509 -text -noout -in domain.crt  
```

## Verify a Certificate was Signed by a CA
```bash
openssl verify -verbose -CAFile ca.crt domain.crt
```
# Private Keys
## Create a Private Key
openssl genrsa -des3 -out domain.key 2048

## Verify a Private Key
```bash
openssl rsa -check -in domain.key
```

## Verify a Private Key Matches a Certificate and CSR
```bash
openssl rsa -noout -modulus -in domain.key | openssl md5 openssl x509 -noout -modulus -in domain.crt | openssl md5 openssl req -noout -modulus -in domain.csr | openssl md5
```

## Encrypt a Private Key
```bash
openssl rsa -des3 -in unencrypted.key -out encrypted.key
```


## Decrypt a Private Key
```bash
openssl rsa -in encrypted.key -out decrypted.key
```

# Convert Certificate Formats
## Convert PEM to DER
```bash
openssl x509 -in domain.crt -outform der -out domain.der
```


## Convert DER to PEM
```bash
openssl x509 -inform der -in domain.der -out domain.crt
```


## Convert PEM to PKCS7
```bash
openssl pkcs12 -inkey domain.key -in domain.crt -export -out domain.pfx
```


## Convert PKCS12 to PEM
```bash
openssl pkcs12 -in domain.pfx -nodes -out domain.combined.crt
```