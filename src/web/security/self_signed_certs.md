# Self-signed Certificate
A website's certificate is trusted by default if it's signed by a well-known CA. However, for local dev and testing of `https`, a self-signed certificate is often needed. Below steps show how to create a custom CA and create a self-signed certificate with it.

## Create a root CA certificate
1. Create a root key:
    ```powershell
    openssl ecparam -out customca.key -name prime256v1 -genkey
    ```
2. Create a root signing request:
    ```powershell
    openssl req -new -sha256 -key customca.key -out customca.csr
    ```
3. Self-sign:
    ```powershell
    openssl x509 -req -sha256 -days 365 -in customca.csr -signkey customca.key -out customca.crt
    ```
This creates the root CA certificate `customca.crt`. You will use this to sign the server certificate. 

## Create a server certificate
1. Create a server key:
    ```powershell
    openssl genrsa -out server.key 2048
    ```
2. Create a request configuration file to to handle SAN. **Important**: localhost must be in either the CN (Common Name) or the list of subjectAltName. Also note the CN for the server certificate must be different from the issuer's domain. For example, the CN for the issuer is www.customcaissuer.com and the server certificate's CN is www.localhost.com.
    ```
    [req]
    distinguished_name = req_distinguished_name
    x509_extensions = v3_req
    prompt = no
    [req_distinguished_name]
    C = US
    ST = DC
    L = DC
    O = OR
    OU = OU
    CN = www.localhost.com
    [v3_req]
    keyUsage = critical, digitalSignature, keyAgreement, keyCertSign
    extendedKeyUsage = serverAuth
    subjectAltName = @alt_names
    [alt_names]
    DNS.1 = localhost
    DNS.2 = 127.0.0.1
    DNS.3 = www.localhost.com
    DNS.4 = localhost.com
    ```
3. Create a certificate signing request: 
    ```
    openssl req -new -sha256 -key server.key -out server.csr -config req.cnf
    ```
4. Self-sign using custom CA. **Important**: this command should include an extension which also uses the req.cnf for the subjectAltName configuration.
    ```powershell
    openssl x509 -req -in server.csr -CA  mwsca.crt -CAkey mwsca.key -CAcreateserial -out server.crt -days 999 -sha256 -extensions v3_req -extfile req.cnf
    ```

## Add the root certificate to your machine's trusted root store
Add the root CA certificate to your machine's trusted root store. When you access the website, ensure the entire certificate chain is seen in the browser.

However, if for security reason, the machine's trusted root store cannnot be modified, you can add the root CA certificate for each client trying to access the server. For example, if accessing using a web browser, add the root CA certificate to the browser trusted root. If using Postman, add the root CA certificate as the trusted CA within Postman.