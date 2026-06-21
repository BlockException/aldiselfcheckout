# Aldi Selbstbediener Kassentechnik - Enterprise Microservice Architektur

Eine komplexe, multi-language Microservice-Architektur für eine Aldi Selbstbediener Kasse.

---

## Systemübersicht

```
┌───────────────────────────────────────────────────────────────────────────────────────────────┐
│                                       Observability Stack                                     │
│  ┌──────────────────┐  ┌──────────────────┐  ┌──────────────────┐  ┌──────────────────┐     │
│  │     Prometheus   │  │     Grafana      │  │      Jaeger      │  │      Consul      │     │
│  └──────────────────┘  └──────────────────┘  └──────────────────┘  └──────────────────┘     │
└───────────────────────────────────────────────────────────────────────────────────────────────┘
                                                                          │
                                                                          ▼
┌───────────────────────────────────────────────────────────────────────────────────────────────┐
│                                     Orchestrator (NestJS)                                      │
│  ┌──────────────────────────────────────────────────────────────────────────────────────────┐ │
│  │  • CQRS Pattern                                                                           │ │
│  │  • Event Sourcing                                                                         │ │
│  │  • Saga Pattern                                                                           │ │
│  └──────────────────────────────────────────────────────────────────────────────────────────┘ │
└───────────────────────────────────────────────────────────────────────────────────────────────┘
                                                                          │
                    ┌───────────────────────────────────────────────────────────────┐
                    │                                                               │
                    ▼                                                               ▼
┌──────────────────────────────────────────────┐   ┌───────────────────────────────────────────┐
│          Product Service (Java/Spring)       │   │          Cart Service (Ruby/Rails)        │
│  • JPA + PostgreSQL                          │   │  • MongoDB + Mongoid                     │
│  • Redis Caching                             │   │  • Observer Pattern                       │
│  • Strategy Pattern (Pricing)                │   │  • Decorator Pattern                      │
└──────────────────────────────────────────────┘   └───────────────────────────────────────────┘
                    │                                                               │
                    │                                                               │
                    └───────────────────────────────────────────────────────────────┘
                                                              │
                                                              ▼
┌───────────────────────────────────────────────────────────────────────────────────────────────┐
│                                     Payment Service (Go)                                       │
│  • Gin Web Framework                                                                          │
│  • Factory Pattern (Payment Processors)                                                       │
│  • Repository Pattern                                                                         │
└───────────────────────────────────────────────────────────────────────────────────────────────┘
                                                              │
                                                              ▼
┌───────────────────────────────────────────────────────────────────────────────────────────────┐
│                                   Inventory Service (C++)                                      │
│  • High-Performance Shared-Lock                                                               │
│  • cpprestsdk (Casablanca)                                                                    │
└───────────────────────────────────────────────────────────────────────────────────────────────┘
                                                              │
                                                              ▼
┌───────────────────────────────────────────────────────────────────────────────────────────────┐
│                                    Fraud Service (Rust)                                        │
│  • Actix-Web                                                                                  │
│  • Unit of Work Pattern                                                                       │
│  • Rule-Based Fraud Detection                                                                 │
└───────────────────────────────────────────────────────────────────────────────────────────────┘
                                                              │
                                                              ▼
┌───────────────────────────────────────────────────────────────────────────────────────────────┐
│                                Discount Engine (Python/FastAPI)                                │
│  • Rule Engine (Discount Rules)                                                               │
│  • Complex Discount Logic                                                                     │
└───────────────────────────────────────────────────────────────────────────────────────────────┘
                                                              │
                                                              ▼
┌───────────────────────────────────────────────────────────────────────────────────────────────┐
│                                  Audit Service (Kotlin/Spring)                                │
│  • Kafka Streams                                                                              │
│  • Event Auditing                                                                             │
└───────────────────────────────────────────────────────────────────────────────────────────────┘
                                                              │
                                                              ▼
┌───────────────────────────────────────────────────────────────────────────────────────────────┐
│                                         Event Stack                                           │
│  ┌──────────────────┐  ┌──────────────────┐  ┌──────────────────┐  ┌──────────────────┐    │
│  │     Kafka        │  │     RabbitMQ     │  │    PostgreSQL    │  │    MongoDB       │    │
│  └──────────────────┘  └──────────────────┘  └──────────────────┘  └──────────────────┘    │
└───────────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## Tech Stack

| Kategorie               | Technologien                                                                 |
|-------------------------|-----------------------------------------------------------------------------|
| Programmiersprachen     | TypeScript, Java, Kotlin, Ruby, Go, C++, Rust, Python                      |
| Frameworks              | NestJS, Spring Boot, Ruby on Rails, Gin, cpprestsdk, Actix-Web, FastAPI   |
| Datenbanken             | PostgreSQL, MongoDB, Redis                                                  |
| Event-Streaming         | Kafka, RabbitMQ                                                            |
| Observability           | Prometheus, Grafana, Jaeger, OpenTelemetry                                 |
| Orchestrierung          | Docker Compose, Kubernetes, Consul (Service Discovery)                     |
| IaC                     | Terraform                                                                   |
| CI/CD                   | GitHub Actions                                                              |
| Design Patterns         | CQRS, Event Sourcing, Saga, Strategy, Factory, Repository, Observer, Decorator, Unit of Work |

---

## Projekt-Struktur

```
c:\Dev\complexes\
├── docker-compose.yml                  # Alle Services + Infrastruktur
├── README.md                           # Diese Datei
├── .github\workflows\
│   └── ci-cd.yml                       # GitHub Actions CI/CD Pipeline
├── services\
│   ├── orchestrator\                   # NestJS Orchestrator (CQRS/Event Sourcing)
│   ├── product-service\                # Java/Spring Boot Product Service
│   ├── cart-service\                   # Ruby/Rails Cart Service
│   ├── payment-service\                # Go Payment Service
│   ├── inventory-service\              # C++ Inventory Service
│   ├── fraud-service\                  # Rust Fraud Detection Service
│   ├── discount-engine\                # Python Discount Engine
│   └── audit-service\                  # Kotlin/Spring Audit Log Service
├── shared\
│   └── protos\                         # gRPC/Protobuf Definitions
├── k8s\                                # Kubernetes Manifeste
│   └── orchestrator-deployment.yml
├── terraform\                          # Terraform Infrastruktur-Code
│   └── main.tf
└── monitoring\
    └── prometheus.yml                  # Prometheus Konfiguration
```

---

## Lokale Ausführung (Docker Compose)

### Voraussetzungen
- Docker & Docker Compose installiert

### Ausführen
```bash
cd c:\Dev\complexes
docker-compose up -d
```

### Verfügbare Endpoints
| Service                     | URL                                  |
|-----------------------------|--------------------------------------|
| Orchestrator REST API       | http://localhost:3000/api/v1/checkouts |
| Orchestrator Swagger        | http://localhost:3000/api             |
| Product Service             | http://localhost:8080/api/v1/products |
| Cart Service                | http://localhost:3001/api/v1/carts     |
| Payment Service             | http://localhost:8081/api/v1/payments |
| Inventory Service           | http://localhost:8082/api/v1/inventory |
| Fraud Service               | http://localhost:8083/api/v1/fraud/check |
| Discount Engine             | http://localhost:5000/api/v1/discounts/calculate |
| Audit Service               | http://localhost:8084/api/v1/audit     |
| Grafana                     | http://localhost:3002                  |
| Prometheus                  | http://localhost:9090                  |
| Jaeger                      | http://localhost:16686                 |
| Consul                      | http://localhost:8500                  |

---

## Dokumentation

- [gRPC Protobuf Definitions](shared/protos/)
- [Kubernetes Manifeste](k8s/)
- [Terraform Code](terraform/)

---

## Über das Projekt

Diese Architektur wurde entwickelt, um eine skalierbare und robuste Lösung für Selbstbedienerkassen zu bieten.

- 8 Programmiersprachen
- 10+ Design Patterns
- Enterprise-grade Infrastruktur
- Observability from Day 1
