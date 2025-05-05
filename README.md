# Module 08 -- High Level Networking

Module 8 Tutorial

## Key Differences: Unary, Server Streaming, Bidirectional Streaming RPC

| Type                  | Description                                                       | Suitable Scenarios                         |
|-----------------------|-------------------------------------------------------------------|-------------------------------------------|
| **Unary**             | Single request → single response.                                 | Simple data fetch, payment processing.     |
| **Server Streaming**  | Single request → stream of responses from server.                 | Transaction history, notifications.        |
| **Bidirectional**     | Both client and server exchange streams of messages simultaneously.| Chat apps, live updates, collaborative apps.|

---

## Security Considerations in Rust gRPC

| Area            | Considerations                                                                 |
|-----------------|-------------------------------------------------------------------------------|
| **Authentication** | Implement mutual TLS or token-based schemes (JWT, OAuth2).                    |
| **Authorization**  | Enforce fine-grained access control policies per service and method.         |
| **Encryption**     | Use TLS (transport layer security) to encrypt communication by default.      |

Additionally:
- Validate input data thoroughly.
- Monitor for DoS/DDoS attack patterns (especially on streaming services).
- Use secure coding practices for async Rust.

---

## Challenges in Bidirectional Streaming (Rust gRPC)

- **Backpressure management**: Avoid overwhelming clients or servers with unprocessed messages.
- **Connection lifecycle**: Handle dropped or stalled connections gracefully.
- **Task cancellation**: Properly shut down streams when clients disconnect.
- **Concurrency complexity**: Use `tokio::mpsc` channels carefully to avoid deadlocks or memory leaks.

---

## Using `ReceiverStream`: Pros & Cons

| Pros                                              | Cons                                               |
|---------------------------------------------------|----------------------------------------------------|
| Easy conversion from Tokio `mpsc::Receiver` to stream. | Buffer size management required to avoid memory bloat. |
| Simplifies async stream handling in server methods. | Potential bottleneck if sender is slow or blocked.   |
| Clean integration with Tonic streaming APIs.      | Not as flexible as custom Stream implementations.    |

---

## Structuring Rust gRPC Code for Modularity & Reuse

- **Separate .proto files** per service → keep interfaces decoupled.
- **Modular service implementations** → one Rust module per service.
- Use traits and generics where possible.
- Abstract common utilities (e.g., authentication, logging).
- Centralized error handling via custom error types.
- Isolate business logic from transport (gRPC) layer.

---

## Extending `MyPaymentService` for Real Payment Logic

1. **Input validation** → Check payment amounts, user ID formats.
2. **Business rule enforcement** → Fraud checks, duplicate payment detection.
3. **Database integration** → Store and retrieve payment records.
4. **Error handling** → Define clear error codes & responses.
5. **Transaction rollback** → Use transactional DB operations for consistency.
6. **Idempotency** → Prevent repeated processing of the same payment.

---

## Impact of gRPC on System Architecture

| Advantage                                         | Challenge                                         |
|---------------------------------------------------|---------------------------------------------------|
| Efficient binary serialization (Protobuf).        | Requires consistent schema management.            |
| Built-in streaming support.                      | More complex error handling than REST.            |
| Strongly typed interfaces → safer development.    | Less flexible for ad-hoc changes than REST.       |
| Cross-language support (C++, Go, Java, Python).   | New tech stack adoption may involve learning curve.|

---

## HTTP/2 vs HTTP/1.1 & WebSocket

| Protocol    | Pros                                             | Cons                                             |
|-------------|--------------------------------------------------|--------------------------------------------------|
| **HTTP/2**  | Multiplexed streams, header compression, binary. | Requires server support, more complex debugging. |
| **HTTP/1.1**| Simple, widely supported.                       | No multiplexing, more overhead per request.      |
| **WebSocket**| Full-duplex communication over HTTP/1.1.       | Less structured than gRPC, requires custom protocols.|

---

## REST vs gRPC Streaming

| Feature             | REST                                 | gRPC Streaming                                 |
|---------------------|-------------------------------------|-----------------------------------------------|
| Communication model | Request-response only.              | Supports client, server, and bidirectional streaming. |
| Real-time support   | Poor (requires polling or WebSocket).| Native, efficient real-time streaming.         |
| Flexibility         | More flexible, schema-less (JSON).   | Strict schema enforcement (Protobuf).          |

---

## Schema-based (gRPC) vs Schema-less (REST/JSON)

| Aspect             | gRPC (Protobuf)                   | REST (JSON)                        |
|--------------------|-----------------------------------|-----------------------------------|
| Type safety        | Strongly typed.                   | Schema-less → runtime errors possible. |
| Efficiency         | Compact binary format.            | Text-based → larger payloads.     |
| Evolution          | Supports backward/forward compatibility with versioning. | Manual versioning required.       |
| Interoperability   | Requires Protobuf support in clients.| Universally supported via JSON.  |



