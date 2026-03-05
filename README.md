JWT Authentication in Oracle APEX & ORDS
Secure API Authentication using JSON Web Tokens



Overview

This project demonstrates how to implement JWT (JSON Web Token) authentication in Oracle APEX and Oracle REST Data Services (ORDS).

JWT authentication is widely used in modern API architectures to provide stateless, secure, and scalable authentication.
This repository provides a complete end-to-end implementation including:

JWT Token Generation

JWT Token Validation

ORDS Secure REST APIs

APEX Integration

PL/SQL Implementation

This project is designed as a learning resource and technical reference for developers working with Oracle APEX APIs and ORDS security.

Architecture
User / Client
      │
      │  Login Request
      ▼
Oracle APEX
      │
      │  Generate JWT
      ▼
ORDS REST API
      │
      │  Validate JWT
      ▼
Protected Database Resources

Flow

User authenticates via APEX

System generates a JWT token

Client sends JWT in API request header

ORDS validates the token

If valid → API response is returned

Repository Structure
jwt-apex-ords-authentication
│
├── README.md
│
├── database
│   ├── jwt_generate.sql
│   ├── jwt_validate.sql
│
├── ords
│   ├── secure_api.sql
│   ├── api_handler.sql
│
├── apex
│   ├── apex_login_process.sql
│   ├── apex_jwt_usage.sql
│
└── docs
    ├── architecture.png
    └── jwt-flow-diagram.png
Technologies Used

Oracle Database

Oracle APEX

Oracle REST Data Services (ORDS)

PL/SQL

JSON Web Tokens (JWT)

REST APIs

JWT Token Generation (PL/SQL)

Example function to generate a JWT token.

FUNCTION generate_jwt_token(
    p_username VARCHAR2
) RETURN VARCHAR2 IS
    l_payload VARCHAR2(4000);
BEGIN
    l_payload :=
        '{"user":"' || p_username || '",' ||
        '"iat":"' || TO_CHAR(SYSDATE,'YYYYMMDDHH24MISS') || '"}';

    RETURN apex_jwt.encode(
        p_payload => l_payload,
        p_secret  => 'MY_SECRET_KEY'
    );
END;
JWT Token Validation
FUNCTION validate_jwt_token(
    p_token VARCHAR2
) RETURN BOOLEAN IS
    l_payload CLOB;
BEGIN
    l_payload :=
        apex_jwt.decode(
            p_token  => p_token,
            p_secret => 'MY_SECRET_KEY'
        );

    RETURN TRUE;

EXCEPTION
    WHEN OTHERS THEN
        RETURN FALSE;
END;
ORDS Secure API Example

Client must send the JWT token in the request header.

Authorization: Bearer <JWT_TOKEN>

Example API endpoint:

GET /ords/demo/secure/customers

If the token is valid, ORDS returns the API response.

Example API Request
GET /ords/demo/secure/customers
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
Security Considerations

When implementing JWT authentication:

Always use strong secret keys

Use HTTPS for all API communication

Implement token expiration

Avoid storing sensitive data inside JWT payload

Rotate secrets periodically

Use Cases

This implementation is useful for:

Securing ORDS REST APIs

Integrating external systems with Oracle APEX

Building microservice architectures

Implementing stateless authentication

How to Use

Clone the repository

git clone https://github.com/YOUR_USERNAME/jwt-apex-ords-authentication.git

Run database scripts

database/jwt_generate.sql
database/jwt_validate.sql

Configure ORDS API modules

ords/secure_api.sql

Integrate with your Oracle APEX application.

Learning Resources

Oracle APEX Documentation

Oracle REST Data Services Documentation

JWT Specification (RFC 7519)

Author

Uthaman M

Oracle APEX Developer
Integration | APIs | EDI | Database Solutions

Contribution

Contributions are welcome.

If you would like to improve this project:

Fork the repository

Create a feature branch

Submit a Pull Request

License

This project is released under the MIT License.