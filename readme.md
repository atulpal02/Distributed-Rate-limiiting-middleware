Distributed Rate Limiting Middleware

A high-performance distributed rate limiting middleware built with Node.js and Redis to protect APIs from abuse and ensure fair usage across different user tiers.

This middleware uses the Token Bucket algorithm and Redis to enforce rate limits consistently across multiple servers in a distributed environment.

Features

Tier-based rate limiting (Basic, Pro, etc.)

Distributed request tracking using Redis

Token Bucket algorithm for burst handling

Stateless middleware supporting horizontal scaling

Low-latency request validation with O(1) Redis operations

Suitable for multi-server deployments behind load balancers

Architecture
Client
   │
   ▼
Load Balancer
   │
   ▼
Node.js API Servers (Multiple Instances)
   │
   ▼
Redis (Shared Token Store)

Redis acts as the centralized store for tokens, ensuring that rate limits remain consistent across all application instances.

Rate Limiting Strategy

This project uses the Token Bucket Algorithm.

Each user is assigned a bucket containing a number of tokens:

Example tiers:

Tier	Limit
Basic	10 requests per minute
Pro	100 requests per minute
How it works

Each request attempts to consume a token.

If tokens are available → request is allowed.

If no tokens remain → request is rejected.

Tokens refill over time based on the configured rate.

This allows short bursts of traffic while still enforcing long-term limits.

Tech Stack

Node.js

Express.js

Redis

Docker (optional)

Nginx / Load Balancer (for multi-server deployment)

Installation

Clone the repository:

git clone https://github.com/yourusername/distributed-rate-limiter.git
cd distributed-rate-limiter

Install dependencies:

npm install

Start Redis:

redis-server

Run the application:

npm start
Environment Variables

Create a .env file:

REDIS_HOST=localhost
REDIS_PORT=6379
PORT=3000
Usage

Import the middleware in your Express application.

const rateLimiter = require("./middleware/rateLimiter");

app.use("/api", rateLimiter("basic"));

Example with tier configuration:

app.use("/basic-api", rateLimiter("basic"));
app.use("/pro-api", rateLimiter("pro"));
Example Response (Rate Limit Exceeded)
{
  "error": "Rate limit exceeded. Please try again later."
}

HTTP Status Code:

429 Too Many Requests
Project Structure
project
│
├── middleware
│   └── rateLimiter.js
│
├── config
│   └── redis.js
│
├── routes
│   └── apiRoutes.js
│
├── app.js
└── package.json
Future Improvements

IP-based rate limiting

API key support

Rate limit analytics

Redis cluster support

Monitoring with Prometheus/Grafana

Why Redis?

Redis is used because it provides:

Fast in-memory operations

Atomic commands

Low latency

Shared state across multiple servers

This makes it ideal for distributed rate limiting systems.

License

MIT License

Author

Atul Pal

GitHub: https://github.com/atulpal02

LinkedIn: https://linkedin.com/in/atulpal02

<img width="778" height="298" alt="Screenshot 2026-01-18 at 7 21 35 PM" src="https://github.com/user-attachments/assets/3d98ca39-3284-44fa-a359-d74b440ce097" />
<img width="778" height="672" alt="Screenshot 2026-01-18 at 7 24 24 PM" src="https://github.com/user-attachments/assets/b404425b-f014-4dbe-8540-634ff5d272d5" />
