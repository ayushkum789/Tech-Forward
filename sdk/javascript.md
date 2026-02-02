---
title: "JavaScript SDK"
description: "Official JavaScript/TypeScript SDK for Movies API"
icon: "js"
---

## Installation

Install via npm or yarn:

<CodeGroup>

```bash npm
npm install @moviesapi/sdk
```

```bash yarn
yarn add @moviesapi/sdk
```

```bash pnpm
pnpm add @moviesapi/sdk
```

</CodeGroup>

## Requirements

- Node.js 14.0.0 or higher
- Browser support: Modern browsers (ES2018+)

## Quick Start

```javascript
import { MoviesClient } from "@moviesapi/sdk";

// Initialize client
const client = new MoviesClient(process.env.MOVIES_API_KEY);

// Use the client
const movies = await client.movies.getAll();
console.log(movies);
```

## Client Configuration

### Basic Configuration

```javascript
import { MoviesClient } from "@moviesapi/sdk";

const client = new MoviesClient("YOUR_API_KEY");
```

### Advanced Configuration

```javascript
const client = new MoviesClient("YOUR_API_KEY", {
  baseUrl: "https://api.moviesapi.com",
  timeout: 30000, // 30 seconds
  retries: 3,
  retryDelay: 1000, // 1 second
  headers: {
    "X-Custom-Header": "value",
  },
});
```

### Configuration Options

| Option       | Type   | Default                     | Description                     |
| ------------ | ------ | --------------------------- | ------------------------------- |
| `baseUrl`    | string | `https://api.moviesapi.com` | API base URL                    |
| `timeout`    | number | `30000`                     | Request timeout in milliseconds |
| `retries`    | number | `3`                         | Number of retry attempts        |
| `retryDelay` | number | `1000`                      | Initial retry delay in ms       |
| `headers`    | object | `{}`                        | Additional headers              |

## Get All Movies

Retrieve a list of all movies with optional filtering and pagination.

```javascript
// Get all movies (default pagination)
const response = await client.movies.getAll();
console.log(response.data); // Array of movies
console.log(response.pagination); // Pagination info

// With pagination
const page1 = await client.movies.getAll({
  page: 1,
  limit: 20,
});

// With filters
const dramaMovies = await client.movies.getAll({
  genre: "Drama",
  minRating: 8.0,
  sort: "-rating", // Sort by rating descending
});

// Multiple filters
const filtered = await client.movies.getAll({
  genre: "Sci-Fi",
  year: 2010,
  director: "Nolan",
  minRating: 8.0,
  maxRating: 9.0,
  page: 1,
  limit: 10,
  sort: "-year,title",
});
```

### Response Type

```typescript
interface MoviesResponse {
  success: boolean;
  data: Movie[];
  pagination: {
    page: number;
    limit: number;
    total: number;
    pages: number;
    hasNext: boolean;
    hasPrev: boolean;
  };
}
```

## Get Movie by ID

Retrieve a specific movie by its ID.

```javascript
// Get movie by ID
const movie = await client.movies.get(1);
console.log(movie.title); // "The Shawshank Redemption"

// With error handling
try {
  const movie = await client.movies.get(999);
} catch (error) {
  if (error.status === 404) {
    console.error("Movie not found");
  }
}
```

### Response Type

```typescript
interface MovieResponse {
  success: boolean;
  data: Movie;
}

interface Movie {
  id: number;
  title: string;
  director: string;
  year: number;
  genre: string;
  rating?: number;
  createdAt: string;
  updatedAt: string;
}
```

## Create Movie

Add a new movie to the database.

```javascript
// Create a new movie
const newMovie = await client.movies.create({
  title: "Inception",
  director: "Christopher Nolan",
  year: 2010,
  genre: "Sci-Fi",
  rating: 8.8,
});

console.log(newMovie.id); // Auto-generated ID
console.log(newMovie.createdAt); // Timestamp

// Without rating (optional field)
const movie = await client.movies.create({
  title: "Oppenheimer",
  director: "Christopher Nolan",
  year: 2023,
  genre: "Biography",
});
```

### Input Type

```typescript
interface CreateMovieInput {
  title: string; // Required, 1-200 chars
  director: string; // Required, 1-100 chars
  year: number; // Required, 1900-2100
  genre: string; // Required, valid genre
  rating?: number; // Optional, 0.0-10.0
}
```

### Valid Genres

```javascript
const validGenres = [
  "Action",
  "Adventure",
  "Animation",
  "Biography",
  "Comedy",
  "Crime",
  "Documentary",
  "Drama",
  "Fantasy",
  "Horror",
  "Musical",
  "Mystery",
  "Romance",
  "Sci-Fi",
  "Thriller",
  "Western",
];
```

## Update Movie

Update an existing movie's information.

```javascript
// Update single field
const updated = await client.movies.update(3, {
  rating: 9.0,
});

// Update multiple fields
const updated = await client.movies.update(3, {
  title: "Inception: Director's Cut",
  rating: 9.0,
  year: 2010,
});

// Partial update
const updated = await client.movies.update(3, {
  rating: 8.9,
  // Other fields remain unchanged
});
```

### Input Type

```typescript
interface UpdateMovieInput {
  title?: string;
  director?: string;
  year?: number;
  genre?: string;
  rating?: number;
}
```

## Delete Movie

Permanently delete a movie from the database.

```javascript
// Delete movie
await client.movies.delete(3);
console.log("Movie deleted successfully");

// With confirmation
const movie = await client.movies.get(3);
const confirmed = confirm(`Delete "${movie.title}"?`);

if (confirmed) {
  await client.movies.delete(3);
}

// With error handling
try {
  await client.movies.delete(999);
} catch (error) {
  if (error.status === 404) {
    console.log("Movie already deleted or never existed");
  }
}
```

## Pagination Helper

Iterate through all pages automatically:

```javascript
// Manual pagination
let page = 1;
let hasMore = true;

while (hasMore) {
  const response = await client.movies.getAll({ page, limit: 20 });

  for (const movie of response.data) {
    console.log(movie.title);
  }

  hasMore = response.pagination.hasNext;
  page++;
}

// Using async iterator (if supported)
for await (const movie of client.movies.iterate()) {
  console.log(movie.title);
}
```

## Error Handling

The SDK provides typed errors for better error handling:

```javascript
import { MoviesApiError } from "@moviesapi/sdk";

try {
  const movie = await client.movies.get(999);
} catch (error) {
  if (error instanceof MoviesApiError) {
    console.error("Error Code:", error.code);
    console.error("Message:", error.message);
    console.error("Status:", error.status);
    console.error("Field:", error.field); // For validation errors

    // Handle specific errors
    switch (error.code) {
      case "NOT_FOUND":
        console.log("Movie does not exist");
        break;
      case "UNAUTHORIZED":
        console.log("Invalid API key");
        break;
      case "RATE_LIMIT_EXCEEDED":
        console.log("Rate limit hit, retry after:", error.retryAfter);
        break;
      case "INVALID_PARAMETER":
        console.log(`Invalid ${error.field}:`, error.message);
        break;
      default:
        console.error("Unknown error:", error.message);
    }
  } else {
    // Network or other errors
    console.error("Request failed:", error.message);
  }
}
```

### Error Types

```typescript
interface MoviesApiError extends Error {
  code: string;
  message: string;
  status: number;
  field?: string;
  retryAfter?: number;
}
```

## TypeScript Support

The SDK is written in TypeScript and provides full type definitions:

```typescript
import { MoviesClient, Movie, CreateMovieInput } from "@moviesapi/sdk";

const client = new MoviesClient(process.env.MOVIES_API_KEY!);

// Type-safe movie creation
const movieData: CreateMovieInput = {
  title: "Inception",
  director: "Christopher Nolan",
  year: 2010,
  genre: "Sci-Fi",
  rating: 8.8,
};

const movie: Movie = await client.movies.create(movieData);

// TypeScript will catch errors
const invalid = await client.movies.create({
  title: "Test",
  director: "Test",
  year: "invalid", // ❌ Type error: year must be number
});
```

## Browser Usage

The SDK works in browsers with modern JavaScript support:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Movies API Demo</title>
  </head>
  <body>
    <script type="module">
      import { MoviesClient } from "https://cdn.skypack.dev/@moviesapi/sdk";

      const client = new MoviesClient("YOUR_API_KEY");

      async function loadMovies() {
        const response = await client.movies.getAll();
        const moviesList = document.getElementById("movies");

        response.data.forEach((movie) => {
          const div = document.createElement("div");
          div.textContent = `${movie.title} (${movie.year}) - ${movie.rating}/10`;
          moviesList.appendChild(div);
        });
      }

      loadMovies();
    </script>

    <div id="movies"></div>
  </body>
</html>
```

## Advanced Examples

### Retry Logic

```javascript
async function createMovieWithRetry(movieData, maxRetries = 3) {
  let lastError;

  for (let i = 0; i < maxRetries; i++) {
    try {
      return await client.movies.create(movieData);
    } catch (error) {
      lastError = error;

      // Don't retry on validation errors
      if (error.status === 400) {
        throw error;
      }

      // Wait before retrying
      await new Promise((resolve) => setTimeout(resolve, 1000 * (i + 1)));
    }
  }

  throw lastError;
}
```

### Bulk Operations

```javascript
// Bulk create movies
async function bulkCreateMovies(moviesData) {
  const results = [];

  for (const movieData of moviesData) {
    try {
      const movie = await client.movies.create(movieData);
      results.push({ success: true, movie });
    } catch (error) {
      results.push({ success: false, error: error.message, data: movieData });
    }

    // Rate limiting: wait 100ms between requests
    await new Promise((resolve) => setTimeout(resolve, 100));
  }

  return results;
}

const movies = [
  { title: "Movie 1", director: "Director 1", year: 2020, genre: "Drama" },
  { title: "Movie 2", director: "Director 2", year: 2021, genre: "Action" },
];

const results = await bulkCreateMovies(movies);
```

### Search and Filter

```javascript
// Complex search
async function searchMovies(query) {
  const results = [];
  let page = 1;

  while (true) {
    const response = await client.movies.getAll({
      page,
      limit: 100,
      director: query,
      sort: "-rating",
    });

    results.push(...response.data);

    if (!response.pagination.hasNext) break;
    page++;
  }

  return results;
}

const nolanMovies = await searchMovies("Nolan");
```

### Caching

```javascript
class CachedMoviesClient {
  constructor(apiKey) {
    this.client = new MoviesClient(apiKey);
    this.cache = new Map();
    this.cacheDuration = 5 * 60 * 1000; // 5 minutes
  }

  async getMovie(id) {
    const cacheKey = `movie_${id}`;
    const cached = this.cache.get(cacheKey);

    if (cached && Date.now() - cached.timestamp < this.cacheDuration) {
      return cached.data;
    }

    const movie = await this.client.movies.get(id);

    this.cache.set(cacheKey, {
      data: movie,
      timestamp: Date.now(),
    });

    return movie;
  }
}
```

## Best Practices

<AccordionGroup>
  <Accordion title="Use Environment Variables" icon="key">
    ```javascript
    // ✅ Good
    const client = new MoviesClient(process.env.MOVIES_API_KEY);
    
    // ❌ Bad
    const client = new MoviesClient('sk_live_1234...');
    ```
  </Accordion>
  
  <Accordion title="Handle Errors Properly" icon="triangle-exclamation">
    ```javascript
    try {
      const movie = await client.movies.get(id);
    } catch (error) {
      if (error instanceof MoviesApiError) {
        // Handle API errors
      } else {
        // Handle network errors
      }
    }
    ```
  </Accordion>
  
  <Accordion title="Respect Rate Limits" icon="gauge">
    ```javascript
    // Add delays between bulk operations
    for (const data of bulkData) {
      await client.movies.create(data);
      await new Promise(resolve => setTimeout(resolve, 100));
    }
    ```
  </Accordion>
  
  <Accordion title="Use TypeScript" icon="code">
    ```typescript
    // Get full type safety and autocomplete
    import { MoviesClient, Movie } from '@moviesapi/sdk';
    ```
  </Accordion>
</AccordionGroup>

## Troubleshooting

### Common Issues

<AccordionGroup>
  <Accordion title="CORS Errors in Browser" icon="globe">
    CORS is enabled for all origins in development. For production, configure allowed origins in your dashboard.
    
    ```javascript
    // Ensure you're using HTTPS
    const client = new MoviesClient('YOUR_API_KEY', {
      baseUrl: 'https://api.moviesapi.com' // Not http://
    });
    ```
  </Accordion>
  
  <Accordion title="Timeout Errors" icon="clock">
    ```javascript
    // Increase timeout for slow connections
    const client = new MoviesClient('YOUR_API_KEY', {
      timeout: 60000 // 60 seconds
    });
    ```
  </Accordion>
  
  <Accordion title="Rate Limit Errors" icon="ban">
    ```javascript
    // Implement exponential backoff
    async function withBackoff(fn, maxRetries = 3) {
      for (let i = 0; i < maxRetries; i++) {
        try {
          return await fn();
        } catch (error) {
          if (error.code === 'RATE_LIMIT_EXCEEDED') {
            const delay = error.retryAfter * 1000 || 1000 * Math.pow(2, i);
            await new Promise(resolve => setTimeout(resolve, delay));
            continue;
          }
          throw error;
        }
      }
    }
    ```
  </Accordion>
</AccordionGroup>

## Next Steps

<CardGroup cols={2}>
  <Card title="API Reference" icon="book" href="/api-reference/introduction">
    Explore all available endpoints
  </Card>
  <Card title="C# SDK" icon="microsoft" href="/sdk/csharp">
    Check out the C# SDK
  </Card>
  <Card title="Python SDK" icon="python" href="/sdk/python">
    Check out the Python SDK
  </Card>
  <Card title="GitHub" icon="github" href="https://github.com/moviesapi/sdk-javascript">
    View source code
  </Card>
</CardGroup>
