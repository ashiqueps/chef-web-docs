+++
title = "Architecture"

draft = false

gh_repo = "automate"
[menu]
  [menu.automate]
    title = "Architecture"
    parent = "automate/reference"
    identifier = "automate/reference/architecture.md Architecture"
    weight = 10
+++

## Automate 2 Architecture

![Automate 2 Architecture](/images/automate/a2-architecture-os.png)

## Component overview

### Automate Gateway

The Automate Gateway serves as the application layer of Chef Automate's architecture. All public-facing requests go through the gateway, and authentication/authorization occurs here.

### Deployment Service

This service collects the initial service configuration from the user. It does everything required to set up Chef Automate initially. The deployment service manages configuration patches, as well.

### Configuration Management Service

This service serves all configuration management related information to the API and user interface, including Chef Infra Server action data and Chef Infra Client run data.

### Ingest Service

This service is the primary ingress event handler for configuration management related events such as Chef Infra Client runs and Chef Infra Server actions. It also manages the data related to these domains, such as cleanup, migration, and index initialization.

### Compliance Service

This service handles InSpec and scans job-related data, including event ingestion and reporting.

### Notification Service

This service is responsible for sending notifications based on configured rules in response to events.

### License Control Service

This service provides policy information to the rest of the system derived from the license file. It also includes telemetry configuration.

### AuthZ Service

This service provides the API to determine which actions a requestor is allowed to take on in Chef Automate.

### AuthN Service

This service provides the API to verify a requestor is allowed to interact with Chef Automate.

### Teams Service

This service is an API for defining local teams used as part of the authorization model for Chef Automate.

### Users Service

This service manages users local to Chef Automate (as opposed to users defined in an external identity provider).

### Session Service

This service stands between the browser and Dex. It acts as an [OpenID Connect](http://openid.net/connect/) client to Dex, and uses the [Authorization Code Grant Flow](https://auth0.com/docs/api-auth/tutorials/authorization-code-grant).

### Secrets Service

Service securely stores credentials for other services.

### OpenSearch Sidecar Service

This service runs alongside OpenSearch. It provides standard OpenSearch functionality to monitor disk usage and handle index purges.

### Dex

[Dex](https://github.com/dexidp/dex) is a federated OpenID Connect (OIDC) provider that allows Automate to integrate with external identity providers via LDAP, SAML, or OpenID Connect.
