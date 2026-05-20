## TLS (Transport Layer Security)

TLS (Transport Layer Security) is the cryptographic protocol that provides end-to-end security for data transmitted over the internet. It is the "S" in HTTPS. (Its predecessor, SSL, is obsolete but the terms are still often used interchangeably.)

If HTTP is the language browsers and servers use to communicate, TLS is the secure, tamper-proof room they enter before speaking.

### The Three Goals of TLS
TLS solves three core security problems:

- **Encryption (Privacy):** Scrambles data so that only the intended recipient can read it. Without TLS, data travels in plain text and can be intercepted by any router or server along the path.
- **Authentication (Identity):** Proves the server is exactly who it claims to be (e.g., `www.bank.com` is the real bank, not a fake site). This is done using Digital Certificates issued by trusted Certificate Authorities.
- **Integrity (Tamper Prevention):** Detects if data has been altered in transit. Any modification (e.g., injecting malicious code) causes the connection to be immediately severed.

### Asymmetric vs Symmetric Encryption
TLS uses both types of encryption strategically:

- **Asymmetric Encryption (Public/Private Keys):** The server publishes a Public Key that anyone can use to encrypt data. Only the server’s hidden Private Key can decrypt it. Extremely secure but computationally slow.
- **Symmetric Encryption (Session Keys):** Both sides use the exact same secret key to encrypt and decrypt data. Extremely fast but requires a secure way to share the key.

**TLS Solution:** It uses the slow asymmetric encryption **only** for the initial key exchange. Once both sides securely share a Symmetric Session Key, they switch to fast symmetric encryption for the rest of the session.

### The TLS Handshake (Step-by-Step)
The handshake occurs in milliseconds before any application data is sent:

1. **Client Hello:** The browser sends a list of supported encryption algorithms (ciphers) to the server.
2. **Server Hello & Certificate:** The server selects a cipher and sends its Digital Certificate containing its Public Key.
3. **Client Verification:** The browser validates the certificate against its list of trusted Certificate Authorities.
4. **Secure Key Exchange:** The browser generates a Pre-Master Secret, encrypts it with the server’s Public Key, and sends it. Only the server’s Private Key can decrypt it.
5. **Session Keys Generated:** Both sides independently derive the same Symmetric Session Key from the Pre-Master Secret.
6. **Secure Connection Established:** All further communication uses the fast symmetric key. The handshake is complete.

This process ensures privacy, authenticity, and integrity for every HTTPS connection.

