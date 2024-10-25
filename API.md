# API Design and Best Practices

This document provides a comprehensive guide to API design, covering standard procedures, endpoint structures, data handling, error handling, and security practices. Following these conventions will ensure consistency, security, and ease of use for both backend and frontend developers.

## 1. Endpoint Structure and Naming Conventions

- **Endpoint**: `data` (essential)
- **Naming**: Endpoints must be **plural** nouns. Avoid using action-based naming like `/getGoods` or `/getUsers`.
- **Example**:
  - **Foods Endpoint**: `/foods` - Returns a list of food items such as `idly`, `dosa`, etc.

## 2. HTTP Methods

Use the following HTTP methods as per their functionality:

- **GET**: Retrieve data from the server (e.g., `/users`, `/products`).
- **POST**: Create new data on the server.
- **PUT**: Update all data for a specified resource.
- **PATCH**: Update part of a specified resource's data.
- **DELETE**: Delete a specific resource using its ID.
- **OPTIONS**: Send data to the server to retrieve information about communication options available for a specific resource.

### Example
- **GET Specific User**: `http://localhost:8080/user/1`
  - Here, `1` is a **path parameter** representing the user's unique ID to retrieve specific data.

## 3. Data Creation

- **Data Creation**: Use **POST** requests with a **request body** to create new entries in the database.
- **Request Body**: Should contain all necessary information for the new resource.

### Example
```json
{
  "name": "New User",
  "email": "user@example.com"
}
