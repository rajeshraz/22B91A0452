# URL Shortener Backend

A sleek, feature-rich URL shortener service built with Node.js, Express, and MongoDB. Transform long URLs into short, shareable links with analytics, expiry, and more.

---

## Features

- **Custom or Auto Shortcodes:** Choose your own or let the system generate one.
- **Time-based Expiry:** Set how long your links should work (default: 30 minutes).
- **Click Analytics:** Track clicks, referrers, and IPs.
- **Reliable Storage:** MongoDB for persistent, safe data.
- **Request Logging:** All requests and events are logged for transparency.
- **Smart Redirects:** Handles expired and invalid links gracefully.
- **Developer Friendly:** CORS enabled, auto port selection, and easy API.

---

## Project Structure

```
backend-test-submission/
├── app.js                # Main application entry point
├── package.json          # Dependencies and scripts
├── controllers/
│   └── shorturl.js       # URL shortener business logic
├── models/
│   └── ShortUrl.js       # Mongoose schema/model
├── routes/
│   └── shorturl.js       # API routes
├── services/             # (Reserved for future service logic)
├── test-api.js           # Automated API test script
logging-middleware/
└── log.js                # Express middleware for request logging
```

---

## Getting Started

### Prerequisites

- **Node.js** (v14+ recommended)
- **npm** (comes with Node.js)
- **MongoDB** (local or cloud, e.g., MongoDB Atlas)

### Setup

1. **Clone the repository and navigate to the project:**
   ```bash
   cd backend-test-submission
   ```

2. **Install dependencies:**
   ```bash
   npm install
   ```

3. **Configure environment variables:**
   Create a `.env` file in the `backend-test-submission` directory:
   ```
   MONGO_URI=mongodb://localhost:27017/urlshortener
   PORT=3000
   ```
   > If using MongoDB Atlas, replace `MONGO_URI` with your connection string.

4. **Start the server:**
   ```bash
   npm start
   ```
   The server will run on `http://localhost:3000` (or the next available port if 3000 is in use).

---

## API Usage

### 1. Create a Short URL

**POST** `/shorturls`

**Request:**
```json
{
  "original": "https://example.com",
  "expiry": "2025-07-01T12:00:00.000Z",  // Optional
  "shortcode": "custom123"               // Optional
}
```

**Response:**
```json
{
  "shortcode": "custom123",
  "original": "https://example.com",
  "expiry": "2025-07-01T12:00:00.000Z",
  "created": "2025-06-27T10:00:00.000Z"
}
```

---

### 2. Get Short URL Stats

**GET** `/shorturls/:shortcode`

**Response:**
```json
{
  "shortcode": "custom123",
  "original": "https://example.com",
  "created": "...",
  "expiry": "...",
  "clicks": 5,
  "clickLogs": [
    {
      "time": "...",
      "referrer": "...",
      "ip": "..."
    }
  ]
}
```

---

### 3. Redirect to Original URL

**GET** `/:shortcode`

Redirects to the original URL if valid and not expired.

- `404`: Shortcode not found
- `410`: URL expired

---

## Example Usage

**Create a short URL (curl):**
```bash
curl -X POST http://localhost:3000/shorturls \
  -H "Content-Type: application/json" \
  -d '{"original": "https://google.com", "shortcode": "google"}'
```

**Get stats:**
```bash
curl http://localhost:3000/shorturls/google
```

**Redirect:**
```bash
curl -L http://localhost:3000/google
```

---

## Automated Testing

A comprehensive test script is included to verify all API features.

### How to Run Tests

1. **Start your server:**
   ```bash
   npm start
   ```
   Wait for the message:  
   `Server is running on port 3000`

2. **In a new terminal, run:**
   ```bash
   npm test
   ```

### Troubleshooting

- If you see:
  ```
  Server Health Check Failed:
  Server is not running on localhost:3000. Please start your server first.
  ```
  - Make sure your server is running and listening on port 3000.
  - Wait for the "Server is running" message before running tests.
  - If your server started on a different port, update the `BASE_URL` and port in `test-api.js` accordingly.

---

## Data Model

```js
{
  shortcode: String,
  original: String,
  created: Date,
  expiry: Date,
  clicks: Number,
  clickLogs: [
    {
      time: Date,
      referrer: String,
      ip: String
    }
  ]
}
```

---

## Logging

- All requests and important events are logged to `../events.log`.
- Useful for debugging and analytics.

---

## License

MIT
