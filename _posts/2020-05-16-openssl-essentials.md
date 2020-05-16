---
layout: post
title: "Working with OpenSSL"
date: 2020-05-16 14:30:31
tags: [tech, cli, cheatsheets, openssl, certificate]
description: "Working with SSL Certificates, Private Keys and CSRs"
excerpt: "SSL Certificates, Private Keys and CSRs"
#image: ""
categories:
    - cheatsheets
---

## Certificate Signing Requests (CSRs)
Obtaining an SSL certificate from a certificate authority (CA) requires to generate a certificate signing request (CSR). Provide information u might be prompted, especially the Common Name (CN) has to be correct and should be the Fully Qualified Domain Name (FQDN).

## Generate a Private Key and a CSR
Create a new folder and dir into it. Create a 2048-bit private key (domain.key) and a CSR (domain.csr):
```bash
openssl req -newkey rsa:2048 -nodes -keyout domain.key -out domain.csr
```

## Generate a CSR from an Existing Private Key
```bash
openssl x509 -in domain.crt -signkey domain.key -x509toreq -out domain.csr
```

## Generate a Self-Signed Certificate
This is for use without a CA
```bash
openssl req -newkey rsa:2048 -nodes -keyout domain.key -x509 -days 365 -out domain.crt
```

## Generate a Self-Signed Certificate from an Existing Private Key
openssl req -key domain.key -new -x509 -days 365 -out domain.crt

