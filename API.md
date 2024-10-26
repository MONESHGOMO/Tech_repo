# API Concepts Guide

## 1. Basic Endpoint Design
   - **Endpoint Design**: Always use plural nouns for endpoints (e.g., `/foods`, `/users`). Avoid using verbs like `getUsers` or `getFoods`. Stick to nouns for clarity and convention.
   - **Essential Data Endpoint**: Ensure there’s a primary endpoint to access essential data, which may include detailed or general information (e.g., `/data`).

## 2. Standard HTTP Methods
   - **GET**: Retrieve data.
   - **POST**: Create new data.
   - **PUT**: Update existing data fully.
   - **PATCH**: Update a part of existing data.
   - **DELETE**: Delete existing data.
   - **OPTIONS**: Send partial data to the server, often used to check available HTTP methods on a resource.

## 3. Path and Query Parameters
   - **Path Parameter**: Identified by `{}` in endpoint (e.g., `GET http://localhost:8080/users/{id}`). Represents a specific resource, like a user ID in `GET http://localhost:8080/users/1`.
   - **Query Parameters**: Optional parameters after the `?` in a URL (e.g., `GET /users?name=John`). Useful for filtering, sorting, or searching data.

## 4. Request and Response Formats
   - **Request Body**: Data submitted during `POST` and `PUT` requests. Should include JSON, XML, or relevant format details.
   - **Response Content Types**: APIs should clearly specify supported content types (`Content-Type`), such as JSON, XML, HTML, TEXT, or Markdown.

## 5. Standard API Operations
   - **Creating Data**: Use `POST` requests with data in the request body.
   - **Updating Data**: Use `PUT` (full data) or `PATCH` (partial data).
   - **Deleting Data**: Use `DELETE` requests, often with a path parameter to specify the resource to delete.

## 6. Hierarchical IDs
   - **Hierarchical IDs**: Used to establish relationships in resources (e.g., `/users/{userId}/orders/{orderId}`).

## 7. Error Handling and Messages
   - **Error Descriptions**: Provide meaningful descriptions for error messages. This helps frontend developers identify and handle issues accurately.
   - **Missing Path/Query Parameters**: Clearly specify errors when expected parameters are missing, for example, `400 Bad Request - Missing Path Parameter: userId`.
   - **Standardized Status Codes**: Follow standard HTTP status codes (e.g., `404 Not Found`, `500 Internal Server Error`, `400 Bad Request`).

## 8. Security and Threat Prevention
   - **Cross-Site Scripting (XSS)**: Sanitize user inputs to prevent XSS attacks.
   - **SQL Injection**: Use parameterized queries to avoid SQL injection vulnerabilities.

## 9. Rate Limiting and Throttling
   - **API Throttling and Rate Limiting**: Implement rate limits to control API traffic, preventing abuse and ensuring system stability.

## 10. Pagination
   - **Pagination**: For large datasets, use pagination to split results across pages, improving performance and readability (e.g., `/users?page=2&size=20`).

## 11. API Deprecation
   - **API Deprecation**: Mark older versions of APIs as deprecated with clear communication to allow developers time to migrate.

---

This guide provides a comprehensive overview of standard practices in API design and usage. Each section addresses a crucial aspect of building and maintaining secure, efficient, and user-friendly APIs.
