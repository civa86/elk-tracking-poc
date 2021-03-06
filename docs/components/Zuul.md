[Documentation](../../README.md#documentation)

# [Zuul] Edge Service

Gateway that provides dynamic routing, monitoring, resiliency, security, and more.

**Official Repository**

[`https://github.com/Netflix/zuul`](https://github.com/Netflix/zuul)

## Routing

Edge Service is the single entrypoint to access API.

It receives all requests, check on user authentication with a Remote Token Service and use the private internal network to communicate with other services.

**Endpoint**

[`http://localhost:8080`](http://localhost:8080)

| Route              | Target Service |
| ------------------ | -------------- |
| /uaa/**            | auth-service   |
| /hash-service/**   | hash-service   |
| /photo-service/**  | photo-service  |

## Routes Authentication Configuration

Edge service has a `@EnableResourceServer` annotation to enable routes authentication.

All routes behind Edge Service are protected, only actuator endpoints are exposed without checking for an access token.

If a specific role is required for a URL, configration can be written in `application.yml`

```yml
zuul:
  auth:
    rules:
      - RULE_KEY:
          path: SERVICE_PATH
          role: REUIRED_ROLE
      - ...
      - ...
```

## Zuul Configuration

This is the zuul configuration inside the edge-service `application.yml` properties.

```yml
zuul:
  routes:
    uaa:
      path: /uaa/**
      sensitive-headers: true
      serviceId: auth-service
    hash:
      path: /hash-service/**
      serviceId: hash-service
    photo:
      path: /photo-service/**
      serviceId: photo-service
  auth:
    rules:
      - hash:
          path: /hash-service/**
          role: USER
      - photo:
          path: /photo-service/**
          role: USER
```

## Remote Token Service

To verify an access token, `Auth Service` is called internally defining a `RemoteTokenServices` bean in the `ResourceServerConfigurerAdapter` configuration class.

Client Credentials (clientId, clientSecret) inside `application.yml` file are required to use the remote token verification and they have to match with ones defined in the Auth Service application properties.

## Authentication Filter

When a token is validated, a ZuulFilter asks for the user associated with the access token and it will decorate the internal request with a `X-FORWARDED-USER-ID` header.

Thanks to the decorated header, every service can filter data depending on the user that is requesting them.