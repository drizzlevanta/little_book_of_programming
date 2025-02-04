# OAuth

## OpenID Connect (OIDC)

OpenID Connect (OIDC) is an open authentication protocol that works on top of the OAuth 2.0 framework. Targeted toward consumers, OIDC allows individuals to use single sign-on (SSO) to access relying party sites using OpenID Providers (OPs), such as an email provider or social network, to authenticate their identities.

## OAuth 2.0

OAuth 2.0 is the industry-standard protocol for authorization. OAuth 2.0 focuses on client developer simplicity while providing specific authorization flows for web applications, desktop applications, mobile phones, and living room devices.

NPM package `angular-oauth2-oidc` provides support for OAuth 2 and OIDC in Angular.

## OAuth Workflow

Below is the workflow for OAuth code flow:

- User initiates login → Redirected to Authorization Server.
- User authenticates & grants permission.
- Client receives an authorization code.
- Client exchanges authorization code for an access token.
- Client uses access token to access APIs.
- (Optional) Client refreshes token when it expires.

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
Once the authorization server responds, the client checks that the returned state matches the original value.
This check confirms that the response was triggered by the client’s own request and not by a malicious third party (which might try to trick the user’s browser into sending a forged request).
In this way, the state parameter helps prevent Cross-Site Request Forgery (CSRF) attacks.

In summary, the state parameter ensures that the response is associated with the proper request (blocking CSRF), and the nonce confirms that the token is unique to the current session (blocking replay attacks). Neither state nor nonce protects against vulnerabilities like XSS; they are specifically designed to secure the OAuth/OIDC authentication process.

| Feature                   | `state`                                         | `nonce`                                     |
| ------------------------- | ----------------------------------------------- | ------------------------------------------- |
| **Purpose**               | Prevents **CSRF attacks**                       | Prevents **Replay attacks**                 |
| **Where is it used?**     | Sent in **login request & redirect URL**        | Sent in **login request & JWT (ID Token)**  |
| **What does it protect?** | Ensures the redirect response is valid          | Ensures the token is not reused             |
| **Checked against?**      | The **redirect URL** from the Identity Provider | The **JWT (ID Token)** received after login |
| **Generated by?**         | Your Angular app                                | Your Angular app                            |
| **Checked by?**           | Your Angular app when processing the redirect   | Your Angular app before using the ID Token  |

## ROPC

The Resource Owner Password Credentials (ROPC) Grant in OAuth2 allows a user to authenticate by directly sending their username and password to the application, which then exchanges them for an access token. While this method may seem simple, it is generally not recommended for modern applications due to several security risks and limitations.
