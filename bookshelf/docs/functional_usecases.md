# Functional Use Case Documentation

---

## Use Case 1: User Registration & Login
**Role:** User  

**Description:**  
Allows a user to create an account and log in to the BookShelf system using JWT authentication.

**Steps:**
- User enters username, email, password, password confirmation, first name, last name, and phone number.
- System validates the data (e.g., email format, password match, alpha characters in names).
- User credentials and profile are securely stored in the database.
- System generates an authentication token (Access and Refresh) for the user.

**Post-condition:**  
User is successfully registered and can authenticate for subsequent requests.

**Exceptions:**
- Duplicate username or email.
- Invalid email or phone number format.
- Passwords do not match or are too short.

---

## Use Case 2: Personal Bookshelf Management
**Role:** User  

**Description:**  
Allows users to maintain a personal collection of books by adding them from the public catalog.

**Steps:**
- User searches or browses the public book catalog.
- User selects a book by its UUID and provides an initial reading status.
- System verifies the book exists and is active.
- System links the book to the user's personal collection.

**Post-condition:**  
Book is successfully added to the user's personal list with the specified status.

**Exceptions:**
- Book already exists in the user's list.
- Book is inactive or deleted.
- Invalid book UUID.

---

## Use Case 3: Reading Status Tracking
**Role:** User  

**Description:**  
Allows users to track and update the status of books in their personal collection (e.g., TO_READ, READING, COMPLETED).

**Steps:**
- User selects a book from their personal list.
- User updates the `status` field.
- System validates the status against the allowed choices (`TO_READ`, `READING`, `COMPLETED`).
- Updated status and `updated_at` timestamp are stored.

**Post-condition:**  
Reading status is successfully updated for the specific user-book relationship.

**Exceptions:**
- Invalid status value.
- Book not found in user's personal list.

---

## Use Case 4: Viewing Personal and Public Books
**Role:** User / Admin

**Description:**  
Allows users to view the available book catalog and their own personal subset with pagination and filtering.

**Steps:**
- User sends a request to list books (public or personal).
- System applies filters (genre, title) and pagination.
- For personal books, the system filters by the authenticated user's ID.
- Paginated results are returned with book details and status.

**Post-condition:**  
User receives a structured list of books.

**Exceptions:**
- Unauthorized access (no token).

---

## Use Case 5: Administrative System Management
**Role:** Admin  

**Description:**  
Allows admins to manage the entire book catalog, including creation, updates, and soft-deletion.

**Steps:**
- Admin creates a new book with details like ISBN, title, and genres.
- Admin updates existing book information or activates/deactivates books.
- Admin performs a soft delete on a book, which also soft-deletes related genre links.
- Admin manages user accounts (viewing or deleting).

**Post-condition:**  
System-wide data is updated, and changes are reflected for all users.

**Exceptions:**
- Duplicate ISBN during creation/update.
- Attempting to delete oneself (admin account).
- Unauthorized access to admin-only endpoints.

---

## Use Case 6: Unauthorized Access Handling
**Role:** System  

**Description:**  
Ensures that all protected resources are accessed only by authenticated users with the appropriate roles.

**Steps:**
- User attempts to access a protected API.
- System validates the JWT in the `Authorization` header.
- System checks the user's role (USER or ADMIN) against endpoint permissions.
- Access is granted or an error is returned.

**Post-condition:**  
Data integrity and security are maintained by preventing unauthorized actions.

**Exceptions:**
- Missing, invalid, or expired Bearer token.
- User role lacks sufficient permissions (e.g., USER trying to access `/api/users/`).
