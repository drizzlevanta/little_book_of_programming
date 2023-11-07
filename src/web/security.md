# Security

## Authentication

### OpenID Connect (OIDC)

OpenID Connect (OIDC) is an open authentication protocol that works on top of the OAuth 2.0 framework. Targeted toward consumers, OIDC allows individuals to use single sign-on (SSO) to access relying party sites using OpenID Providers (OPs), such as an email provider or social network, to authenticate their identities.

### OAuth 2.0

OAuth 2.0 is the industry-standard protocol for authorization. OAuth 2.0 focuses on client developer simplicity while providing specific authorization flows for web applications, desktop applications, mobile phones, and living room devices.

NPM package angular-oauth2-oidc provides support for OAuth 2 and OIDC in Angular.

## Certificates
### Client Certificates
A client certificate and key are components used in SSL/TLS (Secure Sockets Layer/Transport Layer Security) protocols to establish secure communication between a client (such as a web browser) and a server. While server certificates are used to authenticate the server to the client, client certificates are used to authenticate the client to the server.

Client certificates are commonly used in scenarios where the server needs to verify the identity of the connecting client. This is more common in enterprise environments, VPNs (Virtual Private Networks), and certain web applications that require strong client authentication. The use of client certificates adds an extra layer of security by ensuring that both the server and the client can authenticate each other in a mutually trusted manner.

### Server Certificates
The server certificate serves as a way to verify the authenticity of the server to the client. It includes information about the server, such as its domain name, the name of the organization that owns the server, and the digital signature of the certificate authority (CA) that issued the certificate.

One of the primary purposes of SSL/TLS is to encrypt the communication between the client and the server. The server certificate contains a public key, which is used for encryption. When a client connects to a secure website (using HTTPS), the client and server negotiate a shared secret key for encrypting and decrypting data during the session.

### Use of both
When a client connects to a server, the server certificate is **always** used to establish the server's identity. Whether a client certificate is used depends on the server's configuration and security requirements. If client authentication is required, the client certificate and associated private key are used during the SSL/TLS handshake process to provide proof of the client's identity to the server. 