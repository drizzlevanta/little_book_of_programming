# OAuth

## OpenID Connect (OIDC)

OpenID Connect (OIDC) is an open authentication protocol that works on top of the OAuth 2.0 framework. Targeted toward consumers, OIDC allows individuals to use single sign-on (SSO) to access relying party sites using OpenID Providers (OPs), such as an email provider or social network, to authenticate their identities.

## OAuth 2.0

OAuth 2.0 is the industry-standard protocol for authorization. OAuth 2.0 focuses on client developer simplicity while providing specific authorization flows for web applications, desktop applications, mobile phones, and living room devices.

NPM package `angular-oauth2-oidc` provides support for OAuth 2 and OIDC in Angular.

### ID Token vs. Access Token

- ID Token (OIDC): Contains claims about the authenticated user (e.g., sub, email, name).
- Access Token (OAuth2): Used for authorizing API access, but does not inherently contain user identity details.
  | Feature | OAuth 2.0 | OIDC |
  |----------|----------|------|
  | **Purpose** | Authorization | Authentication + Authorization |
  | **Tokens** | Access Token, Refresh Token | ID Token, Access Token, Refresh Token |
  | **User Authentication** | ❌ No | ✅ Yes |
  | **Used for API access** | ✅ Yes | ✅ Yes |
  | **Used for Login (SSO, Federated Identity)** | ❌ No | ✅ Yes |
  | **Discovery Endpoint** | ❌ No | ✅ Yes |

### Authorization Code Workflow with PKCE

The Authorization Code Flow is the most secure way to authenticate users and obtain access tokens in OAuth 2.0 and OpenID Connect (OIDC). It involves exchanging an authorization code for an access token (and optionally an ID token in OIDC).

1. User initiates login, sends state, nonce and code challenge -> Redirected to Authorization Server.
2. User logs in and grants permission. The user authenticates on the Authorization Server. The state is included in the redirect response. If authentication is successful, the user is asked to approve or deny the authorization request (**this includes verifying the state parameter matches the request**, if not, reject the response to prevent CSRF attacks).
3. If the user approves, the authorization server redirects the user back to the application with an authorization code.
4. Client exchanges authorization code for an access token (and an ID token in OIDC). The ID token contains user identity information.
5. When the client request the access token, in addition to the authorization code, it also includes the original code verifier used to generate the code challenge. The authorization server verifies the code challenge in the client request.
6. The client decodes the ID token and verifies: Does the nonce inside the ID token match the nonce originally sent? If not, reject the ID token because it may be a replay attack.
7. Client uses access token to access APIs.
8. (Optional) Client refreshes token when it expires.

### Authorization Code Flow with PKCE

PKCE (Proof Key for Code Exchange, pronounced "pixy") is an OAuth 2.0 security extension that prevents authorization code interception attacks. It is mandatory for public clients like SPAs and mobile apps that cannot safely store a client secret.

Why is PKCE Needed? In a traditional Authorization Code Flow (without PKCE), an attacker could intercept the authorization code and use it to obtain an access token. Since public clients (e.g., SPAs, mobile apps) don't have a client secret, there's no way to verify that the request for an access token came from the legitimate client.

PKCE solves this problem by adding a randomly generated code challenge that binds the authorization request to the token request.

### How PKCE Works (Step-by-Step)

PKCE modifies the **Authorization Code Flow** by adding two extra values:

- **Code Verifier** (random secret stored in the client)
- **Code Challenge** (derived from the code verifier and sent to the authorization server)

#### 1. SPA/Mobile App Generates a Code Verifier & Code Challenge

- A **random string** (`code_verifier`) is generated (43-128 characters).
- A **hashed version** (`code_challenge`) is derived using SHA-256.

#### 2. Client Requests Authorization Code

- The SPA redirects the user to the **authorization server** with:
- `client_id`
- `redirect_uri`
- `response_type=code`
- `code_challenge` (the hashed value)
- `code_challenge_method=S256`

#### 3. User Logs In & Gets an Authorization Code

- The authorization server authenticates the user.
- It **stores** the `code_challenge` and sends the **authorization code** back to the SPA.

#### 4. Client Exchanges Code for an Access Token

- The SPA sends a **backend request** to exchange the code for an access token.
- The request **includes the original `code_verifier`**.

#### 5. Authorization Server Verifies the Code Verifier

- The server **recomputes** the `code_challenge` from the `code_verifier` and **compares it** with the stored `code_challenge`.
- If they match, the server issues an **access token**.

### ROPC

The Resource Owner Password Credentials (ROPC) Grant in OAuth2 allows a user to authenticate by directly sending their username and password to the application, which then exchanges them for an access token. While this method may seem simple, it is generally not recommended for modern applications due to several security risks and limitations:

- Security Risk: The client application directly handles user credentials, increasing the risk of credential leaks.
- No MFA Support: It does not support modern authentication methods like multi-factor authentication (MFA) or Single Sign-On (SSO).
- Requires High Trust in Clients: The client must be fully trusted since it handles passwords directly.

Only recommend this when other options are not viable.

### Implicit flow

Not recommended due to security risks. Implicit flow (directly returns access token), Previously used by SPAs to directly receive an access token in the URL, but is vulnerable to token leakage. Implicit flow does not involve a code, after user authenticates on the authorization server, it immediately returns an access token in the Url.

### Keycloak

In Keycloak, since the login page is served from Keycloak's backend, credentials never touch the client application directly.

### Is the Access Token Exposed to the Browser?

If an application stores the access token in session storage, **JavaScript code in the same origin** can access it. This means if an attacker injects malicious JavaScript via Cross-Site Scripting (XSS), they can steal the token.

However, the token is not directly exposed in the URL (unlike in Implicit Flow), reducing exposure risk.

## State and Nounce

Both the state and nonce parameters are security mechanisms used in OAuth 2.0 (and more specifically in OpenID Connect) to help protect the integrity of the authentication flow. Although they are similar in that they involve passing a random value back and forth between the client and the authorization server, they serve distinct purposes and are used at different stages of the flow.

### State

- Generation: When initiating the OAuth2 authorization request, the client application generates a unique and unpredictable string. This string might also include additional encoded data (like the URL the user was trying to access).
- Inclusion in Request: The generated state is sent as a query parameter (e.g., ?state=abc123...) along with the other OAuth parameters.
- Round-Trip: The authorization server receives the request and then includes the same state parameter in its response (either in the query string or fragment of the redirect URI).
- Verification: When the client receives the response, it checks that the state value matches the one it originally sent. A mismatch indicates that the response might have been tampered with or initiated from an unauthorized source, prompting the client to reject the authentication response.

### CSRF vs Replay Attack vs XSS

Cross-Site Request Forgery (CSRF) is when an attacker tricks a user's browser into making an unwanted request to a website where the user is authenticated. The attacker might craft a malicious link or form that, when clicked or auto-submitted by the user's browser, sends a request (like a fund transfer or data change) to the target site. Since the browser automatically sends cookies and authentication tokens, the site treats the request as legitimate.

Replay attack: A replay attack occurs when an attacker intercepts valid data (like an authentication token or transaction message) and later reuses that data to repeat a request or transaction. Prevention methods include using one-time tokens (nonces), timestamps, and session identifiers that are valid only for a short period or a single use. This is why OAuth flows use a nonce—to bind the token to a specific request and prevent its reuse.

Cross-Site Scripting (XSS): XSS is a vulnerability where an attacker injects malicious scripts into web pages viewed by other users. The attacker exploits weaknesses in input validation to insert JavaScript (or other code) into a page. When other users view the page, the malicious code runs in their browsers, potentially stealing data, manipulating the page, or performing actions on behalf of the user. Defenses include proper output encoding, input validation, using Content Security Policy (CSP) headers.

CSRF:
Targets the user's authenticated session by tricking their browser into submitting unintended requests. It leverages the fact that browsers automatically send credentials (like cookies) with each request.

Replay Attack:
Involves capturing and reusing valid data (such as a token or a signed message) to perform unauthorized actions. It relies on the reuse of a message rather than tricking the browser.

XSS:
Occurs when an attacker injects malicious code into a web page, which then executes in the browser of anyone who views the page. Unlike CSRF and replay attacks, XSS directly affects the client-side code by executing scripts that the attacker controls.

### How State and Nounce are used to prevent these attacks

State Prevents CSRF:

When the client initiates an OAuth/OIDC authentication request, it generates a unique state value and sends it along with the request.
Once the authorization server responds, the client checks that the returned state matches the original value. This check confirms that the response was triggered by the client's own request and not by a malicious third party (which might try to trick the user's browser into sending a forged request). In this way, the state parameter helps prevent Cross-Site Request Forgery (CSRF) attacks.

In summary, the state parameter ensures that the response is associated with the proper request, the authorization request and response belong to the same client (blocking CSRF), and the nonce confirms that the token is unique to the current session (blocking replay attacks). Neither state nor nonce protects against vulnerabilities like XSS; they are specifically designed to secure the OAuth/OIDC authentication process.

| Feature                   | `state`                                         | `nonce`                                     |
| ------------------------- | ----------------------------------------------- | ------------------------------------------- |
| **Purpose**               | Prevents **CSRF attacks**                       | Prevents **Replay attacks**                 |
| **Where is it used?**     | Sent in **login request & redirect URL**        | Sent in **login request & JWT (ID Token)**  |
| **What does it protect?** | Ensures the redirect response is valid          | Ensures the token is not reused             |
| **Checked against?**      | The **redirect URL** from the Identity Provider | The **JWT (ID Token)** received after login |
| **Generated by?**         | Your Angular app                                | Your Angular app                            |
| **Checked by?**           | Your Angular app when processing the redirect   | Your Angular app before using the ID Token  |
| **Used in**               | OAuth 2.0                                       | OpenID Connect (OIDC)                       |
