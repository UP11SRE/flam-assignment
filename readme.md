## Ecommerce Project for FLAM Assignment

This project implements a full-fledged e-commerce platform with microservice architecture for a FLAM assignment.

### Services

The project consists of seven microservices:

1. **User Service (Port 8080):** Handles user registration, login, and profile updates. It utilizes JWT-based role-based authentication and connects to a PostgreSQL database hosted on Render.
2. **Product Service (Port 8081):** Provides functionalities to retrieve all products, get a single product, add, update, or delete products (admin only). Similar to the User Service, it uses PostgreSQL on Render and leverages RabbitMQ as a message broker. Additionally, the Product Service allows users to upload product images using Multer. Uploaded images are compressed and their URLs are stored in the database.
3. **Order Service (PORT 8082):** Enables users to create orders, check order details, view history, and cancel orders. This service uses Sequelize as ORM and interacts with the Product Service via HTTP calls to verify product availability before placing the order. Payment processing is handled through Stripe.
4. **Payment Service (PORT 8083):** Integrates with Stripe for secure payment processing. It receives payment information from the Order Service and returns a redirect URL upon successful payment or cancellation.
5. **Intercommunication Service (PORT 8084):** Acts as a bridge between microservices. It facilitates communication between Order Service, Product Service, and potentially more in the future. Additionally, it attempts user notifications through Nodemailer.
6. **API Gateway (Port 8087):** The main entry point for all API requests. It utilizes JWT authentication locally and integrates with Consul for service discovery. Prometheus collects metrics for monitoring purposes, visualized using Grafana. Opossum circuit breaker and Axios retry are implemented for resiliency. Nginx acts as the load balancer.
7. **Configuration Management Service (PORT 5000):** Provides a centralized configuration point for all microservices. Configurations are stored in Redis hosted on Render and retrieved by each service using a script during startup.

### Technologies

* Programming Language: Node.js
* Database: PostgreSQL (hosted on Render)
* Message Broker: RabbitMQ
* Authentication: JWT (with role-based access)
* ORM: Sequelize
* Image Upload: Multer
* Image Compression: (implementation details not specified)
* Payment Gateway: Stripe
* Service Discovery: Consul (hosted on Docker)
* API Gateway: Nginx (hosted on Docker)
* Monitoring: Prometheus (hosted on Docker), Grafana (hosted on Docker)
* Resiliency: Opossum circuit breaker, Axios retry
* Logging: Winston, Loki
* Configuration Management: Redis (hosted on Render)
* Orchestration: Docker Compose

### API Endpoints (API Gateway)

**User Service:**

* User Registration: `localhost:8087/api/gateway/register`
* User Login: `localhost:8087/api/gateway/login`
* User Profile Update: `localhost:8087/api/gateway/update`

**Product Service:**

* Get All Products: `localhost:8087/api/gateway/products/getall`
* Get a Product Details: `localhost:8087/api/gateway/products/getbyid/:id`
* Delete the Product: `localhost:8087/api/gateway/products/delete` (admin only)
* Add the Product (with image upload): `localhost:8081/api/products/add` (admin only)
* Update the Product: `localhost:8081/api/products/update/:productId` (admin only)

**Order Service:**

* Place the Order: `localhost:8087/api/gateway/placeorder`
* Get order details: `localhost:8087/api/gateway/orders/:orderid`
* Get Order History: `localhost:8087/api/gateway/orders/history`
* Cancel The Order: `localhost:8087/api/gateway/orders/cancel/:orderid`

**Note:** 

* These are example endpoints. Endpoints for other services may follow a similar structure under `/api/gateway`.
* The payment link uses a URL shortener to improve user experience.

### Getting Started

* Clone the project repository.
* Use Docker Compose to manage and run all services simultaneously.
