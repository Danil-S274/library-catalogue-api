# Library Catalogue REST API

## 1. Topic
Catalogue — Library catalogue system for managing books, authors, and categories.

---

## 2. Functional Requirements
- The API must allow clients to retrieve a list of books.
- The API must allow clients to retrieve a single book by its unique identifier.
- The API must support filtering books by author, category, and publication year.
- The API must support pagination for all collection endpoints.
- The API must allow authenticated users to create new books.
- The API must allow authenticated users to update existing books.
- The API must allow authenticated users to delete books.
- The API must expose related resources using HATEOAS links.

---

## 3. Non-Functional Requirements
- The API must follow REST architectural principles.
- All responses must be provided in JSON format.
- HTTP status codes must be meaningful and correctly used.
- The API must support caching for read-only operations.
- The API must be secured using JWT-based authentication.
- The API must support versioning.
- The API documentation must be clear and unambiguous.

---

## 4. Entity Model Description

### Book
| Field | Type | Description |
|------|------|------------|
| id | UUID | Unique identifier of the book |
| title | String | Book title |
| authorId | UUID | Identifier of the author |
| categoryId | UUID | Identifier of the category |
| publicationYear | Integer | Year of publication |
| isbn | String | International Standard Book Number |

---

### Author
| Field | Type | Description |
|------|------|------------|
| id | UUID | Unique identifier of the author |
| name | String | Full name of the author |

---

### Category
| Field | Type | Description |
|------|------|------------|
| id | UUID | Unique identifier of the category |
| name | String | Category name |

---

## 5. REST API Design

### Base URL

All endpoints described below are relative to this base URL.

---

## 6. Operations Description

### GET /books
Returns a paginated list of books.

**Query Parameters:**
- `page` (integer, default: 0)
- `size` (integer, default: 10)
- `authorId` (UUID, optional)
- `categoryId` (UUID, optional)
- `year` (integer, optional)

**Response Status Codes:**
- `200 OK` – Successful request
- `400 Bad Request` – Invalid query parameters

**Caching:**
- Response is cached for 60 seconds using `Cache-Control: max-age=60`

---

### GET /books/{id}
Returns a single book by its identifier.

**Response Status Codes:**
- `200 OK` – Book found
- `404 Not Found` – Book does not exist

**Caching:**
- Response is cached for 60 seconds

---

### POST /books
Creates a new book (authentication required).

**Response Status Codes:**
- `201 Created` – Book successfully created
- `400 Bad Request` – Invalid request body
- `401 Unauthorized` – Authentication required

---

### PUT /books/{id}
Updates an existing book (authentication required).

**Response Status Codes:**
- `200 OK` – Book successfully updated
- `404 Not Found` – Book does not exist
- `401 Unauthorized` – Authentication required

---

### DELETE /books/{id}
Deletes a book (authentication required).

**Response Status Codes:**
- `204 No Content` – Book successfully deleted
- `404 Not Found` – Book does not exist
- `401 Unauthorized` – Authentication required

---

## 7. Pagination
All collection endpoints support pagination using the following parameters:

?page=0&size=10

Pagination metadata is included in responses:
```json
{
  "page": 0,
  "size": 10,
  "totalElements": 120,
  "totalPages": 12
}
```

## 8. Richardson Maturity Model Application

Level 1: Resources are identified by URIs (e.g., /books, /authors).

Level 2: HTTP methods are used correctly (GET, POST, PUT, DELETE).

Level 3: HATEOAS is implemented.

Example HATEOAS Response

```json
{
  "id": "123",
  "title": "Clean Code",
  "_links": {
    "self": { "href": "/api/v1/books/123" },
    "author": { "href": "/api/v1/authors/45" }
  }
}
```

## 9. Authentication

The API uses JWT (JSON Web Token) authentication.

Authorization Header:

Authorization: Bearer <token>

Authentication Errors:

401 Unauthorized – Missing or invalid token

403 Forbidden – Insufficient permissions

## 10. Error Handling

All errors follow a unified response format:

## 11. Caching Strategy

GET /books → cached

GET /books/{id} → cached

POST /books → not cached

PUT /books/{id} → not cached

DELETE /books/{id} → not cached

Cache invalidation occurs after update or delete operations.
