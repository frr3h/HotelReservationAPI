# HotelReservationAPI

Small backend project for managing hotel rooms, reservations and payments.
Built with Spring Boot while learning Spring Security, JWT and Redis, so some parts might still be a bit rough around the edges.

---

## What it does

- Add rooms (standard / deluxe / suite), each with its own pricing logic
- Book a room for a guest, checking availability first
- Cancel a reservation
- Process a payment for a reservation
- Register / login with JWT — protected endpoints require a token

---

## Stack

| Layer          | Tech                          |
|----------------|--------------------------------|
| Language       | Java                            |
| Framework      | Spring Boot                     |
| Persistence    | Spring Data JPA + PostgreSQL    |
| Security       | Spring Security + JWT           |
| Caching        | Redis                           |
| API docs       | Swagger / springdoc             |
| Build tool     | Gradle                          |
| Container      | Docker                          |

---

## Project structure

```
com.jet.hotelreservation2
 ├── config       → security config, jwt filter, swagger config
 ├── controller   → auth, rooms, reservations, payments
 ├── entity       → Room, Guest, Reservation, Payment, User
 ├── exception    → global exception handler
 ├── redis        → caching stuff
 ├── repository
 ├── service
 └── util         → JwtUtil
```

---

## Running it

You'll need PostgreSQL and Redis running — locally or via Docker, either works.

**Local:**
```bash
./gradlew bootRun
```
App runs on `localhost:8090`.

**Docker:**
```bash
docker compose up --build
```

---

## Endpoints

**Auth**
| Method | Endpoint             | Description              |
|--------|-----------------------|---------------------------|
| POST   | `/api/auth/register`  | Create a user             |
| POST   | `/api/auth/login`     | Returns a JWT token       |

**Rooms**
| Method | Endpoint                                  | Description         |
|--------|---------------------------------------------|-----------------------|
| GET    | `/api/rooms`                                | All rooms              |
| GET    | `/api/rooms/available`                      | Only available rooms   |
| POST   | `/api/rooms/standard` `/deluxe` `/suite`    | Add a room             |

**Reservations**
| Method | Endpoint                     | Description        |
|--------|-------------------------------|-----------------------|
| POST   | `/api/reservations`           | Book a room           |
| GET    | `/api/reservations`           | List all              |
| DELETE | `/api/reservations/{id}`      | Cancel a reservation  |

**Payments**
| Method | Endpoint          | Description                    |
|--------|--------------------|----------------------------------|
| POST   | `/api/payments`   | Pay for a reservation           |

Everything except register/login needs this header:
```
Authorization: Bearer <token>
```

---

## Swagger

Once the app is running:
```
http://localhost:8090/swagger-ui/index.html
```

---

## Config

`application.yaml` isn't included here since it has local DB/Redis credentials in it.
Create your own at `src/main/resources/application.yaml`, something like:

```yaml
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
  expiration: 8640000

## Notes to self

- `rabbit` folder exists but nothing's wired up yet, maybe later
