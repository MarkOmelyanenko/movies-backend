# Movies Backend

A Spring Boot REST API for browsing movies and recording user-written reviews. The service stores movies and review documents in MongoDB and exposes endpoints for listing movies, fetching details by IMDb identifier, and creating reviews associated with a movie.

## Frontend Repository

The Frontend for this project is available at: [Movies Frontend](https://github.com/MarkOmelyanenko/movies-frontend)

## Features

- Exposes RESTful endpoints under `/api/v1` for interacting with movie and review data
- Integrates with MongoDB using Spring Data repositories and document models
- Persists review references back onto the corresponding movie document after creation
- Enables cross-origin requests so the API can be consumed by browser-based front ends
- Bundles Spring Boot Actuator for health and metrics endpoints

## Project structure

```
src/main/java/dev/march/movies/
├── Controllers       # REST controllers for movies and reviews
├── Models            # MongoDB document models for Movie and Review
├── Repositories      # Spring Data interfaces for persistence
├── Services          # Business logic, including MongoTemplate updates
└── MoviesApplication # Application entry point
```

Configuration is handled through `src/main/resources/application.properties`, which resolves MongoDB connection details from environment variables.

## Prerequisites

- Java 21 or later
- Maven 3.9+
- Access to a MongoDB deployment (e.g., Atlas cluster) and credentials with read/write permissions

## Configuration

Create a `.env` file in the project root or export the following environment variables before running the application:

| Variable | Description |
| --- | --- |
| `MONGO_DATABASE` | Name of the MongoDB database that stores movie and review collections |
| `MONGO_USER` | Username for authenticating with MongoDB |
| `MONGO_PASSWORD` | Password for the MongoDB user |
| `MONGO_CLUSTER` | Host string for your MongoDB cluster (e.g., `cluster0.abcd123.mongodb.net/`) |

The connection URI is assembled as `mongodb+srv://${MONGO_USER}:${MONGO_PASSWORD}@${MONGO_CLUSTER}` by Spring configuration.

## Running the application

From the project root, use the Maven wrapper to start the Spring Boot application:

```bash
./mvnw spring-boot:run
```

The API listens on `http://localhost:8080` by default. You can also produce an executable JAR:

```bash
./mvnw clean package
java -jar target/movies-0.0.1-SNAPSHOT.jar
```

## API reference

### List movies

- **Endpoint:** `GET /api/v1/movies`
- **Response:** Array of movie documents containing identifiers, titles, metadata, and review references.

### Fetch a movie by IMDb ID

- **Endpoint:** `GET /api/v1/movies/{imdbId}`
- **Response:** Single movie document (if found).

### Create a review

- **Endpoint:** `POST /api/v1/reviews`
- **Request body:**

  ```json
  {
    "imdbId": "tt1234567",
    "reviewBody": "Loved the cinematography!"
  }
  ```

- **Behavior:** Persists a new review document and appends its reference to the matched movie document.
