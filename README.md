## Question 1

- **HTTP/0.9:** GET only; HTML-only responses; no headers or status codes; a separate connection for each request.
- **HTTP/1.0:** Headers and status codes; HEAD and POST methods; support for different content types; non-persistent connections by default.
- **HTTP/1.1:** Persistent connections by default; mandatory `Host` header; chunked transfer encoding; pipelining with responses returned in request order.
- **HTTP/2:** Binary framing; multiple concurrent streams over one TCP connection (multiplexing); HPACK header compression; Server Push; TCP head-of-line blocking remains when packets are lost.
- **HTTP/3:** QUIC over UDP; independent streams; QPACK header compression; TLS 1.3; faster connection establishment; connection migration when the network changes.

HTTP/3 uses UDP to enable QUIC. In HTTP/2, a lost TCP packet can delay the delivery of data for all streams on the connection until the missing packet is recovered. In QUIC, independent streams do not stop merely because data belonging to another stream is lost. QUIC also integrates connection establishment with encryption, reducing startup latency. Reliability, retransmission, and congestion control are provided by QUIC; UDP itself does not provide these guarantees.

## Question 2 Part A

In stateless communication, processing a request does not depend on retaining the state of earlier requests at the protocol level. Each request must therefore carry the information needed to process it. In stateful communication, previous interaction state is retained, and later requests can rely on it, such as a session that stores a user's login state.

## Question 2 Part B

HTTP is a stateless protocol. Web applications commonly use cookies and sessions to preserve user state between requests. The server creates a session and sends its identifier to the browser in a `Set-Cookie` header. The browser includes that identifier in subsequent requests using the `Cookie` header, allowing the server to retrieve the session information.

Another approach is to send a token, such as a JWT, with each request. These mechanisms operate at the application level and do not change HTTP's stateless nature.

## Question 3 Part A

In the usual HTTP request-response model, a server cannot send an independent response to a client without a request. Therefore, a client does not immediately learn about a new chat message or alert unless an additional mechanism is used. Repeatedly checking for updates also increases resource consumption.

Webhooks address this limitation by switching roles: the event producer acts as an HTTP client when sending the notification.

## Question 3 Part B

**Polling:** The client asks the server at fixed intervals whether new information is available. This is simple to implement, but requests are sent even when nothing has changed. Shorter intervals reduce delay while increasing server load and network usage.

**Long Polling:** The client sends a request, and the server delays its response until new data arrives or a timeout occurs. After receiving the response, the client sends another request. This reduces unnecessary responses and event delivery latency, but requires management of open requests and connection timeouts.

**HTTP Streaming:** The server keeps the response to a request open and gradually sends new data through that response. A separate request is therefore unnecessary for each message. However, buffering by intermediaries can prevent immediate delivery.

**Server-Sent Events (SSE):** The client uses `EventSource` to establish an HTTP connection, and the server sends events with the `text/event-stream` content type. SSE is suitable for one-way notifications and updates from a server to a browser, and supports automatic reconnection. Data sent from the client to the server uses separate requests.

**WebSocket:** The client initiates the connection, and an initial handshake establishes a persistent, bidirectional channel. Both sides can then send data without a separate HTTP request for every message. This is suitable for chat and interactive applications, although the application must manage disconnections and reconnections.

**Webhook:** The receiver provides an accessible endpoint for receiving events. When an event occurs, the producing service typically sends a `POST` request to that endpoint. The event producer acts as the client, and the receiver acts as the server, preserving HTTP's request-response model. This approach is suitable for communication between services.

## Question 4

`ss://asghar:1234!!@ss.myproxy.com:1234#shadowSocks1`

| Component | Value |
| --- | --- |
| Scheme | `ss` |
| Userinfo | `asghar:1234!!` |
| Username in generic URL parsing | `asghar` |
| Password in generic URL parsing | `1234!!` |
| Host | `ss.myproxy.com` |
| Subdomain | `ss` |
| Main domain | `myproxy.com` |
| Top-level domain | `com` |
| Port | `1234` |
| Path | Empty |
| Query | None |
| Fragment | `shadowSocks1` |

The symbols `://`, `@`, the colon before the port, and `#` separate the components. In an actual Shadowsocks configuration, the format and meaning of the information before `@` are defined by that scheme's specifications.

## Question 5 Part 1

**503 Service Unavailable:** Appropriate when the database outage is temporary and the login service is currently unavailable. If a suitable retry time is known, the response can include a `Retry-After` header.

If the problem is an unexpected internal error, **500 Internal Server Error** is also appropriate.

## Question 5 Part 2

**401 Unauthorized:** The supplied login credentials are invalid, so authentication has failed. The response should be the same for a nonexistent username and an incorrect password to avoid revealing whether an account exists.

Standard use of `401` also requires a `WWW-Authenticate` header.

## Question 5 Part 3

**301 Moved Permanently:** Appropriate for a permanent move to a new domain, with the destination provided in the `Location` header.

If the request method and body must be preserved, **308 Permanent Redirect** is more appropriate because, unlike `301`, it does not permit changing `POST` to `GET`.

## Question 5 Part 4

**429 Too Many Requests:** The user has exceeded the permitted request rate. A `Retry-After` header can indicate how long the client should wait before trying again.

## Question 5 Part 5

**204 No Content:** The renewal succeeded, and no response body is needed because the renewed token is returned in a header. A `204` response may include headers, but it must not include a body.

If a response body is also returned, **200 OK** is appropriate.

## Question 5 Part 6

**403 Forbidden:** The server understands the request but denies access because of its IP restriction policy.

If access is blocked specifically because of a legal requirement, **451 Unavailable For Legal Reasons** may also be appropriate. A geographical restriction alone does not necessarily justify `451`.

## Question 6 Part 1

**Caching Forward Proxy:** A caching proxy within the university network, such as `apt-cacher-ng`, stores downloaded packages and serves subsequent requests from its cache.

If the internal server simply hosts a synchronized copy of the repository and clients download directly from it, the more accurate term is **Mirror**; it is not necessarily a proxy.

## Question 6 Part 2

**Reverse Proxy with Geo Load Balancing:** Requests are routed to an appropriate server or data center based on the geographical location of their source.

If destination selection happens only through DNS, the more precise mechanism is **GeoDNS**; DNS itself is not a proxy.

## Question 6 Part 3

**Authentication Reverse Proxy:** A reverse proxy is placed in front of the legacy service. It performs authentication and forwards only authorized requests to the underlying service, adding authentication without major changes to the legacy service's code.

---

This translation was completed using GPT.
