**Thumbprint in OIDC and SSL/TLS**

### What is a Thumbprint?
A thumbprint (also known as a fingerprint) is a unique identifier for an SSL/TLS certificate. It is a cryptographic hash (usually SHA-1 or SHA-256) of the certificate used to verify the identity of a trusted entity.

### Thumbprint in OIDC (OpenID Connect)
In OIDC, the thumbprint is used to validate the TLS certificate of the identity provider (IdP). This ensures that the OIDC provider’s public key has not been tampered with, preventing man-in-the-middle attacks.

#### How It Works:
1. When setting up OIDC trust, AWS or other platforms require the thumbprint of the IdP’s certificate.
2. The thumbprint is generated from the root certificate of the IdP.
3. AWS verifies the TLS connection against this thumbprint before trusting the identity provider.

### How to Obtain a Certificate Thumbprint
To manually retrieve a thumbprint, you can use the following methods:

#### Using OpenSSL (Linux/macOS/WSL)
```sh
openssl s_client -showcerts -connect oidc.provider.com:443 </dev/null 2>/dev/null | openssl x509 -fingerprint -sha256 -noout
```

### Thumbprint in AWS OIDC Role Assumption
When setting up OIDC in AWS for authentication (e.g., GitHub Actions), AWS requires the thumbprint of GitHub’s OIDC certificate to verify its legitimacy before allowing role assumption.

#### Avoiding Long-Lived Credentials
By using OIDC and validating the thumbprint, AWS can authenticate requests from trusted sources without needing long-lived access keys. This improves security by reducing credential exposure.

### Considerations for Thumbprint Management
- **Thumbprints can change** when the identity provider rotates their certificate, requiring an update in AWS settings.
- **Automating retrieval** of the latest thumbprint is recommended for maintaining security and availability.
- **Public Thumbprints**: Thumbprints themselves are derived from public certificates, but their usage must be carefully managed to prevent unauthorized access.

This ensures a secure authentication flow when integrating with external identity providers like GitHub Actions or other OIDC-based services.

