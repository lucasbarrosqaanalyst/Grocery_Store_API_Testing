# 🛒 Grocery Store API Collection

A comprehensive documentation for the **Grocery Store API** Postman collection. This documentation maps the operational lifecycle of an e-commerce workflow, capturing the precise endpoints, request payloads, and schema validation logic explicitly implemented within the testing suite.

---

## 🏗️ Architecture & Core Configurations

The collection communicates with a baseline service layer using the environment variable configuration below:
* **Base URL:** `https://simple-grocery-store-api.glitch.me`
* **Format:** All mutated payloads require standard `application/json` structures.

### Global Response Status Matrix
* `200 OK`: Request succeeded and returned requested data schemas.
* `201 Created`: Resources successfully created on the server.
* `204 No Content`: Action successfully processed without a response body.
* `400 Bad Request`: Input validation boundaries or query syntax parameters violated.
* `401 Unauthorized`: Authentication failed due to missing or invalid credentials.

---

## 📡 API Endpoint Catalog & Validation Specifications

### 1. ⚙️ System Status
#### `GET /status`
Checks the availability layer of the core API service.

* **Validations:**
  * Asserts the service responds with a `200 OK` status code.

---

### 2. 🥦 Product Catalog Management
#### `GET /products`
Retrieves a list of available items filtered by optional parameter contexts.

* **Query Parameters:**
  * `available` (Boolean, Optional): Filters items explicitly by inventory visibility.
  * `results` (Integer, Optional): Controls result thresholds.
* **Validations & Error Constraints:**
  * Validates response structures return as an array containing at least one item.
  * Asserts item payloads resolve to an object containing a numerical `id` and a boolean `inStock` flag evaluating to `true`.
  * **Input Validation (Boundary Rules):** Triggers a `400 Bad Request` with an explicit error phrase containing `"Invalid value for query parameter"` if parameter limits are broken (e.g., configuring `results` to an invalid maximum like `21` or a negative minimum like `-3`).

#### `GET /products/:productId`
Retrieves granular resource records for a discrete item ID saved into collection variables.

* **Validations:**
  * Confirms response matches a `200 OK` object structure.
  * Verifies the target item name parameter resolves to a non-null state.
  * Asserts item pricing evaluates to a numerical value strictly above zero (`price > 0`).
  * Confirms the resource's `inStock` key explicitly evaluates to `true`.

---

### 3. 🛒 Shopping Cart Management
#### `POST /carts`
Initializes an unauthenticated shopping cart resource instance.

* **Validations:**
  * Captures a `201 Created` status code verification.
  * Asserts the returned JSON contains a valid, non-null `cartId` represented strictly as a string value.

#### `GET /carts/:cartId`
Inspects general structural data attached to an active cart tracking reference.

* **Validations:**
  * Verifies the reference returns a valid `200 OK` response status.

#### `GET /carts/:cartId/items`
Retrieves an explicit listing of all items nested inside the current active cart session.

#### `POST /carts/:cartId/items`
Appends a defined product reference directly to an active cart.
