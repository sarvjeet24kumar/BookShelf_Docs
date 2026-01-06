# API Documentation

## Overview

This document provides comprehensive API documentation for the **BookShelf – Library Management System**.
The API allows **users** to manage their personal book collection and **admins** to manage users and platform administration.

---

## Table of Contents

1. Resource List
2. Endpoint Reference
3. Authentication
4. Endpoint Details
5. Error Codes

---

## Resource List

| Resource  | Description                          |
| --------- | ------------------------------------ |
| **Auth**  | User registration, login, and logout |
| **Books** | Public and personal book management  |
| **Admin** | User and Books  management                      |

---

## Endpoint Reference

| Method | Endpoint                    | Description                         | Auth Required |
| ------ | --------------------------- | ----------------------------------- | ------------- |
| POST   | `/api/auth/signup/`         | Register new user                   | No            |
| POST   | `/api/auth/login/`          | User login                          | No            |
| POST   | `/api/auth/logout/`         | User logout                         | User          |
| GET    | `/api/books/`               | List all books                      | User and Admin         |
| POST   | `/api/books/`               | Create new book                     | Admin         |
| POST   | `/api/books/`               | Request new book                     | User         |
| GET    | `/api/books/{id}/`          | Get book details                    | User  and Admin        |
| PATCH  | `/api/books/{id}/`          | Partial update book                 | Admin         |
| DELETE | `/api/books/{id}/`          | Delete book                         | Admin         |
| GET    | `/api/my-books/`            | List user's personal books          | User          |
| POST   | `/api/my-books/`            | Add book to personal list           | User          |
| GET    | `/api/my-books/{id}/`       | Get personal book details           | User          |
| PATCH  | `/api/my-books/{id}/`       | Update personal book status         | User          |
| DELETE | `/api/my-books/{id}/`       | Remove book from personal list      | User          |
| GET    | `/api/users/`               | List all users                      | Admin         |
| GET    | `/api/users/{id}/`          | Get user details                    | Admin         |
| DELETE | `/api/users/{id}/`          | Delete user                         | Admin         |

---

## Authentication

### Mechanism

JWT (JSON Web Token) Bearer Authentication.

### Header Format

```
Authorization: Bearer {access_token}
```

### Roles

| Role  | Description                        |
| ----- | ---------------------------------- |
| USER  | Manage own collection and browse   |
| ADMIN | Full platform management and users |

---

## Endpoint Details

---

## Auth Endpoints

### Register User

* **URL:** `/api/auth/signup/`
* **Method:** `POST`
* **Auth Required:** No

**Request Example**

```json
{
  "username": "sarvjeet",
  "email": "sarvjeet@test.com",
  "password": "Password@123",
  "password_confirm": "Password@123",
  "first_name": "Sarvjeet",
  "last_name": "Kumar",
  "phone_no": "+919876543210"
}
```

**Success Response (201 Created)**

```json
{
  "user_id": "018f3a3a-3c4a-718e-8a9d-5f3a3c4a718e"
}
```

**Error Responses**

| Code | Response                              |
| ---- | ------------------------------------- |
| 400  | `{ "errors":"Invalid email format."}` |

---

### Login User

* **URL:** `/api/auth/login/`
* **Method:** `POST`
* **Auth Required:** No

**Request Example**

```json
{
  "username": "sarvjeet",
  "password": "Password@123"
}
```

**Success Response (200 OK)**

```json
{
  "access": "jwt_access_token",
  "refresh": "jwt_refresh_token"
}
```

**Error Responses**

| Code | Response                                     |
| ---- | -------------------------------------------- |
| 400  | `{ "error": "Invalid credentials." }`        |

---

### Logout User

* **URL:** `/api/auth/logout/`
* **Method:** `POST`
* **Auth Required:** User

**Request Example**

```json
{
  "refresh": "jwt_refresh_token"
}
```

**Success Response (200 OK)**

```json
{
  "message": "Logout successful."
}
```

---

## Books Endpoints (User/Admin)

### List All Books

* **URL:** `/api/books/`
* **Method:** `GET`
* **Auth Required:** User

**Query Parameters**

| Parameter | Type   | Description             |
| --------- | ------ | ----------------------- |
| `genre`   | string | Filter by genre name    |
| `title`   | string | Filter by title keyword |

**Success Response (200 OK)**

```json
{
  "count": 1,
  "next": null,
  "previous": null,
  "data": [
    {
      "id": "018f3a3a-3c4a-718e-8a9d-5f3a3c4a718e",
      "title": "Atomic Habits",
      "author": "James Clear",
      "isbn": "9780735211292",
      "published_year": 2018,
      "genres": ["Self-Help", "Psychology"],
      "created_at": "2024-03-21T10:00:00Z"
    }
  ]
}
```

---

### Create New Book

* **URL:** `/api/books/`
* **Method:** `POST`
* **Auth Required:** Admin

**Request Example**

```json
{
  "title": "Deep Work",
  "author": "Cal Newport",
  "isbn": "9781455586691",
  "published_year": 2016,
  "genres": ["Productivity", "Focus"]
}
```

**Success Response (201 Created)**

```json
{
  "id": "018f3a3a-3c4a-718e-8a9f-5f3a3c4a718f",
  "title": "Deep Work",
  "author": "Cal Newport",
  "isbn": "9781455586691",
  "published_year": 2016,
  "is_active": true,
  "genres": ["Productivity", "Focus"],
  "created_at": "2024-03-21T10:05:00Z"
}
```

---
### Request New Book

* **URL:** `/api/books/`
* **Method:** `POST`
* **Auth Required:** User

**Request Example**

```json
{
  "title": "Deep Work",
  "author": "Cal Newport",
  "isbn": "9781455586691",
  "published_year": 2016,
  "genres": ["Productivity", "Focus"]
}
```

**Success Response (201 Created)**

```json
{
  "id": "018f3a3a-3c4a-718e-8a9f-5f3a3c4a718f",
  "title": "Deep Work",
  "author": "Cal Newport",
  "isbn": "9781455586691",
  "published_year": 2016,
  "genres": ["Productivity", "Focus"],
  "created_at": "2024-03-21T10:05:00Z"
}
```

---

### Get Book Details

* **URL:** `/api/books/{id}/`
* **Method:** `GET`
* **Auth Required:** User

**Success Response (200 OK)**

```json
{
  "id": "018f3a3a-3c4a-718e-8a9d-5f3a3c4a718e",
  "title": "Atomic Habits",
  "author": "James Clear",
  "isbn": "9780735211292",
  "published_year": 2018,
  "genres": ["Self-Help", "Psychology"]
}
```


---

## My Books Endpoints (User)

### List Personal Books

* **URL:** `/api/my-books/`
* **Method:** `GET`
* **Auth Required:** User

**Success Response (200 OK)**

```json
{
  "count": 1,
  "next": null,
  "previous": null,
  "data": [
    {
      "id": "018f3a3a-3c4a-718e-8a9d-5f3a3c4a718e",
      "title": "Atomic Habits",
      "author": "James Clear",
      "status": "READING",
      "genres": ["Self-Help"]
    }
  ]
}
```

---

### Add Book to My List

* **URL:** `/api/my-books/`
* **Method:** `POST`
* **Auth Required:** User

**Request Example**

```json
{
  "id": "018f3a3a-3c4a-718e-8a9d-5f3a3c4a718e",
  "status": "TO_READ"
}
```

**Success Response (201 Created)**

```json
{
  "message": "Book added to your list."
}
```

---

### Update Personal Book Status

* **URL:** `/api/my-books/{id}/`
* **Method:** `PATCH`
* **Auth Required:** User

**Request Example**

```json
{
  "status": "COMPLETED"
}
```

**Success Response (200 OK)**

```json
{
  "message": "Book status updated successfully."
}
```

---

### Remove from My List

* **URL:** `/api/my-books/{id}/`
* **Method:** `DELETE`
* **Auth Required:** User

**Success Response (204 No Content)**

---

## Admin Endpoints

### Get All Users

* **URL:** `/api/users/`
* **Method:** `GET`
* **Auth Required:** Admin

**Success Response (200 OK)**

```json
{
  "count": 10,
  "results": [
    {
      "id": "018f3a3a-3c4a-718e-8a9d-5f3a3c4a719a",
      "username": "sarvjeet",
      "email": "sarvjeet@test.com",
      "role": "USER"
    }
  ]
}
```

---
###  User Details

* **URL:** `/api/users/{id}/`
* **Method:** `GET`
* **Auth Required:** Admin

**Success Response (200 OK)**

---

### Delete User

* **URL:** `/api/users/{id}/`
* **Method:** `DELETE`
* **Auth Required:** Admin

**Success Response (204 No Content)**

---

## Error Codes

| Code | Meaning               |
| ---- | --------------------- |
| 200  | OK                    |
| 201  | Created               |
| 204  | No Content            |
| 400  | Bad Request           |
| 401  | Unauthorized          |
| 403  | Forbidden             |
| 404  | Not Found             |
| 500  | Internal Server Error |

---

### Common Error Response

```json
{
  "error": "Error message describing the issue"
}
```
