# Tutorials Project — Reference Guide

A full-stack CRUD application built with **Spring Boot** (backend) and **Angular 16** (frontend), demonstrating Create, Read, Update, and Delete operations on a `Tutorial` resource backed by a MySQL database.

---

## Tech Stack

| Layer      | Technology                        |
|------------|-----------------------------------|
| Backend    | Java 17, Spring Boot 3.2.1        |
| ORM        | Spring Data JPA (Hibernate)       |
| Database   | MySQL                             |
| Frontend   | Angular 16                        |
| Build Tool | Maven (via Maven Wrapper `mvnw`)  |

---

## Project Structure

```
Tutorials/
├── angular-16-crud/              # Angular 16 frontend application
└── Tutorials/                    # Spring Boot backend application
    ├── src/
    │   └── main/
    │       ├── java/com/example/Tutorials/
    │       │   ├── TutorialsApplication.java   # Spring Boot entry point
    │       │   ├── controller/
    │       │   │   └── TutorialController.java # REST API controller
    │       │   ├── model/
    │       │   │   └── Tutorial.java           # JPA entity
    │       │   └── repository/
    │       │       └── TutorialRepository.java # Spring Data JPA repository
    │       └── resources/
    │           └── application.properties      # DB and app config
    ├── pom.xml                   # Maven dependencies
    └── mvnw / mvnw.cmd           # Maven wrapper scripts
```

---

## Data Model

### `Tutorial` Entity

| Field         | Type      | Column Name   | Notes                        |
|---------------|-----------|---------------|------------------------------|
| `id`          | `long`    | id            | Auto-generated primary key   |
| `title`       | `String`  | title         | Title of the tutorial        |
| `description` | `String`  | description   | Description of the tutorial  |
| `published`   | `boolean` | published      | Publication status flag      |

---

## REST API Reference

Base URL: `http://localhost:8080/api`

> The backend allows cross-origin requests from `http://localhost:8081` (Angular dev server).

### Endpoints

| Method   | Endpoint                     | Description                                      | Request Body          | Response                  |
|----------|------------------------------|--------------------------------------------------|-----------------------|---------------------------|
| `GET`    | `/tutorials`                 | Get all tutorials (optionally filter by title)   | —                     | `200 OK` / `204 No Content` |
| `GET`    | `/tutorials?title={keyword}` | Search tutorials by title (partial match)        | —                     | `200 OK` / `204 No Content` |
| `GET`    | `/tutorials/{id}`            | Get a tutorial by ID                             | —                     | `200 OK` / `404 Not Found`  |
| `POST`   | `/tutorials`                 | Create a new tutorial                            | `Tutorial` JSON       | `201 Created`               |
| `PUT`    | `/tutorials/{id}`            | Update an existing tutorial by ID                | `Tutorial` JSON       | `200 OK` / `404 Not Found`  |
| `DELETE` | `/tutorials/{id}`            | Delete a tutorial by ID                          | —                     | `204 No Content`            |
| `DELETE` | `/tutorials`                 | Delete all tutorials                             | —                     | `204 No Content`            |
| `GET`    | `/tutorials/published`       | Get all published tutorials                      | —                     | `200 OK` / `204 No Content` |

### Sample Request / Response

**POST `/api/tutorials`**
```json
// Request Body
{
  "title": "Spring Boot Basics",
  "description": "Introduction to Spring Boot CRUD",
  "published": false
}

// Response — 201 Created
{
  "id": 1,
  "title": "Spring Boot Basics",
  "description": "Introduction to Spring Boot CRUD",
  "published": false
}
```

**PUT `/api/tutorials/1`**
```json
// Request Body
{
  "title": "Spring Boot Basics (Updated)",
  "description": "Updated description",
  "published": true
}

// Response — 200 OK
{
  "id": 1,
  "title": "Spring Boot Basics (Updated)",
  "description": "Updated description",
  "published": true
}
```

---

## Repository Interface

`TutorialRepository` extends `JpaRepository<Tutorial, Long>` and provides:

| Method                                  | Description                                      |
|-----------------------------------------|--------------------------------------------------|
| `findAll()`                             | Retrieve all tutorials (inherited from JPA)      |
| `findById(Long id)`                     | Find tutorial by primary key (inherited)         |
| `save(Tutorial t)`                      | Create or update a tutorial (inherited)          |
| `deleteById(Long id)`                   | Delete by primary key (inherited)                |
| `deleteAll()`                           | Delete all records (inherited)                   |
| `findByPublished(boolean published)`    | Custom — find by publication status             |
| `findByTitleContaining(String title)`   | Custom — partial title search (LIKE query)       |

---

## Getting Started

### Prerequisites

- Java 17+
- Maven (or use the included `mvnw` wrapper)
- MySQL running locally

### Database Setup

Create a MySQL database and update `src/main/resources/application.properties`:

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/your_db_name
spring.datasource.username=your_username
spring.datasource.password=your_password
spring.jpa.hibernate.ddl-auto=update
```

### Run the Backend

```bash
cd Tutorials
./mvnw spring-boot:run
# Windows: mvnw.cmd spring-boot:run
```

The server starts on **http://localhost:8080**.

### Run the Frontend

```bash
cd angular-16-crud
npm install
ng serve
```

The Angular app runs on **http://localhost:8081** by default.

---

## Maven Dependencies (pom.xml highlights)

| Dependency                        | Purpose                          |
|-----------------------------------|----------------------------------|
| `spring-boot-starter-web`         | REST API support                 |
| `spring-boot-starter-data-jpa`    | ORM / database access via JPA    |
| `mysql-connector-j`               | MySQL JDBC driver                |
| `spring-boot-starter-test`        | Unit & integration testing       |

---

## Key Design Decisions

- **`@CrossOrigin(origins = "http://localhost:8081")`** is set on the controller to allow the Angular frontend to communicate with the backend during local development.
- **`published`** defaults to `false` on creation — tutorials must be explicitly published via a `PUT` request.
- The `findByTitleContaining` method leverages Spring Data JPA's query derivation to perform a SQL `LIKE` search without any custom query.

---

*Generated reference document — last updated May 2026.*
