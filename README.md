# **AirBnB Clone**

Building backend services of an Airbnb clone using this [technology stack](#technology-stack)

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
- Redis: Used for caching and session management. It will be the celery message broker
- Docker: Containerization tool for consistent development and deployment environments.
- CI/CD Pipelines: Automated pipelines for testing and deploying code changes.


## **Database Design**
# **User Entity**
| Field | Type | Description |
| :---         |     :---:      |    ---: |
| id   | integer    | Primary Key   |
| email     | string       | unique email    |
| username  | string       | unique username |
| role      | string       | host , guest or admin|
| date_joined | DateTime   | account creation date|
| status  | Boolean  | user account is active |
| profile_image | URL/ String | profile picture |
| first_name | string | user's first name |
| last_name | string | user's second name |