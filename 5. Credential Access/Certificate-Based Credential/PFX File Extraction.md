
```bash
# Per te ber ekstrat te dhenave na nje file .pfx duhet te perodrim openssl komand
openssl pkcs12 -info -in legacyy_dev_auth.pfx -passin pass:thuglegacy

# Extract Private Key
openssl pkcs12 -in legacyy_dev_auth.pfx  -nocerts -nodes  -out private.key  
-passin pass:thuglegacy

# Extract certificate
openssl pkcs12 -in legacyy_dev_auth.pfx  -clcerts -nokeys  -out cert.crt  -passin pass:thuglegacy
```
