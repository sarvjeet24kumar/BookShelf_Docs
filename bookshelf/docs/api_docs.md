# BookShelf REST API Documentation

## Base Configuration

| Setting | Value |
|---------|-------|
| Base URL | `/api/v1/` |
| Versioning | URI-based |
| Authentication | JWT (Bearer Token) |
| Pagination | Enabled for list endpoints |
| Update Strategy | PATCH only |

---

## Pagination

Pagination is applied to all list endpoints.

**Response Structure:**
```json
{
    "count": 50,
    "page": 1,
    "page_size": 10,
    "next": "http://localhost:8000/api/v1/books/?page=2",
    "previous": null,
    "data": [...]
}
```

---

## Response Formats

### Success Response

```json
{
    "id": "019b9b30-fa5f-71ff-8a30-3ec1cf2109dd",
    "title": "Deep Learning",
    "author": "Ian Goodfellow",
    "isbn": "9780262035613",
    "published_year": 2016,
    "request_status": "APPROVED",
    "created_by_email": "admin@gmail.com",
    "genres": ["Technology", "Machine Learning"],
    "created_at": "2026-01-08T06:50:36.959536+05:30"
}
```

### Error Response

```json
{
    "errors": {
        "isbn": ["ISBN already exists."],
        "title": ["Title must contain at least one letter."]
    }
}
```

**or**

```json
{
    "error": "Book not found."
}
```

---

## Authentication

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/auth/signup/` | Register new user |
| POST | `/auth/login/` | Login and get JWT tokens |
| POST | `/auth/refresh/` | Refresh access token |
| POST | `/auth/logout/` | Logout (blacklist token) |

### Signup Request
```json
{
    "username": "testuser",
    "email": "test@example.com",
    "password": "securepassword123",
    "first_name": "Test",
    "last_name": "User"
}
```

### Login Request
```json
{
    "email": "test@example.com",
    "password": "securepassword123"
}
```

### Login Response
```json
{
    "access": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9...",
    "refresh": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..."
}
```

---

## User Management (Admin Only)

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/users/` | List all users |
| GET | `/users/{id}/` | Get user details |
| DELETE | `/users/{id}/` | Soft delete user |

## Me 

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/users/me/` | Get self details |
| DELETE | `/users/me/` | Soft delete user (User only)|

### User Response
```json
{
    "id": "019b99e5-09aa-7bd5-9a3c-71efd21c56ee",
    "username": "testuser1",
    "email": "testuser1@example.com",
    "first_name": "Test",
    "last_name": "User",
    "role": "USER",
    "date_joined": "2026-01-08T00:48:02.922906+05:30"
}
```

---

## Books

| Method | Endpoint | Description | Access |
|--------|----------|-------------|--------|
| GET | `/books/` | List books | All authenticated |
| POST | `/books/` | Create book | All authenticated |
| GET | `/books/{id}/` | Get book details | All authenticated |
| PATCH | `/books/{id}/` | Update book | Admin only |
| DELETE | `/books/{id}/` | Soft delete book | Admin only |

### Query Parameters (GET `/books/`)

| Parameter | Description | Example |
|-----------|-------------|---------|
| `request_status` | Filter by status (PENDING, APPROVED, REJECTED) | `?request_status=APPROVED` |
| `genre` | Filter by genre name | `?genre=Fiction` |
| `title` | Search by title (case-insensitive) | `?title=harry` |

### Visibility Rules

| User Type | Default View | With `request_status` Filter |
|-----------|--------------|------------------------------|
| Admin | All books | Filter by specified status |
| User | APPROVED + own books | APPROVED: all, PENDING/REJECTED: own only |

### Create Book Request
```json
{
    "title": "My Book Title",
    "author": "Author Name",
    "isbn": "9780262035613",
    "published_year": 2020,
    "genres": ["019b9297-6647-72dd-902b-d0fe1e8e2b48"]
}
```

### Book Response
```json
{
    "id": "019b9b30-fa5f-71ff-8a30-3ec1cf2109dd",
    "title": "My Book Title",
    "author": "Author Name",
    "isbn": "9780262035613",
    "published_year": 2020,
    "request_status": "APPROVED",
    "created_by_email": "admin@gmail.com",
    "genres": ["Fiction", "Technology"],
    "created_at": "2026-01-08T06:50:36.959536+05:30"
}
```

### Update Book Request (Admin Only)
```json
{
    "title": "Updated Title",
    "author": "Updated Author",
    "published_year": 2021,
    "request_status": "APPROVED"
}
```

> **Note:** ISBN and genres are NOT updateable after creation.

---

## My Books (User Library)

| Method | Endpoint | Description | Access |
|--------|----------|-------------|--------|
| GET | `/my-books/` | List user's reading list | User only |
| POST | `/my-books/` | Add book to reading list | User only |
| GET | `/my-books/{id}/` | Get book from reading list | User only |
| PATCH | `/my-books/{id}/` | Update reading status | User only |
| DELETE | `/my-books/{id}/` | Remove from reading list | User only |

### Query Parameters (GET `/my-books/`)

| Parameter | Description | Example |
|-----------|-------------|---------|
| `status` | Filter by reading status | `?status=READING` |
| `title` | Search by title | `?title=harry` |
| `author` | Search by author | `?author=rowling` |

### Add Book to Library Request
```json
{
    "id": "019b9b30-fa5f-71ff-8a30-3ec1cf2109dd",
    "status": "TO_READ"
}
```

### Reading Status Values
- `TO_READ`
- `READING`
- `COMPLETED`

### My Book Response
```json
{
    "id": "019b9b30-fa5f-71ff-8a30-3ec1cf2109dd",
    "title": "Deep Learning",
    "author": "Ian Goodfellow",
    "isbn": "9780262035613",
    "published_year": 2016,
    "status": "READING",
    "created_by_email": "admin@gmail.com",
    "genres": ["Technology", "Machine Learning"],
    "created_at": "2026-01-08T06:50:36.959536+05:30"
}
```

> **Note:** `request_status` is hidden in my-books responses. Only APPROVED books can be added.

---

## Genres

| Method | Endpoint | Description | Access |
|--------|----------|-------------|--------|
| GET | `/genres/` | List all genres | All authenticated |

### Genre Response
```json
{
    "id": "019b9297-6647-72dd-902b-d0fe1e8e2b48",
    "name": "Fiction",
    "created_at": "2026-01-07T14:30:00.000000+05:30"
}
```

---

## Request Status Values

| Status | Description |
|--------|-------------|
| `PENDING` | Book awaiting admin approval (default for users) |
| `APPROVED` | Book approved and visible to all (default for admins) |
| `REJECTED` | Book rejected by admin |

---

## Error Codes

| Status Code | Description |
|-------------|-------------|
| 200 | Success |
| 201 | Created |
| 204 | No Content (successful delete) |
| 400 | Bad Request (validation error) |
| 401 | Unauthorized (invalid/missing token) |
| 403 | Forbidden (insufficient permissions) |
| 404 | Not Found |
| 500 | Internal Server Error |

---

## Validation Rules

### Title & Author
- Must contain at least one letter
- Allowed: letters, numbers, spaces, underscores, apostrophes, periods, commas

### ISBN
- Maximum 13 characters
- Must be unique across all books

### Published Year
- Range: 1000 - 2100

### Genres
- Must provide at least one valid genre UUID
- Genre must exist in database
