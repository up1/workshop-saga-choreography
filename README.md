# Shopping Application 
* Spring Boot 3
* Java 21
* MongoDB
* RabbitMQ

## Overview ##
This project is an e-commerce platform built using a microservices architecture. It consists of three core microservices:

- **[Order Service](ms-order-service)**: Handles the creation and management of customer orders
- **[Product Service](ms-product-service)**: Manages product information and inventory stock
- **[Payment Service](ms-payment-service)**: Manages payment processing

## Architecture ##

Saga Choreography Pattern: Ensures data consistency across the microservices through distributed transactions.

* Each microservice performs its operations and publishes events that trigger actions in other microservices.
* No centralized coordinator is used; instead, each service listens for events and reacts accordingly.

Asynchronous Communication:
* Uses RabbitMQ to handle message passing between services.
* Enhances scalability and fault tolerance by decoupling services.

## Workflow ##

Order Placement:
* A customer places an order through the Order Service.
* The Order Service sends a message to the Product Service to check stock availability.

Stock Verification:
* The Product Service verifies stock and responds with availability status.
* If the product is available, the Product Service sends a payment request to the Payment Service.

Payment Processing:
* The Payment Service processes the payment via payment gateway
* If successful payment, the Payment Service sends a confirmation message back to the Order Service.

Order Completion:
* The Order Service finalizes the order and updates its status in MongoDB.

### EndPoints ###

| Service  | EndPoint                               | Port  | Method | Description                                     |
|----------|----------------------------------------|:-----:|:------:|-------------------------------------------------|
| Orders   | /v1/customers/{customerId}/orders      | 7500  |  POST  | Create an order                                 |
| Orders   | /v1/customers/{customerId}/orders      | 7500  |  GET   | Return a list of orders for a specific customer |
| Orders   | /v1/customers/{customerId}/orders/{id} | 7500  |  POST  | Return detail of specified order                |
| Products | /v1/products/{id}                      | 7501  |  GET   | Return detail of specified product              |
| Products | /v1/products                           | 7501  |  GET   | Return a list of products                       |
| Products | /v1/products                           | 7501  |  POST  | Insert a new product                            |
| Products | /v1/products/{id}                      | 7501  |  PUT   | Update a specific product                       |
| Products | /v1/products/{id}                      | 7501  | DELETE | Delete a specific product                       |


## RabbitMQ
Start server
```
docker compose up -d rabbitmq
```

You can open RabbitMQ
* http://localhost:15672/
    * username=guest
    * password=guest

## MongoDB
Start server
```
docker compose up -d mongodb
```

## Build & Run

### Build

Build all projects with Apache Maven

```
mvn clean install
```

### Build Docker images
```
docker compose build order-service
docker compose build payment-service
docker compose build product-service
```

### Run as a container

```
docker compose up -d order-service
```

## OpenApi for each services
- **order-service** : http://localhost:8080/swagger-ui.html
- **product-service** : http://localhost:8081/swagger-ui.html