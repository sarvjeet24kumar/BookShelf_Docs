# Functional Modules

---

## 1. Authentication Module
- Token-based user registration and login.
- Secure password hashing and validation using Django authentication.
- Role-based access control: **User / Admin**.
- Token validation for all protected APIs.

---

## 2. Book Management Module
- Create, read, update, and delete books.
- Each book is linked to an authenticated user.
- Validation for required book fields such as title, author, and reading status.
- Enforced Enum-based reading status values.

---

## 3. Reading Status Tracking Module
- Track reading progress using predefined statuses.
- Allowed statuses:
  - TO_READ
  - READING
  - COMPLETED
- Prevent invalid or unsupported status values.
- Enable seamless updates to book reading progress.

---

## 4. Access Control & Permissions Module
- Custom permission classes for role-based access.
- Users can access and manage only their own books.
- Admins can view and delete any book in the system.
- Prevent unauthorized access to protected resources.

---


## 5. Admin Moderation Module
- System-wide visibility into all book records.
- Ability to remove invalid or inappropriate book entries.


---