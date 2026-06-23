# TLS certificate analysis and explanation

## Subject
CN = 16.16.183.114

## Issuer
Self Signed Certificate

## Key Algorithm
RSA 2048

## Validity
365 Days

## Why HTTPS Works

Nginx is configured with a self-signed TLS certificate and serves content over port 443.

## Why curl without -k fails

The certificate is self-signed and not trusted by public Certificate Authorities. Therefore clients reject it unless certificate validation is bypassed.

## TLS Version

TLS 1.2 and TLS 1.3 are enabled.

TLS 1.1 is disabled for security reasons.
