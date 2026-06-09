# 3-Tier Architecture

A 3-tier architecture separates an application into three logical layers:

1. Presentation tier
2. Application tier
3. Data tier

This separation makes the application easier to develop, deploy, scale, secure, monitor, and maintain.

## Example Application

Example: E-commerce application

An online shopping application like Amazon, Flipkart, or a small shopping website can be built using 3-tier architecture.

```text
User Browser / Mobile App
        |
        v
Presentation Tier
React website hosted on S3 + CloudFront
        |
        v
Application Tier
Node.js / Java Spring Boot APIs running on Kubernetes
        |
        v
Data Tier
PostgreSQL database + Redis cache + S3 storage
```

## 1. Presentation Tier

The presentation tier is the user-facing layer of the application.

This is what users interact with directly. It can be a website, mobile app, or desktop interface.

Examples:

```text
React frontend
Angular frontend
HTML/CSS/JavaScript website
Android/iOS mobile app
```

In an e-commerce application, this tier includes:

```text
Home page
Product listing page
Product details page
Shopping cart
Login page
Checkout page
```

The presentation tier sends user requests to the application tier. For example, when a user clicks "Add to Cart", the frontend sends a request to the backend API.

### DevOps Work In Presentation Tier

DevOps engineers help build, test, deploy, and monitor the frontend.

Typical DevOps tasks:

```text
Set up CI/CD pipeline for frontend code
Build frontend assets
Run linting and unit tests
Deploy static files to web servers or CDN
Configure domain and SSL certificates
Set up caching for faster page loading
Monitor frontend availability and performance
Manage environment variables
Handle rollback if a bad frontend release happens
```

Example tools:

```text
GitHub Actions
Jenkins
GitLab CI
Nginx
Apache
AWS S3
CloudFront
Netlify
Vercel
Docker
Kubernetes
```

Example DevOps flow:

```text
Developer pushes React code to GitHub
CI pipeline runs tests
Pipeline builds production files
Files are deployed to S3
CloudFront serves the website globally
Users access the frontend through HTTPS
```

## 2. Application Tier

The application tier  the business logic layer.

This tier processes user requests, applies business rules, communicates with the database, and returns responses to the presentation tier.

Examples:

```text
Node.js API
Java Spring Boot service
Python Django API
Python Flask API
.NET backend
Go microservice
```

In an e-commerce application, this tier handles:

```text
User login and authentication
Product search
Add to cart logic
Order placement
Payment processing
Inventory checks
Discount calculation
Sending emails or notifications
```

For example, when a user places an order, the application tier checks if the product is available, calculates the price, validates payment, creates the order, and stores order details in the database.

### DevOps Work In Application Tier

DevOps engineers are heavily involved in deploying and operating backend services.

Typical DevOps tasks:

```text
Create CI/CD pipeline for backend services
Build application artifacts or Docker images
Run unit tests, integration tests, and security scans
Deploy backend to servers, containers, or Kubernetes
Configure load balancers
Manage environment-specific configuration
Set up autoscaling
Manage secrets such as API keys and database passwords
Monitor logs, metrics, and traces
Configure health checks
Perform blue-green or rolling deployments
Handle rollback during failures
```

Example tools:

```text
Docker
Kubernetes
Helm
Jenkins
GitHub Actions
GitLab CI
AWS ECS
AWS EKS
AWS EC2
Terraform
Ansible
Prometheus
Grafana
ELK
OpenSearch
Vault
AWS Secrets Manager
```

Example DevOps flow:

```text
Developer pushes backend code
CI pipeline runs tests
Docker image is built
Image is pushed to container registry
Kubernetes deployment is updated
Pods are restarted with the new image
Load balancer routes traffic to healthy pods
Monitoring checks API response time and errors
```

## 3. Data Tier

The data tier stores and manages application data.

This tier usually contains databases, caches, file storage, and sometimes message queues.

Examples:

```text
MySQL
PostgreSQL
MongoDB
Redis
Amazon RDS
DynamoDB
Elasticsearch
S3 object storage
```

In an e-commerce application, this tier stores:

```text
User accounts
Product details
Orders
Payments
Cart data
Inventory data
Reviews
Session data
```

For example, when a user searches for a product, the application tier may query the database or search engine to return matching products.

### DevOps Work In Data Tier

DevOps engineers help make the data tier reliable, secure, backed up, and scalable.

Typical DevOps tasks:

```text
Provision databases using Infrastructure as Code
Configure database networking and access control
Set up automated backups
Test backup restoration
Configure replication and high availability
Monitor database performance
Set up alerts for CPU, memory, disk, connections, and slow queries
Manage database credentials securely
Apply database patches and upgrades
Support database migration pipelines
Configure encryption at rest and in transit
Plan disaster recovery
Scale databases vertically or horizontally
Manage cache services like Redis
```

Example tools:

```text
Terraform
Ansible
AWS RDS
Amazon Aurora
Azure SQL
Cosmos DB
PostgreSQL
MySQL
MongoDB Atlas
Redis
ElastiCache
Prometheus
Grafana
CloudWatch
Vault
Secrets Manager
Flyway
Liquibase
```

Example DevOps flow:

```text
Terraform creates an RDS PostgreSQL database
Database credentials are stored in Secrets Manager
Backend application reads credentials securely
Automated backups run daily
Read replica is configured for scaling reads
CloudWatch alarms monitor CPU and storage
Database migration runs during deployment
```

## Request Flow Example

When a user views a product:

```text
User clicks "View Product"
Frontend sends API request to backend
Backend checks product details from database
Backend returns product data
Frontend displays product page
```

When a user places an order:

```text
User clicks "Place Order"
Frontend sends order request
Backend validates user and cart
Backend checks inventory
Backend processes payment
Backend saves order in database
Backend sends response to frontend
Frontend shows order confirmation
```

## DevOps Responsibilities Across All Tiers

DevOps does not usually write the main business code, but DevOps ensures the application can be built, deployed, secured, monitored, scaled, and recovered properly.

Common DevOps responsibilities:

```text
CI/CD pipeline setup
Infrastructure provisioning
Cloud resource management
Containerization
Kubernetes deployment
Monitoring and alerting
Logging
Security scanning
Secret management
Backup and disaster recovery
Performance optimization
Cost optimization
Release management
Incident response
Automation
```

## Simple Mapping

```text
Presentation tier = User interface
Example = React website
DevOps work = Deploy frontend to CDN, configure SSL, cache static files

Application tier = Business logic
Example = Java Spring Boot API
DevOps work = Build Docker image, deploy to Kubernetes, configure autoscaling

Data tier = Data storage
Example = PostgreSQL database
DevOps work = Provision RDS, configure backups, monitoring, security, and replication
```

## Another Example: Banking Application

Presentation tier:

```text
Mobile banking app
Web banking portal
```

Application tier:

```text
Backend APIs for login
Balance check service
Money transfer service
Statement generation service
```

Data tier:

```text
Customer database
Transaction database
Audit logs
```

DevOps work:

```text
Presentation tier:
Deploy web portal, manage CDN, SSL, and frontend monitoring

Application tier:
Deploy secure backend APIs, manage scaling, logging, secrets, and compliance checks

Data tier:
Manage encrypted databases, backups, replication, disaster recovery, and access control
```

## Why 3-Tier Architecture Is Useful

Benefits:

```text
Better separation of responsibilities
Easier development and maintenance
Independent scaling of each tier
Improved security
Easier troubleshooting
Better deployment management
Higher availability
```

In short:

```text
Presentation tier = What the user sees
Application tier = Business logic
Data tier = Where data is stored

DevOps role = Automate, deploy, secure, monitor, scale, and recover each tier
```

## Practical Flow With Load Balancers, Web Server, App Server, And Database

A common real-world 3-tier architecture looks like this:

```text
User / Browser
      |
      v
Internet / DNS
      |
      v
External Load Balancer
      |
      v
Web Server / Static Hosting
Nginx or S3 + CloudFront
      |
      v
Internal Load Balancer
      |
      v
Application Server
Node.js / Java / Python / .NET
      |
      v
Database
PostgreSQL / MySQL / MongoDB
```

Short form:

```text
Web --> Load Balancer --> Web Server / S3 --> Load Balancer --> App Server --> Database
```

### Step 1: User Opens The Website

The user enters the application URL in the browser.

Example:

```text
https://shop.example.com
```

The browser first contacts DNS to find where this website is hosted.

DNS converts the domain name into the address of the external load balancer or CDN.

Example:

```text
shop.example.com --> CloudFront / External Load Balancer
```

### Step 2: Request Reaches External Load Balancer

The external load balancer receives the user request.

The load balancer is responsible for distributing traffic across multiple web servers or frontend hosting targets.

Example:

```text
User request
      |
      v
External Load Balancer
      |
      +--> Web Server 1
      +--> Web Server 2
      +--> Web Server 3
```

Why load balancer is used:

```text
Distributes traffic
Improves availability
Avoids sending all users to one server
Sends traffic only to healthy servers
Supports SSL/TLS termination
Helps with zero-downtime deployment
```

### Step 3: Request Goes To Web Server Or Static Hosting

The web server serves the frontend application.

This can be:

```text
Nginx web server
Apache web server
AWS S3 static website hosting
CloudFront CDN
Azure Static Web Apps
Vercel
Netlify
```

If the frontend is a React, Angular, or Vue application, the web server usually returns static files:

```text
index.html
main.js
styles.css
images
fonts
```

Example:

```text
Browser requests homepage
Nginx returns index.html, JavaScript, CSS, and image files
Browser loads the frontend UI
```

At this point, the user can see the website page.

### Step 4: Frontend Calls Backend API

After the frontend loads in the browser, it may need data from the backend.

Example actions:

```text
Login
View products
Search products
Add item to cart
Place order
Check payment status
```

The frontend sends an API request.

Example:

```text
GET https://api.example.com/products
POST https://api.example.com/login
POST https://api.example.com/orders
```

This request goes to another load balancer, usually called an internal load balancer or application load balancer.

### Step 5: Request Reaches Internal Load Balancer

The internal load balancer receives API traffic and forwards it to healthy application servers.

Example:

```text
Frontend API request
      |
      v
Internal Load Balancer
      |
      +--> App Server 1
      +--> App Server 2
      +--> App Server 3
```

Why this second load balancer is used:

```text
Distributes backend API traffic
Protects app servers from direct public access
Supports autoscaling
Checks app server health
Routes requests to healthy backend instances
Allows rolling deployments with less downtime
```

### Step 6: Application Server Processes The Request

The application server contains the business logic.

Examples:

```text
Node.js backend
Java Spring Boot application
Python Django application
Python Flask application
.NET API
Go service
```

The app server handles logic such as:

```text
Validate user login
Check user permissions
Calculate order total
Apply discount
Check inventory
Create order
Call payment gateway
Send notification
```

Example:

```text
User clicks "Place Order"
Frontend sends order request
Load balancer sends request to App Server 2
App Server 2 validates user
App Server 2 checks cart items
App Server 2 calculates final price
App Server 2 stores order in database
```

### Step 7: Application Server Communicates With Database

The database stores application data.

Examples:

```text
PostgreSQL
MySQL
MongoDB
DynamoDB
Oracle
SQL Server
```

The application server sends queries to the database.

Example:

```text
SELECT product details
INSERT new order
UPDATE inventory
SELECT user account
INSERT payment record
```

The database returns the requested data or confirms that data was saved.

Example:

```text
App Server --> Database: Get product with ID 101
Database --> App Server: Product name, price, stock, image URL
```

## Request And Response Flow

### Example: User Views Product Page

Request flow:

```text
1. User opens https://shop.example.com
2. DNS sends request to external load balancer or CDN
3. Load balancer sends request to Nginx or S3/CloudFront
4. Web server returns frontend files to browser
5. Browser loads React/Angular/Vue application
6. Frontend calls backend API: GET /products/101
7. API request reaches internal load balancer
8. Internal load balancer forwards request to one app server
9. App server queries database for product 101
10. Database returns product data
```

Response flow:

```text
1. Database returns product data to app server
2. App server creates API response in JSON format
3. App server sends response to internal load balancer
4. Internal load balancer sends response back to frontend
5. Frontend receives JSON data
6. Browser displays product name, price, image, and stock
```

Final response example:

```json
{
  "id": 101,
  "name": "Laptop",
  "price": 55000,
  "stock": 12
}
```

### Example: User Places Order

Request flow:

```text
1. User clicks "Place Order"
2. Frontend sends POST /orders request
3. Request reaches internal load balancer
4. Load balancer forwards request to app server
5. App server validates login token
6. App server checks cart items
7. App server checks inventory in database
8. App server creates order in database
9. App server updates inventory
10. App server sends success response
```

Response flow:

```text
1. Database confirms order is saved
2. App server returns order confirmation
3. Load balancer sends response back to frontend
4. Frontend displays "Order placed successfully"
```

## CI/CD Flow In 3-Tier Architecture

CI/CD means Continuous Integration and Continuous Delivery or Continuous Deployment.

It automates the process of building, testing, and deploying new code.

### CI/CD Flow For Frontend

Frontend code example:

```text
React
Angular
Vue
HTML/CSS/JavaScript
```

Frontend CI/CD flow:

```text
1. Developer writes frontend code
2. Developer pushes code to GitHub
3. CI/CD pipeline starts automatically
4. Pipeline installs dependencies
5. Pipeline runs lint checks
6. Pipeline runs unit tests
7. Pipeline builds production frontend files
8. Build output is generated
9. Files are uploaded to S3, Nginx server, or CDN
10. CDN cache is invalidated if needed
11. Users receive the new frontend version
```

Example frontend build output:

```text
dist/
build/
index.html
assets/main.js
assets/style.css
```

If using S3 and CloudFront:

```text
GitHub --> CI/CD --> Build React App --> Upload To S3 --> Invalidate CloudFront Cache --> User Gets New UI
```

If using Nginx:

```text
GitHub --> CI/CD --> Build Frontend --> Copy Files To Nginx Server --> Reload Nginx --> User Gets New UI
```

### CI/CD Flow For Backend

Backend code example:

```text
Node.js API
Java Spring Boot API
Python API
.NET API
Go API
```

Backend CI/CD flow:

```text
1. Developer writes backend code
2. Developer pushes code to GitHub
3. CI/CD pipeline starts automatically
4. Pipeline installs dependencies
5. Pipeline runs unit tests
6. Pipeline runs integration tests
7. Pipeline runs security scan
8. Pipeline builds application artifact or Docker image
9. Docker image is pushed to container registry
10. Deployment tool updates app servers or Kubernetes
11. Load balancer health checks verify the new version
12. Traffic is sent to healthy new app instances
```

If using Docker and Kubernetes:

```text
GitHub
  |
  v
CI/CD Pipeline
  |
  v
Build Docker Image
  |
  v
Push Image To Registry
  |
  v
Update Kubernetes Deployment
  |
  v
New Pods Start
  |
  v
Health Checks Pass
  |
  v
Load Balancer Sends Traffic To New Pods
```

### CI/CD Flow For Database Changes

Database changes are usually handled carefully because data is very important.

Database CI/CD may include:

```text
Schema migration
Table creation
Column addition
Index creation
Data migration
Rollback planning
Backup before deployment
```

Tools:

```text
Flyway
Liquibase
Alembic
Prisma migrations
Sequelize migrations
Terraform
```

Database deployment flow:

```text
1. Developer creates database migration file
2. Migration file is committed to GitHub
3. CI/CD pipeline validates migration
4. Backup is checked or created
5. Migration runs against staging database
6. Tests are executed
7. Migration runs against production database
8. Application starts using the new database structure
```

## How New Code Is Updated In Production

### Frontend Update

Example: A developer changes the homepage design.

Flow:

```text
1. Developer updates React code
2. Developer commits and pushes code to GitHub
3. CI/CD pipeline builds the frontend
4. New static files are uploaded to S3 or Nginx
5. CDN cache is cleared or invalidated
6. Browser downloads the latest JavaScript and CSS
7. User sees the updated homepage
```

Important DevOps tasks:

```text
Run tests before deployment
Use versioned static files
Invalidate CDN cache
Monitor errors after release
Rollback to previous build if needed
```

### Backend Update

Example: A developer adds a new discount feature.

Flow:

```text
1. Developer updates backend code
2. Developer pushes code to GitHub
3. CI/CD pipeline runs tests
4. Pipeline builds new Docker image
5. Image is pushed to container registry
6. Kubernetes or deployment tool starts new app instances
7. Health checks verify the new instances
8. Load balancer slowly sends traffic to the new instances
9. Old instances are stopped after new ones are healthy
```

This is called a rolling deployment.

Rolling deployment example:

```text
Before deployment:
App Server 1 = old version
App Server 2 = old version
App Server 3 = old version

During deployment:
App Server 1 = new version
App Server 2 = old version
App Server 3 = old version

After health checks:
App Server 1 = new version
App Server 2 = new version
App Server 3 = old version

Final state:
App Server 1 = new version
App Server 2 = new version
App Server 3 = new version
```

### Database Update

Example: A developer adds a new column to store delivery instructions.

Flow:

```text
1. Developer creates migration file
2. Migration is tested in development
3. Migration is tested in staging
4. Backup is confirmed before production release
5. Migration runs in production
6. Backend code starts writing data to the new column
7. Monitoring checks database errors and slow queries
```

Example migration:

```sql
ALTER TABLE orders ADD COLUMN delivery_instructions TEXT;
```

DevOps must be careful with database updates because a wrong database change can affect live data.

## Complete End-To-End Flow

```text
Developer pushes code
      |
      v
GitHub repository
      |
      v
CI/CD pipeline starts
      |
      v
Run tests and security checks
      |
      v
Build frontend files or backend Docker image
      |
      v
Deploy to web server, S3, Kubernetes, or app servers
      |
      v
Load balancer checks health
      |
      v
Traffic goes to new healthy version
      |
      v
Monitoring checks logs, metrics, and errors
```

## DevOps Role In This Full Flow

DevOps engineers manage the automation and reliability of the complete path.

DevOps responsibilities:

```text
Configure DNS
Configure external and internal load balancers
Set up Nginx or static hosting
Create CI/CD pipelines
Build and push Docker images
Deploy application servers
Configure database access
Manage secrets and environment variables
Set up SSL certificates
Configure monitoring and alerting
Set up logging
Manage backups and disaster recovery
Handle rollback
Scale frontend, backend, and database layers
```

Simple summary:

```text
Request flow:
User --> Load Balancer --> Web Server --> App Load Balancer --> App Server --> Database

Response flow:
Database --> App Server --> App Load Balancer --> Web Server / Frontend --> User

CI/CD flow:
Developer --> GitHub --> Pipeline --> Build --> Test --> Deploy --> Health Check --> Live Traffic
```
