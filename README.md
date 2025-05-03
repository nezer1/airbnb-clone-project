# **AirBnB Clone**

Building micro-service based  Airbnb clone using this [technology stack](#technology-stack)

## :dizzy: **Objective**

The backend for the Airbnb Clone project is designed to mimic the following core features:
 - user interactions [registration, authentication, user profile management]
 - property listings
 - bookings
 - payments 
 - property review and rating
 - database opotimization

## :busts_in_silhouette: **Team Roles**

- Backend Developer: Responsible for implementing API endpoints, database schemas, and business logic.
- Database Administrator: Manages database design, indexing, and optimizations.
- DevOps Engineer: Handles deployment, monitoring, and scaling of the backend services by automating CI/CD pipelines.
- QA Engineer: Ensures the backend functionalities are thoroughly tested and meet quality standards


## **Technology Stack**
- Django: A high-level Python web framework used for building the RESTful API.
- Django REST Framework: Provides tools for creating and managing RESTful APIs.
- PostgreSQL: A powerful relational database used for data storage.
- GraphQL: Allows for flexible and efficient querying of data. Helps to query data from API gateway
- Celery: For handling asynchronous tasks such as sending notifications or processing payments.
- Redis: Used for caching and session management. 
- Docker: Containerization tool for consistent development and deployment environments.
- CI/CD Pipelines: Automated pipelines for testing and deploying code changes.


## **Database Design**
### **User**
| Field | Type | Description |
| :---         |     :---:      |    ---: |
| userid   | UUID    | Primary Key   |
| email     | string       | unique email    |
| user_type      | string       | host , guest or admin|
| date_joined | DateTime   | account creation date|
| status  | Boolean  | active or inactive user account|
| profile_image | String | profile picture link|
| name | string | full name |
| password | string | hashed password |


### **Property**
| Field | Type | Description |
| :---         |     :---:      |    ---: |
| property_id   | UUID    | Primary Key   |
| user_id     | UUID  | FK -> User (host)    |
| location  | string     | property address|
| price_per_night      | string       | night rate |
| image | string   | property image link |
| title | string   | property name  |
| status | string   | active or inactive |
| description | text   | detailed property description|

**Note: Availability of property computed based on existing bookings**


### **Amenities**
| Field | Type | Description |
| :---         |     :---:      |    ---: |
| amenity_id   | UUID    | Primary Key   |
| name    | string  |  name of amenity  |


### **Property-Amenities**
*Junction table 
| Field | Type | Description |
| :---         |     :---:      |    ---: |
| prop_amen_id   | UUID    | Primary Key   |
| property_id     | UUID  | FK -> property    |
| amenity_id  | UUID     | FK -> amenity|


### **Bookings**
| Field | Type | Description |
| :---         |     :---:      |    ---: |
| booking_id   | UUID    | Primary Key   |
| property_id     | UUID  | FK -> property    |
| user_id  | UUID     |     FK -> user(guest)|
| check_in      | DateTime       | check-in date & time |
|check_out | DateTime | check_out date & time |
|status | string | confirmed or cancelled |

### **Payments**
| Field | Type | Description |
| :---         |     :---:      |    ---: |
| payment_id   | UUID    | Primary Key   |
| booking_id     | UUID  | FK -> booking   |
| user_id     | UUID  | FK -> user(guest)   |
| amount  | decimal     | payment amount|
| status      | string       | pending or paid or failed |
|timestamp | DateTime | time payment was made|

### **Reviews**
| Field | Type | Description |
| :---         |     :---:      |    ---: |
| review_id   | UUID    | Primary Key   |
| user_id     | UUID  | FK -> User(guest)    |
| property_id  | UUID     |    FK -> property|
| rating      | integer      | rating |
|comment | text | optional written feedback|



### Entity Relationships

| From Entity  | To Entity         | Relationship Type | Description  |
|--------------|------------------|-------------------|---------------|
| Users        | Properties        | One-to-Many       | One user (host) can own many properties. |
| Users        | Bookings          | One-to-Many       | One user (guest) can make many bookings.  |
| Users        | Reviews           | One-to-Many       | One user (guest) can write many reviews.          |
| Users        | Payments          | One-to-Many       | One user (guest) can make multiple payments.             |
| Properties   | Bookings          | One-to-Many       | One property can have many bookings.   |
| Properties   | Reviews           | One-to-Many       | One property can receive many reviews.    |
| Properties   | Amenities         | Many-to-Many      | Properties can have multiple amenities (via a junction table). |
| Amenities    | Properties        | Many-to-Many      | An amenity can be available in multiple properties (via a junction table).|
| Bookings     | Payments          | One-to-One        | One booking has one payment record.                  |


## **Feature Breakdown**

### 1. API Documentation
- OpenAPI Standard: The backend APIs are documented using the OpenAPI standard to ensure clarity and ease of integration.
- Django REST Framework: Provides a comprehensive RESTful API for handling CRUD operations on user and property data.
- GraphQL: Offers a flexible and efficient query mechanism for interacting with the backend.
### 2. User Authentication
- Endpoints: /users/, /users/{user_id}/
- Features: Register new users, authenticate, and manage user profiles.
### 3. Property Management
- Endpoints: /properties/, /properties/{property_id}/
- Features: Create, update, retrieve, and delete property listings.
### 4. Booking System
- Endpoints: /bookings/, /bookings/{booking_id}/
- Features: Make, update, and manage bookings, including check-in and check-out details.
### 5. Payment Processing
- Endpoints: /payments/
- Features: Handle payment transactions related to bookings.
### 6. Review System
- Endpoints: /reviews/, /reviews/{review_id}/
- Features: Post and manage reviews for properties.
### 7. Database Optimizations
- Indexing: Implement indexes for fast retrieval of frequently accessed data.
- Caching: Use caching strategies to reduce database load and improve performance.


## 🔒 API Security

### 1. 🔐 Authentication
- **Implementation**: Token-based authentication using JWT (JSON Web Tokens).
- **Purpose**: Ensures that only registered users can access protected routes (e.g., booking, payments).
- **Importance**: Prevents unauthorized access to personal data and user accounts.

### 2. 🛂 Authorization
- **Implementation**: Role-based access control (RBAC) to distinguish between guests, hosts, and admins.
- **Purpose**: Restricts actions (e.g., only property owners can edit their listings).
- **Importance**: Prevents privilege escalation and unauthorized actions on the platform.

### 3. 🚨 Rate Limiting
- **Implementation**: API throttling via Django REST Framework or a gateway proxy (e.g., NGINX).
- **Purpose**: Limits the number of requests a user/IP can make in a time period.
- **Importance**: Protects against brute-force attacks, DDoS, and API abuse.

### 4. 🧬 Data Validation & Sanitization
- **Implementation**: Strict schema validation with DRF serializers and form sanitization.
- **Purpose**: Ensures incoming data is clean and expected.
- **Importance**: Prevents injection attacks (SQL injection, XSS, etc.).

### 5. 🔐 Secure Payments
- **Implementation**: Use of third-party payment gateways (e.g., Stripe) with encrypted transactions.
- **Purpose**: Offloads sensitive payment handling to trusted services.
- **Importance**: Avoids storing credit card data directly and reduces liability.

### 6. 🔒 HTTPS Enforcement
- **Implementation**: All communications will be encrypted with SSL/TLS.
- **Purpose**: Prevents data interception (man-in-the-middle attacks).
- **Importance**: Secures login credentials, personal info, and payment details during transmission.

### 7. 🗝️ Password Hashing
- **Implementation**: Passwords stored using secure hashing algorithms (e.g., PBKDF2, Argon2).
- **Purpose**: Ensures even if the database is compromised, raw passwords remain protected.
- **Importance**: Protects users from credential reuse and identity theft.

### 8. 🧯 Logging & Monitoring
- **Implementation**: Log authentication attempts, errors, and suspicious behavior.
- **Purpose**: Detects and alerts on potential attacks or anomalies.
- **Importance**: Enables rapid incident response and system hardening.

---

### ✅ Why Security Matters

| Area           | Why Security Is Crucial                                                                 |
|----------------|------------------------------------------------------------------------------------------|
| User Data      | Protects personal and contact information from leaks or theft.                          |
| Authentication | Prevents unauthorized access to accounts and sensitive actions.                         |
| Payments       | Ensures financial transactions are secure and PCI compliant.                            |
| Booking System | Prevents booking fraud or manipulation of availability and pricing.                    |
| Admin Controls | Protects platform operations from unauthorized modifications or abuse.  |


##  CI/CD Pipeline

### What is CI/CD?
- **Continuous Integration (CI)**: Automatically runs tests and builds the project when code changes are made.
- **Continuous Deployment (CD)**: Automatically deploys the latest changes to a staging or production environment once they pass all tests.

### Why It’s Important
-  **Improves Code Quality**: Automated testing helps catch bugs early in the development cycle.
-  **Saves Time**: Reduces the need for manual testing and deployment steps.
-  **Faster Iteration**: Encourages small, frequent updates, making the product evolve faster.
-  **Reliable Deployments**: Reduces human errors and ensures consistent deployments across environments.

### Tools Used
- **GitHub Actions**: Automates testing and deployment workflows directly from the GitHub repository.
- **Docker**: Ensures that the application runs in consistent environments across development, testing, and production.
- **Docker Compose**: Manages multi-container environments (e.g., Django + PostgreSQL + Redis).
- **Heroku / Render / AWS / DigitalOcean (Optional)**: Platforms for deploying the application.
- **pytest / unittest**: For running automated tests.

### Workflow Example
1. Developer pushes code to GitHub.
2. GitHub Actions triggers:
   - Linting and formatting checks.
   - Unit and integration tests.
   - Docker image build.
3. On success:
   - Deploy to staging or production using Docker.