# API Security when calling APIS from a Browser Application

## Context and Problem Statement

Currently re-users of the Open Food Facts APIs have two options for authentication, where this is required:

- pass the username and password with each request using HTTP Basic Authentication
- use the login API to obtain a session cookie

However, in both cases this requires the consumer to know the username and password for the contributor which is not best practice and would not support other authentication mechanisms like passkeys of social login.

Also, if a session cookie is created on the openfoodfacts.org domain via a legitimate web application this would allow any other web application to also make authenticated API requests.

The introduction of Keycloak allows us to use OAuth and OIDC authentication mechanisms. This allows the user to login via their preferred method and once Keycloak has confirmed their identity an Access Token is issued which can be used to call APIs that require authentication.

However, there are a number of articles, such as [this one](https://ianlondon.github.io/posts/dont-use-jwts-for-sessions/) advise against the use of JWTs in browser apps.

The purpose of this document is to determine the best way forward.

## Decision Drivers

* at no point should a re-users application need to know a user's username and password
* users should not be forced to log in again on a regular basis
* the authentication process should follow industry standard practice as closely as possible to reduce friction for API re-users and Open Food Facts code contributors
* the level of protection should be proportionate to the sensitivity of the APIs, e.g. there is minimal benefit to an attacker in being able to submit malicious food data contributions on a user's behalf

## Considered Options

* Allow access tokens to be used for API requests, irrespective of origin
* Disable CORS for authenticated Open Food Facts APIs so that access tokens can only be used from the re-user's backend

## Decision Outcome

Chosen option: "{title of option 1}", because
{justification. e.g., only option, which meets k.o. criterion decision driver | which resolves force {force} | … | comes out best (see below)}.

<!-- This is an optional element. Feel free to remove. -->
### Consequences

{Provide detail on the implications of making this decision and how any foreseen problems can be mitigated}

<!-- This is an optional element. Feel free to remove. -->
### Confirmation

{Describe how the implementation of/compliance with the ADR is confirmed. E.g., by a review or an ArchUnit test.
 Although we classify this element as optional, it is included in most ADRs.}

<!-- This is an optional element. Feel free to remove. -->
## Pros and Cons of the Options

### Allow Access Tokens to be used in all API requests

With this approach we make it the re-user's responsibility to secure their Access Tokens. Our APIs would support CORS so that API calls can be made directly from the browser. The consequence of this is that the re-user may choose to store the Access Token in an insecure way.

This can be mitigated to some extent by enabling the [Revoke Refresh Token](https://www.keycloak.org/docs/latest/server_admin/index.html#_offline-access) option so that a Refresh Token can only be used once. However, there are still attacks that can get around this.

* Good: Simplest to implement. API calls are the same whether from a web application, mobile application or backend
* Bad: Open to XSS attacks or others depending on how the Access / Refresh Token is stored

### Only allow Authenticated APIs to be called from backends

This could be achieved by disabling CORS on our authenticated APIs and only creating non-public clients in Keycloak, thus preventing Implicit or PKCE login flows.

* Good: There is no incentive for a re-user to store an Access Token in the browser
* Neutral: Increased latency (but this only applies to authenticated actions like product updates where the payload is small)
* Bad: Web applications will always need a corresponding backend to obtain Access Tokens and call authenticated APIs

<!-- This is an optional element. Feel free to remove. -->
## More Information

{You might want to provide additional evidence/confidence for the decision outcome here and/or
 document the team agreement on the decision and/or
 define when/how this decision the decision should be realized and if/when it should be re-visited.
Links to other decisions and resources might appear here as well.}