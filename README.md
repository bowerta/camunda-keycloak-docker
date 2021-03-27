# camunda-keycloak-docker

![GitHub](https://img.shields.io/github/license/bowerta/camunda-keycloak-docker) ![Docker Automated build](https://img.shields.io/docker/automated/bowerta/camunda-keycloak-docker) ![Docker Image Version (latest by date)](https://img.shields.io/docker/v/bowerta/camunda-keycloak-docker)

Dockerised Camunda service with Keycloak authentication

This project runs instances of Postgres, PGadmin, and Camunda.

The [Camunda Platform run](https://docs.camunda.org/manual/latest/user-guide/camunda-bpm-run/) version of the Camunda image is used here, and the [Keycloak Camunda Identity Provider Plugin version 2.0.0](https://github.com/camunda/camunda-bpm-identity-keycloak) is copied into the configurations directory in the build process.

This image is available on Docker Hub at [bowerta/camunda-keycloak-docker](https://hub.docker.com/r/bowerta/camunda-keycloak-docker).

## Prerequisites
* Docker
* A running instance of Keycloak, configured as described [here](https://github.com/camunda/camunda-bpm-identity-keycloak) with
    * A client with access type set to `Confidential`
    * A service account enabled for the client with the following service roles assigned (either from `master-realm` or from `realm-management`)
        * `query-users`
        * `query-groups`
        * `view-users`
    * A group created named `camunda-admin`

## Configuration
The docker-compose file copies two files into the Camunda container `conf/default.yml` and `conf/production.yml`.

These files both need to be configured for the identity server, e.g.

```YAML
plugin.identity.keycloak:
  keycloakIssuerUrl: https://my-keycloak-service.com/auth/realms/my-realm
  keycloakAdminUrl: https://my-keycloak-service.com/auth/admin/realms/my-realm
  clientId: my-camunda-client-id
  clientSecret: my-client-secret
  useUsernameAsCamundaUserId: true
  useGroupPathAsCamundaGroupId: true
  administratorGroupName: camunda-admin
  disableSSLCertificateValidation: true
```

In this example, anything prepended with 'my-' needs to be modified.

## Supplementary services
The docker-compose file contains configurations for Postgres and PGadmin. The credentials for these services can be set in `env/db.dev.env` and `env/pgadmin.dev.env` respectively. Database credentials used by the Camunda service can be found in `env/camunda.dev.env`

## Usage
This example is run using docker-compose, to build and run this service the following command can be used.

```
docker-compose up --build
```

## Contributing

Contributions / pull requests are welcome, if you notice any problems with this project please log an issue on this GitHub repository.

## License

[MIT](https://choosealicense.com/licenses/mit/)
