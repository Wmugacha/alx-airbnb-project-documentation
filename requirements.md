## 🧩 Backend Feature Requirements

### 1. 🔐 User Authentication

#### Functional Requirements

* Users can register and log in using email and password.
* Passwords are securely hashed and stored.
* Authentication tokens (JWT) are issued upon login.
* Supports login sessions with expiration.

#### API Endpoints

```http
POST /api/register
POST /api/login
POST /api/logout
GET  /api/profile
```

#### Input/Output Specs

* **Register**

  * **Input:** `{ first_name, last_name, email, password }`
  * **Output:** `201 Created`, `{ message, user_id, token }`
* **Login**

  * **Input:** `{ email, password }`
  * **Output:** `200 OK`, `{ token, user_id }`
* **Profile**

  * **Input:** `Bearer token`
  * **Output:** `200 OK`, `{ user data }`

#### Validation Rules

* Email must be unique and valid format.
* Password must be at least 8 characters.
* All fields are required during registration.

#### Performance Criteria

* Registration and login should complete within 300ms under load.
* Rate limit login attempts to prevent brute-force attacks.

---

### 2. 🏠 Property Management

#### Functional Requirements

* Hosts can create, update, and delete property listings.
* Properties have attributes like location, description, price per night.
* Users can view property listings and search by filters.

#### API Endpoints

```http
GET    /api/properties
GET    /api/properties/:id
POST   /api/properties
PUT    /api/properties/:id
DELETE /api/properties/:id
```

#### Input/Output Specs

* **Create Property**

  * **Input:** `{ title, description, location, price_per_night }`
  * **Output:** `201 Created`, `{ property_id }`
* **Get Properties**

  * **Input:** Optional query params (location, price range)
  * **Output:** `200 OK`, `[ { property1 }, { property2 }, ... ]`

#### Validation Rules

* `price_per_night` must be a positive number.
* `description` max 500 characters.
* Only the host who created the property can modify it.

#### Performance Criteria

* Properties list must return within 500ms for standard filters.
* Pagination supported: 10 items per page.

---

### 3. 📅 Booking System

#### Functional Requirements

* Users can book a property for a given date range.
* Bookings have statuses: `pending`, `confirmed`, `cancelled`.
* Total cost is calculated based on nightly rate and duration.

#### API Endpoints

```http
POST   /api/bookings
GET    /api/bookings/:id
GET    /api/bookings/user/:userId
PUT    /api/bookings/:id/status
```

#### Input/Output Specs

* **Create Booking**

  * **Input:** `{ property_id, start_date, end_date }`
  * **Output:** `201 Created`, `{ booking_id, total_price, status }`
* **Update Status**

  * **Input:** `{ status }`
  * **Output:** `200 OK`, `{ message }`

#### Validation Rules

* No overlapping bookings allowed.
* `start_date` must be before `end_date`.
* `total_price = price_per_night * number_of_nights`.

#### Performance Criteria

* Booking creation must complete in < 400ms.
* Email notification sent on status change (async).

---