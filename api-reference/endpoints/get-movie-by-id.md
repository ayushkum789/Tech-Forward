---
title: "Get Movie by ID"
openapi: "openapi.json GET /api/movies/{id}"
description: "Returns detailed information about a single movie."
---

## Overview

Retrieve complete details for a specific movie using its unique identifier. This endpoint returns all available information including title, director, release year, genre, and rating.

<ParamField path="id" type="integer" required placeholder="1">
  Unique movie identifier (minimum: 1)
</ParamField>

## Response Example

Status Code: `200 OK`

```json
{
  "id": 1,
  "title": "The Shawshank Redemption",
  "director": "Frank Darabont",
  "year": 1994,
  "genre": "Drama",
  "rating": 9.3
}
```

---

Status Code: `404 Not Found`

```json
{
  "message": "Movie with id 999 not found",
  "statusCode": 404
}
```

---

Status Code: `500 Internal Server Error`

```json
{
  "message": "An internal server error occurred",
  "statusCode": 500
}
```

<ResponseExample>
```json 200 Success
{
  "id": 1,
  "title": "The Shawshank Redemption",
  "director": "Frank Darabont",
  "year": 1994,
  "genre": "Drama",
  "rating": 9.3
}
```

```json 404 Not Found
{
  "message": "Movie with id 999 not found",
  "statusCode": 404
}
```

```json 500 Internal Server Error
{
  "message": "An internal server error occurred",
  "statusCode": 500
}
```

</ResponseExample>

## Use Cases

• Display detailed movie information on a details page
• Verify movie existence before performing updates or deletions
• Fetch specific movie data for user selections
• Show movie details in search results

## Code Examples

### C#

```csharp
using var client = new MovieApiClient("https://tech-fwd-poc-production.up.railway.app");

int movieId = 1;

try
{
    var movie = await client.GetMovieByIdAsync(movieId);

    Console.WriteLine("Movie Details:");
    Console.WriteLine($"ID: {movie.Id}");
    Console.WriteLine($"Title: {movie.Title}");
    Console.WriteLine($"Director: {movie.Director}");
    Console.WriteLine($"Year: {movie.Year}");
    Console.WriteLine($"Genre: {movie.Genre}");
    Console.WriteLine($"Rating: {movie.Rating}/10");
}
catch (MovieNotFoundException ex)
{
    Console.WriteLine($"Movie not found: {ex.Message}");
}
catch (Exception ex)
{
    Console.WriteLine($"Error retrieving movie: {ex.Message}");
}
```

### JavaScript

```javascript
const movieId = 1;

const response = await fetch(
  `https://tech-fwd-poc-production.up.railway.app/api/movies/${movieId}`,
  {
    method: "GET",
    headers: {
      "Content-Type": "application/json",
    },
  },
);

if (response.ok) {
  const movie = await response.json();

  console.log("Movie Details:");
  console.log(`Title: ${movie.title} (${movie.year})`);
  console.log(`Director: ${movie.director}`);
  console.log(`Genre: ${movie.genre}`);
  console.log(`Rating: ${movie.rating}/10`);
} else if (response.status === 404) {
  console.error("Movie not found");
} else {
  console.error("Error fetching movie");
}
```

### Python

```python
import requests

movie_id = 1

response = requests.get(
    f"https://tech-fwd-poc-production.up.railway.app/api/movies/{movie_id}",
    headers={"Content-Type": "application/json"}
)

if response.status_code == 200:
    movie = response.json()

    print("Movie Details:")
    print(f"Title: {movie['title']} ({movie['year']})")
    print(f"Director: {movie['director']}")
    print(f"Genre: {movie['genre']}")
    print(f"Rating: {movie['rating']}/10")
elif response.status_code == 404:
    print("Movie not found")
else:
    print(f"Error: {response.status_code}")
```

## Related Endpoints

• [Get All Movies](/api-reference/endpoints/get-all-movies) - Browse all available movies
• [Update Movie](/api-reference/endpoints/update-movie) - Modify this movie's information
• [Delete Movie](/api-reference/endpoints/delete-movie) - Remove this movie from the database
