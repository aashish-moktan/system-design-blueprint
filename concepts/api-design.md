# Comprehensive Guide to API Design in System Design Interviews

---

## Overview of API Design

API design defines how different software components communicate, especially between:

- Client (Web/Mobile applications)
- Server
- Backend services

In system design interviews, API design usually comes after identifying core entities and before designing the high-level architecture.

The primary goal is to define:

- How clients and servers communicate
- Which protocol should be used
- How resources are structured
- How security and scalability concerns are handled

A good API design should be:

- Simple
- Consistent
- Scalable
- Easy to maintain

---

## API Design Process in Interviews

A typical system design interview flow:

1. Requirements
2. Core Entities
3. API Design
4. Data Flow
5. High-Level Design
6. Deep Dives

### Interview Strategy

API design should be concise. Focus on:

- Important endpoints
- Request/response structure
- Protocol selection
- Security considerations

Avoid spending excessive time designing every small detail.

---

## Primary Communication Protocols

### 1. REST (Representational State Transfer)

REST is the default choice for external client-server communication.

It uses:

- HTTP methods
- Resource-based URLs
- JSON responses

REST is preferred because it is:

- Universal
- Human-readable
- Easy to debug
- Widely supported

---

### REST Resource Modeling

REST resources should map directly to core entities.

Example:

- `/users`
- `/events`
- `/tickets`

#### Naming Convention

Use plural nouns.

**Good:**

- GET /events

**Avoid:**

- GET /getEvents

The HTTP method defines the action, not the URL.

---

#### Path Parameters

Used to identify specific resources.

Example:

- GET /events/123

---

#### Query Parameters

Used for filtering, sorting, or refining requests.

Example:

- GET /events?location=LA

---

### HTTP Methods

| Method | Action           | Idempotency | Use Case               |
| ------ | ---------------- | ----------- | ---------------------- |
| GET    | Retrieve data    | Yes         | Fetch resources        |
| POST   | Create data      | No          | Create new resource    |
| PUT    | Replace resource | Yes         | Full update            |
| PATCH  | Partial update   | Partial     | Update specific fields |
| DELETE | Remove resource  | Yes         | Delete resource        |

---

### Response Standards

API responses usually contain:

- HTTP status code
- Response body (usually JSON)

#### Common Status Codes

**2xx Success**

- `200 OK`
- `201 Created`

**4xx Client Errors**

- `400 Bad Request`
- `401 Unauthorized`
- `404 Not Found`

**5xx Server Errors**

- `500 Internal Server Error`

---

### 2. GraphQL

GraphQL is a query language that allows clients to request exactly the data they need.

It solves common REST problems:

- **Over-fetching**: Receiving more data than required.  
  _Example_: Requesting a user name but getting name, email, address, phone, history.

- **Under-fetching**: Multiple API calls are required to fetch related data.

---

#### GraphQL Characteristics

- **Single Endpoint**  
  Unlike REST (`/users`, `/posts`, `/comments`), GraphQL usually uses `/graphql`. The client sends a query describing the required data.

- **N+1 Problem**  
  A single query for multiple items triggers separate database queries for related data.  
  _Example_: Fetch 100 posts → 100 separate user queries.  
  **Solutions**: Data loaders, query batching.

- **Field-Level Authorization**  
  GraphQL supports permission checks at individual fields.  
  _Example_: Allowed (`username`, `profile_image`), Restricted (`salary`, `private_data`).

---

### 3. RPC and gRPC

RPC (Remote Procedure Call) is designed for internal service-to-service communication.

Instead of resource-based URLs, RPC models communication as function calls.

**REST:**
GET /users/123

**RPC:**
GetUser(123)

#### gRPC Advantages

gRPC uses:

- Protocol Buffers (Protobuf)
- Binary serialization

**Benefits:**

- Lower latency
- Smaller payload size
- Efficient communication

Best suited for:

- Microservices
- Internal APIs
- Backend-to-backend communication

REST is usually preferred for external clients because of its simplicity and compatibility.

---

## Advanced API Concepts

### Pagination Strategies

Pagination is required when an endpoint can return large datasets. Without pagination:

- High latency
- Large response payloads
- Increased memory usage

#### Offset-Based Pagination

Example:
?page=2&limit=20

**Advantages:**

- Simple implementation

**Disadvantages:**

- Can skip records
- Can return duplicates when data changes frequently

#### Cursor-Based Pagination

Uses a cursor to fetch the next set of results.

Example:
?cursor=abc123

**Advantages:**

- More stable for high-write systems
- Prevents duplicate results
- Prevents missing records

Commonly used in:

- Social media feeds
- Messaging systems
- Large datasets

---

## API Security

Security should be considered during API design.

### Authentication and Authorization

Authentication information should be passed through HTTP headers.

Common approaches:

- JWT Tokens
- Session Tokens

Example:
Authorization: Bearer <token>

#### Avoid User ID From Request Body

**Bad:**

```json
{
  "user_id": 123
}
```

# API Security, Real-Time Communication, and Key Takeaways

## Authentication and Authorization

### Avoid User ID From Request Body

**Why?**  
The client can modify this value.

**Better:**  
Server derives identity from authentication tokens.

---

### JWT (JSON Web Tokens)

JWT contains:

- User information
- Roles
- Expiration time

The server verifies the signature before trusting the data.

---

### Session Tokens

Session tokens contain an identifier. The server uses this identifier to retrieve session information from:

- Database
- Cache

---

## Real-Time Communication

Some applications require continuous communication. Examples:

- Chat applications
- Live notifications
- Collaboration tools

### WebSockets

Provides two-way communication between client and server.

Used for:

- Chat
- Live updates
- Multiplayer applications

---

### Server-Sent Events (SSE)

Allows servers to push updates to clients over HTTP.

Used for:

- Notifications
- Live feeds

---

### WebRTC

Used for:

- Peer-to-peer communication
- Audio/video streaming

---

## Key Takeaways

- API design defines communication between system components.
- REST is the default choice for external client-server communication.
- Use plural nouns for REST resources.
- HTTP methods define actions, not URLs.
- GraphQL solves over-fetching and under-fetching problems.
- Handle GraphQL N+1 problems using batching or data loaders.
- gRPC is preferred for internal service communication.
- Use cursor pagination for high-write systems.
- Authentication should use headers, not request body fields.
- WebSockets, SSE, and WebRTC support real-time communication.
- Keep API design concise during system design interviews.
