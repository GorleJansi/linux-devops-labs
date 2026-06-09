# 3-Tier Complete Deployment Example

This document explains a complete real-life 3-tier application deployment example from development to production.

Example project:

```text
Online Shopping Application
```

This application allows users to:

```text
Register and login
View products
Search products
Add items to cart
Place orders
Make payments
Track orders
```

## High-Level Architecture

```text
User Browser / Mobile App
        |
        v
DNS
        |
        v
CDN / External Load Balancer
        |
        v
Presentation Tier
React frontend hosted on S3 + CloudFront or Nginx
        |
        v
Internal Load Balancer
        |
        v
Application Tier
Backend APIs running on Kubernetes / EC2 / ECS
        |
        v
Data Tier
PostgreSQL database + Redis cache + S3 file storage
```

Short architecture flow:

```text
User --> DNS --> Load Balancer/CDN --> Web Server/S3 --> App Load Balancer --> App Server --> Database
```

## Technology Stack

For this example, assume the company is using AWS.

```text
Frontend:
React
HTML
CSS
JavaScript
S3
CloudFront

Backend:
Java Spring Boot or Node.js
Docker
Kubernetes or AWS ECS
Application Load Balancer

Database:
Amazon RDS PostgreSQL
Redis / ElastiCache
S3 for product images and invoices

DevOps:
GitHub
GitHub Actions or Jenkins
Docker
Terraform
Kubernetes
Helm
CloudWatch
Prometheus
Grafana
Secrets Manager
```

## Complete Application Components

### 1. Frontend Application

The frontend is the user interface.

Example pages:

```text
Home page
Login page
Product list page
Product details page
Cart page
Checkout page
Order history page
```

Frontend responsibilities:

```text
Display data to users
Collect user input
Call backend APIs
Show success and error messages
Manage browser session or token
Display order status
```

Example frontend API call:

```text
GET https://api.shop.example.com/products
POST https://api.shop.example.com/login
POST https://api.shop.example.com/orders
```

### 2. Backend Application

The backend contains business logic.

Backend services:

```text
User service
Product service
Cart service
Order service
Payment service
Notification service
```

Backend responsibilities:

```text
Authenticate users
Validate requests
Apply business rules
Calculate prices and discounts
Check inventory
Create orders
Call payment gateway
Send emails or SMS
Read and write database records
```

Example backend API endpoints:

```text
POST /login
GET /products
GET /products/{id}
POST /cart
POST /orders
GET /orders/{id}
```

### 3. Database Layer

The database stores permanent application data.

Example database tables:

```text
users
products
cart_items
orders
order_items
payments
inventory
audit_logs
```

Example data:

```text
User details
Product details
Order details
Payment status
Inventory count
Delivery address
```

Redis can be used for:

```text
Session storage
Frequently viewed products
Cart cache
API rate limiting
Temporary tokens
```

S3 can be used for:

```text
Product images
User profile images
Invoices
Reports
Static files
```

## Real-Life Request Flow

### Scenario 1: User Opens The Website

```text
1. User enters https://shop.example.com in the browser.
2. Browser asks DNS for the address of shop.example.com.
3. DNS returns CloudFront or external load balancer address.
4. Browser sends HTTPS request to CloudFront.
5. CloudFront checks if frontend files are cached.
6. If files are cached, CloudFront returns them immediately.
7. If files are not cached, CloudFront fetches files from S3 or Nginx.
8. Browser receives index.html, CSS, JavaScript, and images.
9. React application loads in the browser.
10. User sees the shopping website.
```

Response flow:

```text
S3/Nginx --> CloudFront/Load Balancer --> Browser --> User sees UI
```

### Scenario 2: User Views Product List

```text
1. React frontend calls GET /products.
2. Request goes to api.shop.example.com.
3. DNS routes API domain to application load balancer.
4. Application load balancer forwards request to a healthy backend server.
5. Backend validates request.
6. Backend checks Redis cache for product list.
7. If data is available in Redis, backend returns it quickly.
8. If not available, backend queries PostgreSQL.
9. PostgreSQL returns product data.
10. Backend stores data in Redis cache.
11. Backend returns product list as JSON.
12. Frontend displays products to the user.
```

Example JSON response:

```json
[
  {
    "id": 101,
    "name": "Laptop",
    "price": 55000,
    "stock": 12
  },
  {
    "id": 102,
    "name": "Phone",
    "price": 25000,
    "stock": 30
  }
]
```

### Scenario 3: User Places An Order

```text
1. User adds products to cart.
2. User clicks Place Order.
3. Frontend sends POST /orders request.
4. Request reaches application load balancer.
5. Load balancer forwards request to backend server.
6. Backend validates login token.
7. Backend checks user address and cart items.
8. Backend checks inventory in PostgreSQL.
9. Backend calculates total amount.
10. Backend calls payment gateway.
11. Payment gateway returns success or failure.
12. Backend creates order record in database.
13. Backend updates inventory.
14. Backend stores payment status.
15. Backend sends notification request.
16. Backend returns order confirmation to frontend.
17. Frontend shows order success page.
```

Response flow:

```text
Database --> Backend App --> App Load Balancer --> Frontend --> User
```

## Infrastructure Setup

DevOps usually creates infrastructure using Infrastructure as Code.

Example Terraform-created resources:

```text
VPC
Public subnets
Private subnets
Internet gateway
NAT gateway
Security groups
External load balancer
Internal load balancer
Kubernetes cluster or ECS cluster
RDS PostgreSQL database
Redis / ElastiCache
S3 bucket
CloudFront distribution
IAM roles
CloudWatch alarms
Secrets Manager entries
```

### Network Design

```text
Public subnet:
External load balancer
NAT gateway

Private subnet:
Application servers
Kubernetes worker nodes
Database
Redis
```

Why private subnet is used:

```text
App servers are not directly exposed to the internet
Database is not directly exposed to the internet
Only load balancers receive public traffic
Security is improved
```

## Security Design

Security groups:

```text
External Load Balancer:
Allows HTTPS from internet

Web Server:
Allows traffic only from external load balancer

Application Load Balancer:
Allows traffic only from frontend/web layer

App Server:
Allows traffic only from application load balancer

Database:
Allows traffic only from app servers
```

Secrets:

```text
Database username
Database password
JWT secret
Payment gateway key
Email service password
API tokens
```

Secrets should be stored in:

```text
AWS Secrets Manager
HashiCorp Vault
Kubernetes Secrets
Azure Key Vault
```

They should not be stored directly in source code.

## CI/CD Flow

CI/CD automates build, test, and deployment.

There are usually separate pipelines for:

```text
Frontend deployment
Backend deployment
Infrastructure deployment
Database migration
```

## Frontend CI/CD Pipeline

Example: React application deployment to S3 and CloudFront.

```text
1. Developer creates feature branch.
2. Developer updates frontend code.
3. Developer creates pull request.
4. CI pipeline runs lint checks.
5. CI pipeline runs frontend unit tests.
6. Code is reviewed and merged into main branch.
7. CD pipeline starts.
8. Pipeline installs dependencies.
9. Pipeline builds React production files.
10. Build files are uploaded to S3.
11. CloudFront cache is invalidated.
12. Users receive the new frontend version.
```

Example commands used in pipeline:

```text
npm install
npm test
npm run build
aws s3 sync build/ s3://shop-frontend-bucket
aws cloudfront create-invalidation --distribution-id ABC123 --paths "/*"
```

Frontend deployment result:

```text
Old UI files are replaced by new UI files
CloudFront starts serving the new version
Browser loads updated JavaScript and CSS
```

## Backend CI/CD Pipeline

Example: Backend deployment using Docker and Kubernetes.

```text
1. Developer creates feature branch.
2. Developer updates backend code.
3. Developer opens pull request.
4. CI pipeline runs unit tests.
5. CI pipeline runs integration tests.
6. CI pipeline runs security scan.
7. Code is reviewed and merged into main.
8. CD pipeline builds Docker image.
9. Docker image is tagged with commit ID.
10. Docker image is pushed to container registry.
11. Kubernetes deployment is updated with new image tag.
12. Kubernetes starts new pods.
13. Readiness probe checks if new pods are healthy.
14. Load balancer sends traffic to healthy pods.
15. Old pods are removed after new pods are ready.
```

Example Docker image:

```text
registry.example.com/shop-backend:b7ac801
```

Example backend deployment flow:

```text
GitHub --> CI Pipeline --> Docker Build --> Container Registry --> Kubernetes --> Load Balancer --> Users
```

## Database Migration Pipeline

Example: Adding a new column to the orders table.

Migration:

```sql
ALTER TABLE orders ADD COLUMN delivery_instructions TEXT;
```

Pipeline flow:

```text
1. Developer creates migration file.
2. Migration file is committed with backend code.
3. Pipeline validates migration syntax.
4. Migration runs on test database.
5. Automated tests run.
6. Migration runs on staging database.
7. Backup is verified for production database.
8. Migration runs on production database.
9. Backend deployment starts using new column.
```

Important rule:

```text
Database changes should be backward compatible whenever possible.
```

Example safe deployment:

```text
Step 1: Add new nullable column.
Step 2: Deploy backend that can write to the new column.
Step 3: Backfill old records if needed.
Step 4: Later make the column mandatory if required.
```

## How New Code Is Updated

### Frontend Code Update

Example change:

```text
Change homepage banner and add product filter
```

Flow:

```text
1. Developer changes React components.
2. Developer pushes code to GitHub.
3. Pipeline runs tests.
4. Pipeline builds static files.
5. New files are uploaded to S3.
6. CloudFront cache is invalidated.
7. New users get the latest files.
8. Existing users get new files after refresh or cache expiry.
```

Rollback:

```text
Upload previous frontend build
Invalidate CDN cache
Verify website is working
```

### Backend Code Update

Example change:

```text
Add coupon discount feature
```

Flow:

```text
1. Developer changes backend service code.
2. Developer adds tests.
3. Developer pushes code.
4. CI pipeline runs tests and security checks.
5. Docker image is created.
6. Docker image is pushed to registry.
7. Kubernetes deployment updates image version.
8. New pods start.
9. Health checks pass.
10. Load balancer routes traffic to new pods.
11. Old pods are terminated.
```

Rolling deployment:

```text
Before:
backend-v1, backend-v1, backend-v1

During:
backend-v2, backend-v1, backend-v1

Later:
backend-v2, backend-v2, backend-v1

Final:
backend-v2, backend-v2, backend-v2
```

Rollback:

```text
Kubernetes changes image from backend-v2 back to backend-v1
New v1 pods start
Health checks pass
Traffic returns to stable version
```

### Infrastructure Update

Example change:

```text
Increase app server autoscaling limit from 3 to 6
```

Flow:

```text
1. DevOps updates Terraform code.
2. Terraform plan shows the proposed change.
3. Team reviews the plan.
4. Terraform apply updates cloud infrastructure.
5. Monitoring confirms infrastructure is healthy.
```

Example Terraform commands:

```text
terraform init
terraform plan
terraform apply
```

## Monitoring And Logging

Monitoring helps DevOps know whether the application is healthy.

Metrics to monitor:

```text
CPU usage
Memory usage
Disk usage
API response time
HTTP 4xx errors
HTTP 5xx errors
Database connections
Database CPU
Database storage
Load balancer target health
Pod restart count
Payment failure rate
Order success rate
```

Logs to collect:

```text
Frontend errors
Backend application logs
Nginx access logs
Nginx error logs
Load balancer logs
Database slow query logs
Audit logs
Deployment logs
```

Tools:

```text
CloudWatch
Prometheus
Grafana
ELK Stack
OpenSearch
Datadog
New Relic
Splunk
```

Example alert:

```text
If API 5xx error rate is greater than 5% for 5 minutes, send alert to DevOps team.
```

## Backup And Disaster Recovery

Backup strategy:

```text
Daily database backup
Point-in-time recovery for RDS
S3 versioning enabled
Infrastructure code stored in GitHub
Docker images stored in registry
Regular restore testing
```

Disaster recovery example:

```text
1. Production database fails.
2. DevOps checks RDS health.
3. If Multi-AZ is enabled, AWS fails over to standby database.
4. Application reconnects to database.
5. Monitoring confirms traffic is normal.
6. DevOps writes incident report.
```

## Production Release Example

Feature:

```text
Add coupon discount during checkout
```

Complete release flow:

```text
1. Developer creates feature branch.
2. Developer updates backend discount logic.
3. Developer updates frontend checkout page.
4. Developer adds database migration for coupon table.
5. Developer writes tests.
6. Pull request is created.
7. CI runs frontend tests.
8. CI runs backend tests.
9. CI runs security scans.
10. Code review is completed.
11. Code is merged into main branch.
12. Database migration runs in staging.
13. Backend is deployed to staging.
14. Frontend is deployed to staging.
15. QA tests coupon feature in staging.
16. Production database backup is verified.
17. Database migration runs in production.
18. Backend production deployment starts.
19. New backend pods pass health checks.
20. Frontend production deployment starts.
21. CDN cache is invalidated.
22. Monitoring checks errors, latency, and order success rate.
23. Release is marked successful.
```

## DevOps Work In This Complete Project

DevOps responsibilities:

```text
Create cloud infrastructure
Manage DNS
Configure load balancers
Set up S3 and CloudFront
Set up Nginx if web servers are used
Create Kubernetes or ECS cluster
Create CI/CD pipelines
Create Dockerfiles
Manage container registry
Deploy frontend and backend
Manage secrets
Configure database connectivity
Set up backup strategy
Set up monitoring dashboards
Create alerts
Manage logs
Support rollback
Handle production incidents
Optimize cost and performance
Document deployment process
```

## Complete Summary

```text
User request flow:
User --> DNS --> CDN/LB --> Web Server/S3 --> App LB --> App Server --> Database

Response flow:
Database --> App Server --> App LB --> Frontend/Web Layer --> Browser --> User

Frontend deployment:
GitHub --> CI/CD --> Build Static Files --> S3/Nginx --> CDN Cache Invalidation --> Users

Backend deployment:
GitHub --> CI/CD --> Docker Image --> Registry --> Kubernetes/ECS --> Health Check --> Load Balancer Traffic

Database deployment:
Migration File --> Test DB --> Staging DB --> Backup Check --> Production DB

DevOps goal:
Automate deployment, improve reliability, secure the system, monitor production, and recover quickly from failures.
```
