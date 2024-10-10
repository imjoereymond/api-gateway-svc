# API Gateway Service (api-gateway-svc)

This project contains the API Gateway Service, which acts as the entry point for client requests in a microservices architecture. It routes requests to various backend microservices, manages centralized authentication, and applies routing logic. This service is configured using a Spring Cloud Config Server and is registered with Eureka for service discovery.

## Features

- Routes API requests to appropriate backend services.
- Uses Eureka for service discovery and registration.
- Fetches its configurations from a centralized Spring Cloud Config Server.
- Runs on port `9093`.
- Integrated with Spring Cloud Gateway.

## Configuration Management

The `api-gateway-svc` fetches its configurations from a Spring Cloud Config Server, which retrieves configurations from a Git repository.

### Spring Cloud Config Setup

The `api-gateway-svc` connects to the config server using the following properties:

```yaml
spring:
  application:
    name: api-gateway-svc
  config:
    import: optional:configserver:http://localhost:8888
  cloud:
    config:
      username: welljr
      password: <your-password>
```

- **Config Server URL**: `http://localhost:8888`
- **Git Repository**: A repository containing configuration files for the services.
- **Branch**: `development`

## Eureka Registration

The API Gateway Service is registered with Eureka for service discovery and can be viewed in the Eureka dashboard running on port `8761`.

### Eureka Client Configuration

The following configuration ensures that the `api-gateway-svc` is registered with Eureka:

```yaml
eureka:
  client:
    serviceUrl:
      defaultZone: http://localhost:8761/eureka/
```

## Example `application.yml`

```yaml
server:
  port: 9093

spring:
  application:
    name: api-gateway-svc

eureka:
  client:
    serviceUrl:
      defaultZone: http://localhost:8761/eureka/

management:
  endpoints:
    web:
      exposure:
        include: "*"
  security:
    enabled: false
```

## Running the API Gateway

### Prerequisites

- Ensure that the Config Server is running and accessible.
- Ensure that Eureka Server is running on port `8761`.

### Steps to Run

1. Clone the repository:

   ```bash
   git clone <your-repo-url>
   cd <api-gateway-directory>
   ```

2. Build and run the service:

   ```bash
   ./mvnw spring-boot:run
   ```

3. The API Gateway service will be available at `http://localhost:9093`.

## Accessing the Eureka Dashboard

To see the API Gateway service registered in Eureka, open the following link in your browser:

```
http://localhost:8761
```

Here, you can view all registered microservices including the `api-gateway-svc`.

## Troubleshooting

- **Service not showing in Eureka**: Verify that the Eureka Server is running and the service has been registered.
- **Config properties not loading**: Ensure that the Config Server is running and reachable at `http://localhost:8888`.
- **Incorrect routing**: Double-check routing rules and backend service configurations.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for more details.
