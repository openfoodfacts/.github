# API Security when calling APIs from a Browser Application

## Context and Problem Statement

Currently, re-users of the Open Food Facts APIs have two options for authentication, where this is required:

- pass the username and password with each request as query or form parameters
- use the login API to obtain a session cookie

However, in both cases this requires the consumer to know the username and password for the contributor which is not best practice and would not support other authentication mechanisms like passkeys of social login.

Also, if a session cookie is created on the openfoodfacts.org domain via a legitimate web application, this would allow any other web application to also make authenticated API requests.

The introduction of Keycloak allows us to use OAuth and OIDC authentication mechanisms. This allows the user to login via their preferred method and once Keycloak has confirmed their identity, an Access Token is issued which can be used to call APIs that require authentication.

However, there are a number of articles, such as [this one](https://ianlondon.github.io/posts/dont-use-jwts-for-sessions/) advise against the use of JWTs in browser apps.

Another potential concern is that a malicious app could potentially use another app's client id and capture the user's entered credentials and / or redirect URL in an embedded WebView. The [current advice](https://datatracker.ietf.org/doc/html/rfc8252#section-8.12) is not to use Embedded User-Agents, like WebViews, for authentication. [User agent filtering](https://blog.please-open.it/posts/user-agent-filter-authenticator/) can mitigate this although it could still be circumvented by user agent manipulation in the WebView, but user agent filtering could still promote the best-practice of using the main browser or an In-App browser rather than a WebView.

The purpose of this document is to determine the best way forward.

## Decision Drivers

* at no point should a re-users application need to know a user's username and password
* users should not be forced to log in again on a regular basis
* the authentication process should follow industry standard practice as closely as possible to reduce friction for API re-users and Open Food Facts code contributors
* many re-users create apps that do not have a backend, e.g. [waistline](https://github.com/davidhealey/waistline)
* the level of protection should be proportionate to the sensitivity of the APIs, e.g. there is minimal benefit to an attacker in being able to submit malicious food data contributions on a user's behalf

## Considered Options

* Allow access tokens to be used for API requests, irrespective of origin
* Disable CORS for authenticated Open Food Facts APIs so that access tokens can only be used from the re-user's backend
* Implement CORS-enabled authentication endpoint to start a session

## Decision Outcome

Chosen option: "Implement CORS-enabled authentication endpoint to start a session", because this is the most secure way to support apps with no backend and therefore no secure way of storing a client secret.

### Consequences

Browser applications will need to call the "Login API" after obtaining an access token before calling other APIs. The "Login API" would only support access tokens obtained through a user login flow, i.e not via the client credentials flow.

CORS will be disallowed on all APIs apart from the "Login API". All APIs will support authentication with an access token or a session cookie.

The backend will need to periodically refresh the session cookie and check that the session has not been revoked by the user in Keycloak (e.g. using the [GET /admin/realms/{realm}/users/{user-id}/sessions](https://www.keycloak.org/docs-api/latest/rest-api/index.html#_get_adminrealmsrealmusersuser_idsessions) API)

User Agent Filtering could be implemented to ensure that mobile apps don't inadvertently use a WebView in the authentication flow.

### Confirmation

Re-user clients should be created via a standard tool so that permissions are set correctly, depending on the type of client.

## Pros and Cons of the Options

### Allow Access Tokens to be used in all API requests

With this approach we make it the re-user's responsibility to secure their Access Tokens. Our APIs would support CORS so that API calls can be made directly from the browser. The consequence of this is that the re-user may choose to store the Access Token in an insecure way.

This can be mitigated to some extent by enabling the [Revoke Refresh Token](https://www.keycloak.org/docs/latest/server_admin/index.html#_offline-access) option so that a Refresh Token can only be used once. However, there are still attacks that can get around this.

* Good: Simplest to implement. API calls are the same whether from a web application, mobile application or backend
* Neutral: The client would only be able to call authenticated APIs on behalf of a user, so user consent would always be required
* Bad: Open to XSS attacks or others depending on how the Access / Refresh Token is stored

### Only allow Authenticated APIs to be called from backends

This could be achieved by disabling CORS on our authenticated APIs and only creating non-public clients in Keycloak, thus preventing Implicit or PKCE login flows.

* Good: There is no incentive for a re-user to store an Access Token in the browser
* Neutral: Increased latency (but this only applies to authenticated actions like product updates where the frequency is low)
* Bad: Web applications will always need a corresponding backend to obtain Access Tokens and call authenticated APIs

### Implement CORS-enabled authentication endpoint to start a session

With this approach, a web application would go through a normal PKCE flow to obtain an access token and then use this to call a CORS-enabled endpoint (analogous to the existing Login API) to create a session, which would be stored in a Secure HttpOnly cookie. The web application can then forget the access token as there is no need to use this on subsequent requests. Other APIs would disable CORS so could only be called with a session cookie from a browser, but could still be called with an access token from a mobile app or backend.

* Good: Access tokens are not retained in the browser
* Neutral: An additional step is required to start a session
* Neutral: With no refresh token being used the Open Food Facts backend will need to manually check for revoked sessions in Keycloak
