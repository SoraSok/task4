# task4

# Secure Local REST API

This project implements a secure REST API using Flask. It includes token-based authentication (JWT), Role-Based Access Control (RBAC), secure HTTPS communication using self-signed certificates, and comprehensive logging.

## Security Setup
- **HTTPS/TLS**: The server runs exclusively on HTTPS using `pyOpenSSL` with an adhoc self-signed certificate to encrypt data in transit (localhost only).
- **Authentication**: Implemented using JSON Web Tokens (JWT). Tokens expire after 1 hour.
- **Authorization**: Role-Based Access Control (RBAC) is enforced. 
  - `admin_user`: Can create/delete projects and view system logs.
  - `regular_user`: Can only read projects.
- **Logging**: All requests (IP, method, path, timestamps) and system errors/exceptions are logged to `api.log` to maintain an audit trail.

## API Usage Guide

*Note: Because we use a self-signed certificate, ensure SSL verification is disabled in Postman or add `-k` flag if using cURL.*

### 1. Authentication
- **Endpoint**: `POST https://127.0.0.1:5000/login`
- **Body (JSON)**: `{"username": "admin_user", "password": "password123"}` 
  *(Alternative user: "regular_user")*
- **Response**: Returns a JWT token. Use this token in the `Authorization` header as `Bearer <token>` for all subsequent requests.

### 2. CRUD Operations (Projects)
- **Get All Projects**: 
  - `GET https://127.0.0.1:5000/projects`
  - Auth: Required (Admin or User)
- **Create Project**: 
  - `POST https://127.0.0.1:5000/projects`
  - Body: `{"name": "New App", "description": "App details"}`
  - Auth: Required (Admin Only)
- **Delete Project**: 
  - `DELETE https://127.0.0.1:5000/projects/1`
  - Auth: Required (Admin Only)

### 3. Logging Dashboard
- **Endpoint**: `GET https://127.0.0.1:5000/logs`
- **Description**: Retrieves the 20 most recent server log entries (including IPs and errors).
- **Auth**: Required (Admin Only)
