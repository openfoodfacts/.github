# Should we use TLS for communication between internal services (behind the reverse proxy)?

## Context and Problem Statement

[Current recommendations](https://cheatsheetseries.owasp.org/cheatsheets/Microservices_Security_Cheat_Sheet.html#mutual-transport-layer-security) for microservice architectures recommend the use of Mutual Transport Layer Security (mTLS) for communication between all internal services.

TLS provides two levels of protection:

1. Data exchanged between the client and server is encrypted
2. By signing data with a private key the server / client can prove they are who they say they are

This stops malicious actors from monitoring unencrypted communication and prevents "man in the middle" attacks, where a rogue system impersonates the intended server / client.

However, this kind of deployment can be complicated and generally requires some kind of Service Mesh when deployed in a Kubernetes cluster. Despite this, we might want to consider using TLS internally when communicating with servers containing sensitive information, such as Keycloak and its associated PostgreSQL database.

## Decision Drivers

* Security should be good enough for our domain
* Approach should not have a significant impact on performance
* It needs to be easy to develop and test

## Considered Options

* Use unsecured communication internally, terminating TLS at the reverse proxy
* Sensitive services require TLS internally, with a different certificate from the public one
* Sensitive services require TLS internally, with the same certificate as the public one

## Decision Outcome

The current decision is to continue to use unsecured communication internally as we have a high level of trust in our current data centres and so the additional complexity of securing communication between individual services can't be justified.

If a bad actor were to gain access to any of the current containers then they would potentially be able to access certificate keys and other secrets directly anyway.

### Consequences

Services can assume that TLS is not needed for internal communication and that network security will be handled by the infrastructure where needed.

## Pros and Cons of the Options

### Use unsecured communication internally

This matches the current model, where unsecured communication is used internally, with TLS terminated at then nginx reverse proxy. Communication between networks is carried out over SSH tunnels. If we are using a data centre that is not fully trusted then SSH tunnels should also be used for communication between hosts.

* Good: Matches current deployment model
* Good: No need to use TLS during development
* Good: Communication between data centres is encrypted and cannot be monitored
* Good: Can do full request logging at the reverse proxy
* Bad: The authenticity of the intended recipient is not guaranteed, allowing "man in the middle" attacks if the infrastructure has been compromised
* Bad: Internal communication is not encrypted so can be monitored if a bad actor gets network access

### Use SSL with internal certificates

In this case we would use our own Public Key Infrastructure (PKI) to issue internal certificates for any services that require TLS. Any clients needing access to these services would need to trust our internal Certificate Authority (CA).

* Good: Prevents basic "man in the middle" and network monitoring
* Good: Private keys for public certificates only need to be deployed to the reverse proxy
* Bad: Traffic would need to be re-encrypted at the reverse proxy, adding some latency
* Bad: Requires developers to use TLS internal for accurate integration testing
* Bad: Need to implement mechanisms to renew certificates

### Use SSL with public certificates

In this case we would use the public certificates for sensitive services for internal communication too. This would require internal DNS configuration to ensure that external host names can also be used internally.

* Good: Prevents basic "man in the middle" and network monitoring
* Good: Reverse proxy can operate in pass through mode avoiding re-encryption
* Good: Private keys for public certificates only need to be deployed to target service
* Good: No need to install our CA certificate on all clients
* Bad: Not possible to log request information on the reverse proxy
* Bad: Need to implement mechanisms to renew certificates on each service
* Bad: Requires developers to use TLS internal for accurate integration testing
* Bad: We would still need an internal PKI to issue certificates for services that do not need to be exposed externally, such as the Keycloak PostgreSQL database

