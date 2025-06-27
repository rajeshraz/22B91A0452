# URL Shortener Backend – Project Guide

This repository contains a Node.js backend for a URL shortener service. It allows you to create, manage, and track short links with expiry and analytics, using MongoDB for storage.

---

## Table of Contents
- [Project Overview](#project-overview)
- [Folder Structure](#folder-structure)
- [Setup Process](#setup-process)
- [Development Process](#development-process)
- [Testing Process](#testing-process)
- [API Usage](#api-usage)
- [Deployment Notes](#deployment-notes)
- [Troubleshooting](#troubleshooting)
- [License](#license)

---

## Project Overview

This backend provides:
- Shortening of long URLs with custom or random shortcodes
- Expiry for links (default: 30 minutes, configurable)
- Click analytics (count, referrer, IP, timestamp)
- RESTful API endpoints
- Logging of all requests and events

---

## Folder Structure

```
backend-test-submission/
├── app.js                # Main server file
├── package.json          # Project metadata and scripts
├── controllers/
│   └── shorturl.js       # Main logic for short URLs
├── models/
│   └── ShortUrl.js       # Mongoose schema/model
├── routes/
│   └── shorturl.js       # API endpoints
├── test-api.js           # Automated API test script
├── services/             # (Reserved for future logic)
├── logging-middleware/
│   └── log.js            # Request logging middleware
├── .gitignore            # node_modules and .env are not tracked
└── README.md             # This file
```

---

## Setup Process

1. **Clone the repository:**
   ```bash
   git clone <your-repo-url>
   cd backend-test-submission
   ```

2. **Install dependencies:**
   ```bash
   npm install
   ```

3. **Configure environment variables:**
   - Create a `.env` file in the project root:
     ```
     MONGO_URI=mongodb://localhost:27017/urlshortener
     PORT=3000
     ```
   - If using MongoDB Atlas, use your connection string for `MONGO_URI`.

4. **Start MongoDB:**
   - Make sure your MongoDB server is running locally or your cloud instance is accessible.

5. **Start the server:**
   ```bash
   npm start
   ```
   - The server will run on `http://localhost:3000` (or the next available port).

---

## Development Process

- **Main logic is in `controllers/shorturl.js`**
- **API routes are defined in `routes/shorturl.js`**
- **Mongoose schema is in `models/ShortUrl.js`**
- **Logging is handled by `logging-middleware/log.js`**
- **All requests and important events are logged to `../events.log`**
- **Sensitive files like `node_modules` and `.env` are excluded from git (see `.gitignore`)**

If you want to add features, create new controllers/services and wire them up in `app.js` and the routes folder.

---

## Testing Process

1. **Automated API Testing:**
   - The `test-api.js` script covers all main features: creation, auto-shortcode, stats, redirect, invalid input, duplicates, non-existent codes, and click tracking.
   - To run tests:
     ```bash
     npm test
     ```
   - Make sure the server is running before running tests.

2. **Manual Testing (with Postman/Insomnia):**
   - **Create a short URL:**
     - POST `http://localhost:3000/shorturls`
     - Body (JSON):
       ```json
       {
         "original": "https://www.google.com",
         "shortcode": "google",
         "expiry": "2025-12-31T23:59:59.000Z"
       }
       ```
   - **Get stats:**
     - GET `http://localhost:3000/shorturls/google`
   - **Redirect:**
     - GET `http://localhost:3000/google`

---

## API Usage

### Create a Short URL
- **POST** `/shorturls`
- **Body Example:**
  ```json
  {
    "original": "https://example.com",
    "expiry": "2025-12-31T23:59:59.000Z", // optional
    "shortcode": "custom123"              // optional
  }
  ```

### Get Short URL Stats
- **GET** `/shorturls/:shortcode`

### Redirect to Original URL
- **GET** `/:shortcode`

---

## Deployment Notes
- For production, set your environment variables securely.
- Use a process manager like PM2 or Docker for reliability.
- Make sure your MongoDB instance is secured and backed up.
- Monitor logs for errors and usage patterns.

---

## Troubleshooting
- If you see `Server Health Check Failed`, make sure your server is running and accessible.
- If port 3000 is busy, the server will try the next available port and print it in the console.
- If you get MongoDB connection errors, check your `MONGO_URI` and MongoDB server status.
- For any issues, check `../events.log` for detailed logs.

---

## License
MIT
