# E-Commerce Application

Simple Spring Boot + MySQL REST API project designed for practicing:

- Java 17
- Spring Boot
- Spring Data JPA
- MySQL
- Maven
- Git/GitHub
- Jenkins CI
- AWS EC2 JAR deployment

## 1. Create MySQL database

```sql
CREATE DATABASE ecommerce_db;
```

Default local configuration:
- DB URL: `jdbc:mysql://localhost:3306/ecommerce_db`
- Username: `root`
- Password: `root`

Change these using environment variables if needed:

```text
DB_URL
DB_USERNAME
DB_PASSWORD
```

## 2. Run locally

```bash
mvn clean package
java -jar target/ecommerce-app-0.0.1-SNAPSHOT.jar
```

Application:
`http://localhost:8080`

Health:
`http://localhost:8080/actuator/health`

## 3. APIs

### Products

POST `/api/products`

```json
{
  "name": "Laptop",
  "description": "HP Laptop",
  "price": 55000,
  "quantity": 10
}
```

GET `/api/products`

GET `/api/products/1`

PUT `/api/products/1`

```json
{
  "name": "HP Laptop Updated",
  "description": "Updated product",
  "price": 60000,
  "quantity": 8
}
```

DELETE `/api/products/1`

### Customers

POST `/api/customers`

```json
{
  "name": "Sreevani",
  "email": "sreevani@example.com",
  "phone": "9876543210"
}
```

GET `/api/customers`

GET `/api/customers/1`

### Orders

POST `/api/orders`

```json
{
  "customerId": 1,
  "productId": 1,
  "quantity": 2
}
```

GET `/api/orders`

GET `/api/orders/1`

GET `/api/orders/customer/1`

Creating an order checks stock, reduces product quantity, calculates total amount, and saves the order in one transaction.

## 4. Git

```bash
git init
git add .
git commit -m "Initial e-commerce application"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/ecommerce-app.git
git push -u origin main
```

## 5. Jenkins

Create a Pipeline job.

Use:

```text
Definition: Pipeline script from SCM
SCM: Git
Branch: */main
Script Path: Jenkinsfile
```

Configure GitHub credentials in Jenkins.

First test with:

```text
Build Now
```

## 6. GitHub webhook

Jenkins job:

```text
Configure
-> Build Triggers
-> GitHub hook trigger for GITScm polling
```

GitHub:

```text
Settings
-> Webhooks
-> Add webhook
```

Payload URL:

```text
http://EC2_PUBLIC_IP:8080/github-webhook/
```

Content type:

```text
application/json
```

Select push events.

## 7. EC2

Install Java, Git and Maven.

Clone the project or receive the JAR from Jenkins.

Example:

```bash
java -version
git --version
mvn -version
```

Create the MySQL database and configure environment variables.

Run:

```bash
java -jar ecommerce-app-0.0.1-SNAPSHOT.jar
```

For a long-running server, use a process manager such as systemd rather than leaving an SSH terminal open.

## CI/CD flow

Developer
-> git push
-> GitHub
-> webhook
-> Jenkins
-> checkout
-> Maven build
-> tests
-> JAR
-> EC2 deployment
-> Spring Boot
-> MySQL
