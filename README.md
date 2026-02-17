# Production-Grade E-Commerce Microservices Platform

A distributed, event-driven microservices architecture built with **Java 17, Spring Boot 3, Spring Cloud, Kafka, Kafka Streams, Docker, and Kubernetes**.

This project demonstrates a real-world, industry-standard implementation of a scalable e-commerce backend, designed for reliability, resilience, and high availability.

## 🚀 Key Features

*   **Microservices Architecture**: 7 business services, 3 infrastructure services, API Gateway, Service Discovery.
*   **Event-Driven & Async**: Kafka for decoupled communication, **Kafka Streams** for real-time inventory analytics.
*   **Distributed Transactions**:
    *   **Orchestrated SAGA** (Order Placement): ACIDs transactions across services (Order → Inventory → Payment).
    *   **Choreography SAGA** (Return/Refund): Loose coupling for returns efficiently.
*   **CQRS Pattern**: Command Query Responsibility Segregation for high-performance reads (Product Catalog).
*   **Resilience**: Circuit Breakers (Resilience4j), Retries, Rate Limiting (Redis-backed), Bulkheads.
*   **Security**: OAuth2 / OpenID Connect (Keycloak) with JWT Resource Servers.
*   **Observability**: Distributed Tracing (Micrometer Tracing/Zipkin), Metrics (Prometheus), Logs (Loki), Dashboards (Grafana).
*   **GitOps & IaC**: Terraform for AWS (EKS, RDS, MSK), Helm Charts, GitHub Actions CI/CD.

## 🏗️ Architecture

### Services
| Service | Tech Stack | Responsibility | DB |
| :--- | :--- | :--- | :--- |
| **API Gateway** | Spring Cloud Gateway, Netty | Edge server, routing, rate limiting, auth | - |
| **User Service** | Java 17, PostgreSQL | Identity, profiles, address book | Postgres |
| **Product Service** | Java 17, MongoDB | Catalog management, search | MongoDB |
| **Inventory Service** | **Kafka Streams**, MongoDB | Stock mgmt, real-time analytics | MongoDB |
| **Order Service** | Java 17, PostgreSQL | **SAGA Orchestrator**, order lifecycle | Postgres |
| **Payment Service** | Java 17, PostgreSQL | Payment processing, refund (Choreography) | Postgres |
| **Cart Service** | **RestClient**, Redis | Shopping cart, session mgmt | MongoDB + Redis |
| **Notification Service** | Kafka Consumer | Email/SMS alerts (No inbound REST) | MongoDB |

### Communication Strategy
*   **Synchronous**:
    *   **OpenFeign**: Order Service (legacy pattern example).
    *   **RestClient + Http Exchange**: Cart Service (modern Spring 6 pattern).
*   **Asynchronous (Event-Driven)**:
    *   App Apache Kafka is the backbone for inter-service events (`order-events`, `payment-events`, etc.).

## 🛠️ Tech Stack

*   **Language**: Java 17
*   **Framework**: Spring Boot 3.2.5, Spring Cloud 2023.0.1
*   **Messaging**: Apache Kafka, Kafka Streams
*   **Databases**: PostgreSQL, MongoDB, Redis
*   **Infrastructure**: Docker, Kubernetes (EKS), Terraform, Helm
*   **Observability**: Prometheus, Grafana, Loki, Tempo, OpenTelemetry

## 🏃‍♂️ Running Locally

### Prerequisites
*   Java 17+
*   Docker & Docker Compose
*   Maven 3.8+

### Steps
1.  **Clone the repository**:
    ```bash
    git clone https://github.com/ajeet7yadav25/microservices-antigravity.git
    cd microservices-antigravity
    ```

2.  **Start Infrastructure (Database, Kafka, Redis)**:
    ```bash
    # Coming in Phase 3
    docker-compose up -d
    ```

3.  **Build & Run Services**:
    ```bash
    mvn clean install
    # Run individual services via IDE or java -jar
    ```

## 📚 Module Structure
```
microservices-antigravity/
├── common/                 # Shared DTOs, Exceptions, Security Lib
├── platform/
│   ├── api-gateway/        # Edge Server
│   ├── service-discovery/  # Eureka Server
│   └── config-server/      # Centralized Config
└── services/
    ├── user-service/       # Identity Domain
    ├── product-service/    # Catalog Domain
    ├── order-service/      # Order Domain (SAGA Orchestrator)
    ├── ...
```

## 📝 License
MIT License
