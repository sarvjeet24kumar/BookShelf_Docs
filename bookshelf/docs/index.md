# BookShelf – Personalized Library Management System

## Overview
BookShelf is a REST-based Library Management System built with **Django** and **Django REST Framework**, designed to help users organize, track, and manage their personal book collections in a secure and structured manner.

It serves as a centralized digital bookshelf where users can maintain records of the books they own, are currently reading, or plan to read in the future.

The platform enables:

- Users to add, update, and manage their personal book collections.
- Admins to monitor, manage, and moderate book data across the entire system.

---

## Key Goals

### 1. Centralize Personal Book Management
All books owned by a user—whether planned (`TO_READ`), currently being read (`READING`), or completed (`COMPLETED`)—are stored in a single digital platform.

Users can easily track reading progress without relying on manual notes. This improves organization, accessibility, and reading consistency through a structured backend.

---

### 2. Enable Role-Based Functionality
The system defines clear roles and permissions:

- **Users:**  
  Can browse the public book catalog and manage books in their personal bookshelf (add, view, update status, and soft-delete).

- **Admins:**  
  Can manage the entire book database (create new books, update information) and perform administrative tasks like user management.

---

## Stakeholders

### Users
- Maintain a personalized digital bookshelf.
- Track reading status using predefined categories (`TO_READ`, `READING`, `COMPLETED`).
- Manage book information securely with a private collection view.

### Admins
- View and manage system-wide book data and genres.
- Moderate book entries and deactivate/activate books for the public catalog.
- Manage user profiles and ensure platform-level data integrity.

---

## Scope

### 1. Technology Stack
- **Backend Framework:** Django 6.0
- **API Development:** Django REST Framework (DRF)
- **Database:** PostgreSQL
- **Dependency Management:** `uv`
- **Authentication:** JWT (SimpleJWT) with access and refresh tokens.

---

### 2. Core Features
- User registration and login system with secure password validation.
- Token-based authentication and authorization.
- Personal bookshelf management with soft-delete functionality.
- Enum-based reading status tracking.
- RESTful API endpoints following standard HTTP methods.
- Role-based access control (USER/ADMIN).
- Global exception handling for unified API responses.
- Paginated data access for performance and scalability.

---

## Authentication System
BookShelf uses **SimpleJWT** to provide secure, stateless authentication and controlled API access throughout the system.

- **Signup/Login APIs:**  
  Allow users to authenticate using their credentials and receive JWT tokens.

- **Access and Refresh Tokens:**  
  Support secure sessions with token rotation and blacklisting on logout.

- **Role-based Permissions:**  
  Custom permissions (`IsAdmin`, `IsUser`) ensure that only authorized roles can access specific endpoints.

This mechanism enhances data security, ensures data isolation between users, and allows for seamless integration with modern frontend clients.

---

## Expected Outcome
By the end of this project, the BookShelf platform delivers the following outcomes:

### 1. For Users
- A secure and personalized digital bookshelf experience.
- Reliable tracking of reading progress across multiple status categories.
- Simplified book management through a clean API interface.

### 2. For Admins
- Centralized visibility into all platform data.
- Robust tools to moderate content and manage the book catalog.
- Automated role-enforcement reducing the need for manual oversight.

### 3. For the System
- A scalable and maintainable REST API architecture.
- Strong enforcement of data isolation and soft-delete consistency.
- A future-proof foundation ready for features like book reviews and community recommendations.

---

## Conclusion
BookShelf simplifies personal book management by providing a secure, structured, and role-based digital platform. By utilizing Django, DRF, and modern JWT authentication, the project demonstrates a professional-grade backend system designed for reliability, scalability, and ease of use.
