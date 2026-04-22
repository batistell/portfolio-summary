[🇺🇸 English](#english) | [🇧🇷 Português](#português)

---

<h1 id="english">🇺🇸 English</h1>

# 🚀 Portfolio Summary - Microservices Architecture

Welcome to my portfolio! This repository serves as an index and summary for my distributed event-driven microservices architecture ecosystem. Here you will find links to the repositories of each service, alongside the specific architecture and technology choices implemented in each.

## 🏗️ The Ecosystem

This architecture represents a robust, highly-performant E-commerce/Order Management environment using modern backend patterns.

---

### 📦 1. Catalog API
**Repository:** [batistell/catalog-api-java](https://github.com/batistell/catalog-api-java)

This service manages the product catalog and handles high read volumes, complex data aggregations, and orchestrates the creation of products pushing events to the ecosystem.

**Architecture & Technologies Implemented:**
- **Structured Concurrency (Java 21):** Utilizes `StructuredTaskScope` for orchestrating virtual threads cleanly. [See implementation](https://github.com/batistell/catalog-api-java/blob/main/src/main/java/com/batistell/catalogapi/controller/CatalogController.java)
- **Parallel CPU Processing:** Maximizes CPU logic cores with `parallelStream()` and `@Async` for mathematical aggregations. [See implementation](https://github.com/batistell/catalog-api-java/blob/main/src/main/java/com/batistell/catalogapi/service/CatalogAnalysisService.java)
- **Synchronous Direct Calls (OpenFeign):** Pulls synchronous stock values natively across the network. [See implementation](https://github.com/batistell/catalog-api-java/blob/main/src/main/java/com/batistell/catalogapi/client/InventoryClient.java)
- **Event-Driven Outbox (Kafka):** Non-blocking broadcast of events allowing downstream services to react without bottlenecking REST threads. [See implementation](https://github.com/batistell/catalog-api-java/blob/main/src/main/java/com/batistell/catalogapi/service/SearchServiceListener.java)
- **Circuit Breaker (Resilience4j):** Reroutes traffic to Redis cache to prevent timeouts if the primary database degrades. [See implementation](https://github.com/batistell/catalog-api-java/blob/main/src/main/java/com/batistell/catalogapi/service/CatalogService.java)

---

### 📦 2. Inventory API
**Repository:** [batistell/inventory-api-java](https://github.com/batistell/inventory-api-java)

Operates in the background, managing critical stock synchronizations and relational aggregations without relying on heavily abstracted ORM data mappings.

**Architecture & Technologies Implemented:**
- **Database Versioning (Flyway):** Deterministic SQL schema evolution replacing Hibernate's `ddl-auto`. [See implementation](https://github.com/batistell/inventory-api-java/blob/main/src/main/resources/db/migration/V1__init.sql)
- **Synchronous REST Relay (OpenFeign):** Interrogates the Catalog API to merge relational data with remote document data. [See implementation](https://github.com/batistell/inventory-api-java/blob/main/src/main/java/com/batistell/inventoryapi/client/CatalogClient.java)
- **Event-Driven Consumers (Kafka):** Silently provisions stock rows responding to external lifecycle events providing "Eventual Consistency". [See implementation](https://github.com/batistell/inventory-api-java/blob/main/src/main/java/com/batistell/inventoryapi/service/KafkaConsumerService.java)
- **Advanced Native Relational Analytics:** Executes deep aggregations via `@Query(nativeQuery = true)` avoiding memory overhead on the Java Heap. [See implementation](https://github.com/batistell/inventory-api-java/blob/main/src/main/java/com/batistell/inventoryapi/repository/InventoryRepository.java)

---

### 📦 3. Order API
**Repository:** [batistell/order-api-java](https://github.com/batistell/order-api-java)

The core transactional service. It demonstrates complex distributed transactions across the ecosystem utilizing Saga and Outbox concepts.

**Architecture & Technologies Implemented:**
- **Saga & Outbox Patterns (Kafka):** Implements consistency in distributed transactions pairing database inserts with Kafka message dispatching.
- **Structured Concurrency (Virtual Threads):** High-throughput, low-latency network/IO operations natively in Java 21 without blocking physical cores.
- **Synchronous Validations (OpenFeign):** Native declarative HTTP clients validating external stock and catalogs. [CatalogClient](https://github.com/batistell/order-api-java/blob/main/src/main/java/com/batistell/orderapi/client/CatalogFeignClient.java) & [InventoryClient](https://github.com/batistell/order-api-java/blob/main/src/main/java/com/batistell/orderapi/client/InventoryFeignClient.java)
- **Circuit Breakers & Bulkheads (Resilience4j):** Defends the core transactional system from incoming traffic surges and latent external dependencies. [See implementation](https://github.com/batistell/order-api-java/blob/main/src/main/java/com/batistell/orderapi/service/OrderValidationService.java)
- **Database Migrations (Flyway):** Validated scripts managing the Order context schemas securely. [See implementation](https://github.com/batistell/order-api-java/blob/main/src/main/resources/db/migration/V1__create_orders_schema.sql)

<br/><hr/><br/>

<h1 id="português">🇧🇷 Português</h1>

# 🚀 Resumo do Portfólio - Arquitetura de Microsserviços

Bem-vindo ao meu portfólio! Este repositório serve como um índice e resumo para meu ecossistema de microsserviços distribuídos e orientados a eventos. Aqui você encontrará os links para os repositórios de cada serviço, ao lado das escolhas específicas de arquitetura e tecnologia implementadas em cada um deles.

## 🏗️ O Ecossistema

Esta arquitetura representa um ambiente robusto e de alta performance para E-commerce e Gerenciamento de Pedidos, usando os padrões mais modernos de backend.

---

### 📦 1. Catalog API (API de Catálogo)
**Repositório:** [batistell/catalog-api-java](https://github.com/batistell/catalog-api-java)

Este serviço gerencia o catálogo de produtos, lidando com altos volumes de leitura, agregações complexas e orquestrando o envio de eventos de novos produtos ao ecossistema.

**Arquitetura e Tecnologias Implementadas:**
- **Concorrência Estruturada (Java 21):** Utiliza o `StructuredTaskScope` para orquestrar threads virtuais de forma limpa. [Ver implementação](https://github.com/batistell/catalog-api-java/blob/main/src/main/java/com/batistell/catalogapi/controller/CatalogController.java)
- **Processamento Paralelo de CPU:** Maximiza o uso de núcleos lógicos via `parallelStream()` e `@Async` para agregações matemáticas. [Ver implementação](https://github.com/batistell/catalog-api-java/blob/main/src/main/java/com/batistell/catalogapi/service/CatalogAnalysisService.java)
- **Chamadas Síncronas Diretas (OpenFeign):** Extrai dados de estoque via rede usando clientes HTTP nativos. [Ver implementação](https://github.com/batistell/catalog-api-java/blob/main/src/main/java/com/batistell/catalogapi/client/InventoryClient.java)
- **Event-Driven Outbox (Kafka):** Transmissões não-bloqueantes que permitem aos demais serviços reagirem sem gargalos na thread principal. [Ver implementação](https://github.com/batistell/catalog-api-java/blob/main/src/main/java/com/batistell/catalogapi/service/SearchServiceListener.java)
- **Circuit Breaker (Resilience4j):** Redireciona tráfego cirurgicamente para cache Redis prevenindo timeouts na queda do banco. [Ver implementação](https://github.com/batistell/catalog-api-java/blob/main/src/main/java/com/batistell/catalogapi/service/CatalogService.java)

---

### 📦 2. Inventory API (API de Estoque)
**Repositório:** [batistell/inventory-api-java](https://github.com/batistell/inventory-api-java)

Opera nos bastidores gerenciando sincronizações críticas de estoque e agregações relacionais sem depender de mapeamentos ORM pesados.

**Arquitetura e Tecnologias Implementadas:**
- **Versionamento de Banco (Flyway):** Evolução determinística via scripts SQL, aposentando o `ddl-auto` do Hibernate. [Ver implementação](https://github.com/batistell/inventory-api-java/blob/main/src/main/resources/db/migration/V1__init.sql)
- **Integração Síncrona via REST (OpenFeign):** Interroga o Catálogo para mesclar propriedades relacionais com o MongoDB. [Ver implementação](https://github.com/batistell/inventory-api-java/blob/main/src/main/java/com/batistell/inventoryapi/client/CatalogClient.java)
- **Consumidores Assíncronos (Kafka):** Provisiona silenciosamente linhas de estoque respondendo a eventos em modelo de "Consistência Eventual". [Ver implementação](https://github.com/batistell/inventory-api-java/blob/main/src/main/java/com/batistell/inventoryapi/service/KafkaConsumerService.java)
- **Queries Nativas e Análise Relacional:** Executa agregações profundas nativas no banco usando `@Query(nativeQuery = true)` poupando memória na Heap do Java. [Ver implementação](https://github.com/batistell/inventory-api-java/blob/main/src/main/java/com/batistell/inventoryapi/repository/InventoryRepository.java)

---

### 📦 3. Order API (API de Pedidos)
**Repositório:** [batistell/order-api-java](https://github.com/batistell/order-api-java)

O serviço transacional core. Ele demonstra orquestração de transações distribuídas ao longo de toda a arquitetura através de conceitos de Saga e Outbox.

**Arquitetura e Tecnologias Implementadas:**
- **Padrões Saga e Outbox (Kafka):** Mantém a consistência em transações distribuídas pareando inserções no banco com disparos no Kafka.
- **Concorrência Estruturada (Virtual Threads):** Alto throughput com baixa latência para I/O e rede de forma nativa no Java 21.
- **Validações Síncronas (OpenFeign):** Clientes HTTP declarativos e tipados testando validez de produtos e estoques remotamente. [CatalogClient](https://github.com/batistell/order-api-java/blob/main/src/main/java/com/batistell/orderapi/client/CatalogFeignClient.java) e [InventoryClient](https://github.com/batistell/order-api-java/blob/main/src/main/java/com/batistell/orderapi/client/InventoryFeignClient.java)
- **Circuit Breakers & Bulkheads (Resilience4j):** Defende o motor de pedidos contra tráfego excedente e latência vinda de integrações terceiras. [Ver implementação](https://github.com/batistell/order-api-java/blob/main/src/main/java/com/batistell/orderapi/service/OrderValidationService.java)
- **Migrations de Banco (Flyway):** Scripts validados gerenciando a integridade das tabelas de transação de pedidos. [Ver implementação](https://github.com/batistell/order-api-java/blob/main/src/main/resources/db/migration/V1__create_orders_schema.sql)