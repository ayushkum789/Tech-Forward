---
title: "Error Reference"
description: "Complete guide to Movies API error codes and troubleshooting"
icon: "triangle-exclamation"
---

## Overview

The Movies API uses conventional HTTP response codes to indicate the success or failure of an API request. Errors are returned with JSON bodies containing detailed information about what went wrong.

## Error Response Format

All error responses follow this structure:

```json
{
  "success": false,
  "error": {
    "code": "ERROR_CODE",
    "message": "Human readable error message",
    "field": "fieldName" // Optional, for validation errors
  }
}
```

## HTTP Status Codes

| Code    | Name                  | Description                   |
| ------- | --------------------- | ----------------------------- |
| **200** | OK                    | Request successful            |
| **201** | Created               | Resource created successfully |
| **204** | No Content            | Request successful (no body)  |
| **400** | Bad Request           | Invalid request parameters    |
| **401** | Unauthorized          | Invalid or missing API key    |
| **404** | Not Found             | Resource does not exist       |
| **429** | Too Many Requests     | Rate limit exceeded           |
| **500** | Internal Server Error | Server-side error             |
| **503** | Service Unavailable   | Temporary service outage      |

## Common Error Codes

### UNAUTHORIZED (401)

**Description**: Invalid or missing API key.

**Response**:

```json
{
  "success": false,
  "error": {
    "code": "UNAUTHORIZED",
    "message": "Invalid or missing API key"
  }
}
```

**Causes**:

- API key not included in request
- API key is invalid or expired
- API key has been revoked
- Wrong authorization format

**Solutions**:
<AccordionGroup>
<Accordion title="Check Authorization Header" icon="key">
```bash # ✅ Correct
Authorization: Bearer sk_live_1234567890abcdef

    # ❌ Wrong - missing "Bearer"
    Authorization: sk_live_1234567890abcdef

    # ❌ Wrong - wrong header name
    X-API-Key: sk_live_1234567890abcdef
    ```

  </Accordion>
  
  <Accordion title="Verify API Key" icon="check">
    - Check your API key in the dashboard
    - Ensure you're using the correct environment (test vs live)
    - Verify the key hasn't been revoked
  </Accordion>
  
  <Accordion title="Environment Variables" icon="gear">
    ```javascript
    // Ensure environment variable is set
    console.log(process.env.MOVIES_API_KEY); // Should not be undefined
    
    const client = new MoviesClient(process.env.MOVIES_API_KEY);
    ```
  </Accordion>
</AccordionGroup>

---

### NOT_FOUND (404)

**Description**: The requested resource does not exist.

**Response**:

```json
{
  "success": false,
  "error": {
    "code": "NOT_FOUND",
    "message": "Movie with ID 999 not found"
  }
}
```

**Causes**:

- Movie ID doesn't exist in the database
- Movie was previously deleted
- Incorrect endpoint URL
- Typo in the movie ID

**Solutions**:
<AccordionGroup>
<Accordion title="Verify Resource Exists" icon="magnifying-glass">
`javascript
    // Check if movie exists before operations
    async function movieExists(movieId) {
      try {
        await client.movies.get(movieId);
        return true;
      } catch (error) {
        if (error.status === 404) {
          return false;
        }
        throw error;
      }
    }
    `
</Accordion>

  <Accordion title="Handle 404 Gracefully" icon="shield-check">
    ```javascript
    try {
      const movie = await client.movies.get(movieId);
    } catch (error) {
      if (error.status === 404) {
        console.log('Movie not found, showing default content');
        return defaultMovie;
      }
      throw error;
    }
    ```
  </Accordion>
</AccordionGroup>

---

### INVALID_PARAMETER (400)

**Description**: One or more request parameters are invalid.

**Response**:

```json
{
  "success": false,
  "error": {
    "code": "INVALID_PARAMETER",
    "message": "Invalid page number. Must be a positive integer.",
    "field": "page"
  }
}
```

**Common Validation Errors**:

| Field      | Error         | Solution                         |
| ---------- | ------------- | -------------------------------- |
| `title`    | Too long      | Max 200 characters               |
| `director` | Too long      | Max 100 characters               |
| `year`     | Out of range  | Must be 1900-2100                |
| `genre`    | Invalid value | Use valid genre (case-sensitive) |
| `rating`   | Out of range  | Must be 0.0-10.0                 |
| `page`     | Not a number  | Must be positive integer         |
| `limit`    | Too large     | Max 100 items per page           |

**Solutions**:
<AccordionGroup>
<Accordion title="Validate Input Client-Side" icon="check-circle">
```javascript
function validateMovieData(data) {
const errors = [];

      if (!data.title || data.title.length > 200) {
        errors.push('Title must be 1-200 characters');
      }

      if (!data.director || data.director.length > 100) {
        errors.push('Director must be 1-100 characters');
      }

      if (data.year < 1900 || data.year > 2100) {
        errors.push('Year must be between 1900 and 2100');
      }

      if (data.rating && (data.rating < 0 || data.rating > 10)) {
        errors.push('Rating must be between 0.0 and 10.0');
      }

      return errors;
    }
    ```

  </Accordion>
  
  <Accordion title="Use Correct Data Types" icon="code">
    ```javascript
    // ✅ Correct
    const movie = {
      title: "Inception",
      director: "Christopher Nolan",
      year: 2010,           // Number, not string
      genre: "Sci-Fi",      // Exact case
      rating: 8.8           // Float, not string
    };
    
    // ❌ Wrong
    const movie = {
      title: "Inception",
      director: "Christopher Nolan",
      year: "2010",         // Should be number
      genre: "sci-fi",      // Wrong case
      rating: "8.8"         // Should be number
    };
    ```
  </Accordion>
</AccordionGroup>

---

### MISSING_FIELD (400)

**Description**: A required field is missing from the request.

**Response**:

```json
{
  "success": false,
  "error": {
    "code": "MISSING_FIELD",
    "message": "Missing required field: title",
    "field": "title"
  }
}
```

**Required Fields for POST /movies**:

- `title` (string)
- `director` (string)
- `year` (integer)
- `genre` (string)

**Solutions**:
<AccordionGroup>
<Accordion title="Check Required Fields" icon="list-check">
```javascript
// Ensure all required fields are present
const requiredFields = ['title', 'director', 'year', 'genre'];

    function validateRequiredFields(data) {
      const missing = requiredFields.filter(field => !data[field]);

      if (missing.length > 0) {
        throw new Error(`Missing required fields: ${missing.join(', ')}`);
      }
    }
    ```

  </Accordion>
</AccordionGroup>

---

### INVALID_GENRE (400)

**Description**: The provided genre is not valid.

**Response**:

```json
{
  "success": false,
  "error": {
    "code": "INVALID_GENRE",
    "message": "Invalid genre. Must be one of: Action, Adventure, ...",
    "field": "genre"
  }
}
```

**Valid Genres** (case-sensitive):

- Action
- Adventure
- Animation
- Biography
- Comedy
- Crime
- Documentary
- Drama
- Fantasy
- Horror
- Musical
- Mystery
- Romance
- Sci-Fi
- Thriller
- Western

**Solutions**:
<AccordionGroup>
<Accordion title="Use Exact Genre Names" icon="tag">
```javascript
// ✅ Correct
const genres = {
sciFi: 'Sci-Fi', // Note the hyphen and capital F
drama: 'Drama',
action: 'Action'
};

    // ❌ Wrong
    const genres = {
      sciFi: 'SciFi',       // Missing hyphen
      drama: 'drama',       // Wrong case
      action: 'ACTION'      // Wrong case
    };
    ```

  </Accordion>
  
  <Accordion title="Validate Genre Before Request" icon="check">
    ```javascript
    const validGenres = [
      'Action', 'Adventure', 'Animation', 'Biography',
      'Comedy', 'Crime', 'Documentary', 'Drama',
      'Fantasy', 'Horror', 'Musical', 'Mystery',
      'Romance', 'Sci-Fi', 'Thriller', 'Western'
    ];
    
    function isValidGenre(genre) {
      return validGenres.includes(genre);
    }
    ```
  </Accordion>
</AccordionGroup>

---

### RATE_LIMIT_EXCEEDED (429)

**Description**: Too many requests have been made in a short period.

**Response**:

```json
{
  "success": false,
  "error": {
    "code": "RATE_LIMIT_EXCEEDED",
    "message": "Rate limit exceeded. Try again in 3600 seconds.",
    "retryAfter": 3600
  }
}
```

**Rate Limits by Plan**:
| Plan | Limit | Burst |
|------|-------|-------|
| Free | 1,000/hour | 100/minute |
| Pro | 10,000/hour | 500/minute |
| Enterprise | Unlimited | Unlimited |

**Solutions**:
<AccordionGroup>
<Accordion title="Implement Exponential Backoff" icon="clock">
```javascript
async function withRetry(fn, maxRetries = 3) {
for (let i = 0; i < maxRetries; i++) {
try {
return await fn();
} catch (error) {
if (error.code === 'RATE_LIMIT_EXCEEDED') {
const delay = error.retryAfter
? error.retryAfter _ 1000
: 1000 _ Math.pow(2, i);

            console.log(`Rate limited, waiting ${delay}ms`);
            await new Promise(resolve => setTimeout(resolve, delay));
            continue;
          }
          throw error;
        }
      }
    }
    ```

  </Accordion>
  
  <Accordion title="Add Delays Between Requests" icon="hourglass">
    ```javascript
    // For bulk operations, add delays
    for (const item of items) {
      await client.movies.create(item);
      await new Promise(resolve => setTimeout(resolve, 100)); // 100ms delay
    }
    ```
  </Accordion>
  
  <Accordion title="Monitor Rate Limit Headers" icon="gauge">
    ```javascript
    const response = await fetch('https://api.moviesapi.com/movies', {
      headers: { 'Authorization': `Bearer ${apiKey}` }
    });
    
    // Check rate limit headers
    const limit = response.headers.get('X-RateLimit-Limit');
    const remaining = response.headers.get('X-RateLimit-Remaining');
    const reset = response.headers.get('X-RateLimit-Reset');
    
    console.log(`${remaining}/${limit} requests remaining`);
    
    if (remaining < 10) {
      console.warn('Approaching rate limit!');
    }
    ```
  </Accordion>
</AccordionGroup>

---

### INTERNAL_SERVER_ERROR (500)

**Description**: An unexpected error occurred on the server.

**Response**:

```json
{
  "success": false,
  "error": {
    "code": "INTERNAL_SERVER_ERROR",
    "message": "An unexpected error occurred. Please try again later."
  }
}
```

**Causes**:

- Temporary server issue
- Database connection problem
- Unexpected server condition

**Solutions**:
<AccordionGroup>
<Accordion title="Retry the Request" icon="rotate">
`javascript
    // Implement retry logic for 500 errors
    async function retryOn500(fn, maxRetries = 3) {
      for (let i = 0; i < maxRetries; i++) {
        try {
          return await fn();
        } catch (error) {
          if (error.status === 500 && i < maxRetries - 1) {
            await new Promise(resolve => setTimeout(resolve, 1000 * (i + 1)));
            continue;
          }
          throw error;
        }
      }
    }
    `
</Accordion>

  <Accordion title="Contact Support" icon="headset">
    If errors persist:
    - Note the timestamp of the error
    - Save the request details
    - Contact support@moviesapi.com
    - Check status page: status.moviesapi.com
  </Accordion>
</AccordionGroup>

## Error Handling Best Practices

### 1. Always Handle Errors

```javascript
// ✅ Good
try {
  const movie = await client.movies.get(id);
  displayMovie(movie);
} catch (error) {
  if (error.status === 404) {
    showNotFoundMessage();
  } else {
    showErrorMessage(error.message);
  }
}

// ❌ Bad
const movie = await client.movies.get(id); // Unhandled promise rejection
```

### 2. Provide User-Friendly Messages

```javascript
function getUserFriendlyError(error) {
  const messages = {
    UNAUTHORIZED: "Please check your API credentials",
    NOT_FOUND: "The requested movie could not be found",
    RATE_LIMIT_EXCEEDED: "Too many requests. Please try again later",
    INVALID_PARAMETER: `Invalid input: ${error.message}`,
  };

  return messages[error.code] || "An error occurred. Please try again";
}
```

### 3. Log Errors for Debugging

```javascript
function logError(error, context) {
  console.error("API Error:", {
    code: error.code,
    message: error.message,
    status: error.status,
    field: error.field,
    context: context,
    timestamp: new Date().toISOString(),
  });
}
```

### 4. Implement Retry Logic

```javascript
async function fetchWithRetry(fn, options = {}) {
  const maxRetries = options.maxRetries || 3;
  const retryDelay = options.retryDelay || 1000;
  const retryOn = options.retryOn || [500, 502, 503, 504];

  for (let i = 0; i < maxRetries; i++) {
    try {
      return await fn();
    } catch (error) {
      const shouldRetry = retryOn.includes(error.status) && i < maxRetries - 1;

      if (shouldRetry) {
        await new Promise((resolve) =>
          setTimeout(resolve, retryDelay * (i + 1)),
        );
        continue;
      }

      throw error;
    }
  }
}
```

## Debugging Tips

<Tip>
  **Enable Debug Mode**: Set `DEBUG=true` in your environment to see detailed request/response logs.
</Tip>

<Tip>
  **Check Network Tab**: Use browser DevTools Network tab to inspect actual requests and responses.
</Tip>

<Tip>
  **Validate JSON**: Ensure your request body is valid JSON using tools like jsonlint.com.
</Tip>

<Tip>
  **Test with cURL**: Isolate issues by testing with cURL commands first.
</Tip>

## Status Page

Check the API status in real-time:

<Card title="API Status Page" icon="signal" href="https://status.moviesapi.com">
  View current API status, uptime, and incident history
</Card>

## Need More Help?

<CardGroup cols={2}>
  <Card title="Troubleshooting Guide" icon="wrench" href="/support/troubleshooting">
    Common issues and solutions
  </Card>
  <Card title="FAQ" icon="question" href="/support/faq">
    Frequently asked questions
  </Card>
  <Card title="Contact Support" icon="envelope" href="mailto:support@moviesapi.com">
    Get help from our team
  </Card>
  <Card title="Community" icon="users" href="https://community.moviesapi.com">
    Ask the community
  </Card>
</CardGroup>
