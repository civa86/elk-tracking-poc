# [P.O.C.] Microservices Architecture

Proof of Concept of a Microservices Architecture.

* Microservices implementation with [Spring Boot](https://spring.io/projects/spring-boot) and [Netflix OSS](https://netflix.github.io/)
* Log centralization with [ELK Stack](https://www.elastic.co/elk-stack)
* Architecture running inside [Docker](https://www.docker.com/) with [Docker Compose](https://docs.docker.com/compose/)

## Documentation

Learn how to run the architecture inside docker containers, call API, configure services and develop single parts independently.

* Architecture Components
    * Infrastructure
        * [Eureka Service](/)
        * [Edge Service](/)
        * [Auth Service](/)
    * Microservices
        * [Hash Service](/)
* [API Usage](docs/api/README.md)
* [Run with Docker Compose](docs/docker-compose/README.md)
* [Development](docs/development/README.md)

---

## Architecture Scheme

![architecture](docs/architecture.jpg 'Architecture Scheme')