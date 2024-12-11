# How to Write a Perfect OpenAPI Specification

Creating a robust OpenAPI Specification is essential for building well-documented, secure, and easy-to-understand APIs. Follow these steps and guidelines to create specifications like the provided "perfect" example.

---

## General Structure
An OpenAPI Specification consists of the following main sections:

1. **OpenAPI Metadata**: Basic information about the API.
2. **Servers**: List of environments where the API is hosted.
3. **Security**: Define the authentication mechanisms.
4. **Components**: Define reusable objects (schemas, enums, security schemes).
5. **Paths**: Define the API endpoints and their behaviors.

---

## Writing a Perfect OpenAPI Specification

### 1. **Metadata (`info`)**
Define the basic information about your API:
- `title`: The name of your API.
- `version`: The current version of the API.
- `description`: A short description of what the API does.

Example:
```yaml
info:
  title: founder-matching-api
  version: "1.0.0"
  description: The API for the FounderMatching project by HarDeconstruction
```

---

### 2. **Servers**
List all the environments where the API is available:
- `description`: A description of the environment (e.g., production, development).
- `url`: The base URL of the API environment.

Example:
```yaml
servers:
  - description: SwaggerHub API Auto Mocking
    url: https://virtserver.swaggerhub.com/DUCLNC2004/FounderMatching/1.0.0
```

---

### 3. **Security**
Define the security schemes used by your API. For example, if using JWT for authentication:
- `type`: Must be `http`.
- `scheme`: Use `bearer` for bearer tokens.
- `bearerFormat`: Specify the token format (e.g., `JWT`).

Example:
```yaml
components:
  securitySchemes:
    bearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT
```

Apply the security globally:
```yaml
security:
  - bearerAuth: []
```

---

### 4. **Components**
Reusable definitions for data types, enums, and objects.

#### a. Enums
- Define a reusable enumeration using `type: string` and `enum`.
- Use meaningful names for the schema.

Example:
```yaml
GenderType:
  type: string
  enum:
    - male
    - female
    - other
```

#### b. Data Type Constraints
- For **numbers**:
  - Specify `type: integer` or `type: number`.
  - Include `format: int32` or `format: float`.
  - Define `minimum` and `maximum` values.
  
  Example:
  ```yaml
  UserID:
    type: integer
    format: int32
    minimum: 1
    maximum: 99999999
  ```

- For **strings**:
  - Specify `type: string`.
  - Include `maxLength` to enforce a character limit.
  - Define a `pattern` using regex for validation.

  Example:
  ```yaml
  Email:
    type: string
    format: email
    maxLength: 255
    pattern: "^[^@\\s]+@[^@\\s]+\\.[^@\\s]+$"
  ```

---

### 5. **Paths**
Define the API endpoints and operations (GET, POST, PATCH, DELETE).

#### a. Path-Level Structure
- `tags`: Group endpoints for easier documentation.
- `summary`: A short description of the endpoint.
- `description`: A more detailed explanation of the operation.
- `operationId`: A unique identifier for the operation.
- `responses`: Define possible HTTP status codes and their behaviors.

Example:
```yaml
/users/me:
  get:
    tags:
      - Profile
    summary: Get the current user's profile
    description: Fetch the profile details of the logged-in user.
    operationId: getUserProfile
    responses:
      '200':
        description: User profile details.
        content:
          application/json:
            schema:
              type: object
              properties:
                UserAccount:
                  $ref: '#/components/schemas/UserAccount'
      '404':
        description: Profile not found
        content:
          application/json:
            schema:
              type: object
              properties:
                message:
                  type: string
                  example: "Profile not found."
                  maxLength: 100
                  pattern: "^[A-Za-z0-9 ,.?!'-]+$"
      '401':
        description: Unauthorized
        content:
          application/json:
            schema:
              type: object
              properties:
                error:
                  type: string
                  example: "Unauthorized access"
                  maxLength: 100
                  pattern: "^[A-Za-z0-9 ,.?!'-]+$"
```

#### b. Parameters
Define input parameters for paths or requests:
- `name`: The parameter name.
- `in`: Specify where the parameter is located (`path`, `query`, `header`, etc.).
- `schema`: Define the data type and constraints.
- `required`: Specify if the parameter is mandatory.

Example:
```yaml
parameters:
  - name: id
    in: path
    required: true
    schema:
      type: integer
      format: int32
      minimum: 1
      maximum: 99999999
```

#### c. Request Body
For `POST` or `PATCH` requests, define a `requestBody`:
- `content`: Specify the content type (`application/json`).
- `schema`: Reference a reusable object or define inline.

Example:
```yaml
requestBody:
  content:
    application/json:
      schema:
        $ref: '#/components/schemas/CandidateProfile'
```

#### d. Responses
For each response, specify:
- `description`: A short explanation.
- `content`: Define the response type and schema.
- Use meaningful error messages in the `default` response.

Example:
```yaml
responses:
  '200':
    description: Profile created successfully.
    content:
      application/json:
        schema:
          type: object
          properties:
            message:
              type: string
              example: "Profile created successfully."
              maxLength: 100
              pattern: "^[A-Za-z0-9 ,.?!'-]+$"
  '404':
    description: Resource not found.
    content:
      application/json:
        schema:
          type: object
          properties:
            message:
              type: string
              example: "Resource not found."
              maxLength: 100
              pattern: "^[A-Za-z0-9 ,.?!'-]+$"
  '401':
    description: Unauthorized.
    content:
      application/json:
        schema:
          type: object
          properties:
            error:
              type: string
              example: "Unauthorized access."
              maxLength: 100
              pattern: "^[A-Za-z0-9 ,.?!'-]+$"
  default:
    description: Unexpected error
    content:
      application/json:
        schema:
          type: object
          properties:
            error:
              type: string
              example: "An unexpected error occurred."
              maxLength: 100
              pattern: "^[A-Za-z0-9 ,.?!'-]+$"
```

---

## Best Practices
1. **Define Reusable Schemas**: Use `$ref` to reference reusable schemas.
2. **Validation Everywhere**:
   - For **numbers**, define `minimum` and `maximum`.
   - For **strings**, use `maxLength` and `pattern`.
3. **Security**: Always define and apply security schemes.
4. **Descriptive Responses**: Include meaningful error messages with proper validation.
5. **Consistent Naming**: Use PascalCase or camelCase for property names.
6. **Global Defaults**: Provide a `default` response for unexpected errors.

---

## Example Schema Recap

Here’s how to write each schema element perfectly:

### Numbers
```yaml
UserID:
  type: integer
  format: int32
  minimum: 1
  maximum: 99999999
```

### Strings
```yaml
Email:
  type: string
  format: email
  maxLength: 255
  pattern: "^[^@\\s]+@[^@\\s]+\\.[^@\\s]+$"
```

### Enums
```yaml
GenderType:
  type: string
  enum:
    - male
    - female
    - other
```

---

By following these guidelines, you can ensure your OpenAPI Specification is comprehensive, secure, and easy to use.