# HotelReservationAPI
Small backend project for managing hotel rooms, reservations and payments. Built with Spring Boot while learning Spring Security, JWT and Redis, so some parts might still be a bit rough around the edges.

What it does
add rooms (standard / deluxe / suite, each has its own pricing logic)
book a room for a guest (checks availability first)
cancel a reservation
process a payment for a reservation
register / login with JWT, protected endpoints need a token
Stack
Java + Spring Boot
Spring Data JPA + PostgreSQL
Spring Security + JWT
Redis (caching)
Swagger / springdoc for API docs
Docker
Gradle
Project structure
com.jet.hotelreservation2
 ├── config       -> security config, jwt filter, swagger config
 ├── controller   -> auth, rooms, reservations, payments
 ├── entity       -> Room, Guest, Reservation, Payment, User
 ├── exception    -> global exception handler
 ├── redis        -> caching stuff
 ├── repository
 ├── service
 └── util         -> JwtUtil
Running it

Need PostgreSQL and Redis running (locally or with Docker, both work).

bash
./gradlew bootRun

runs on localhost:8090.

Or with Docker:

bash
docker compose up --build
Endpoints

Auth:

POST /api/auth/register - create a user
POST /api/auth/login - returns a JWT token

Rooms:

GET /api/rooms - all rooms
GET /api/rooms/available - only available ones
POST /api/rooms/standard /deluxe /suite - add a room

Reservations:

POST /api/reservations - book a room
GET /api/reservations - list all
DELETE /api/reservations/{id} - cancel

Payments:

POST /api/payments - pay for a reservation

Everything except register/login needs a header:

Authorization: Bearer <token>
Swagger

After running the app:

http://localhost:8090/swagger-ui/index.html
Config

application.yaml is not included here since it has local db/redis credentials in it. Create your own at src/main/resources/application.yaml, something like:

yaml
spring:
  application:
    name: HotelReservation2
  datasource:
    url: jdbc:postgresql://localhost:5432/postgres
    driver-class-name: org.postgresql.Driver
    username: your_db_username
    password: your_db_password
  data:
    redis:
      host: localhost
      port: 6379
  jpa:
    hibernate:
      ddl-auto: update

server:
  port: 8090

jwt:
  secret: your_own_secret_key_min_32_chars
  expiration: 86400000
Notes to self
rabbit folder exists but nothing's wired up yet, maybe later
