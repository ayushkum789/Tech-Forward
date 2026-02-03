---
title: "Get All Movies"
openapi: "openapi.json GET /api/movies"
description: "Returns a list of all movies from the database."
---

## Overview

Retrieve a complete list of all movies in the database. This endpoint returns an array of movie objects with their full details including ID, title, director, release year, genre, and rating.

## Response Example

Status Code: `200 OK`

```json
[
  {
    "id": 1,
    "title": "The Shawshank Redemption",
    "director": "Frank Darabont",
    "year": 1994,
    "genre": "Drama",
    "rating": 9.3
  },
  {
    "id": 2,
    "title": "The Godfather",
    "director": "Francis Ford Coppola",
    "year": 1972,
    "genre": "Crime",
    "rating": 9.2
  },
  {
    "id": 3,
    "title": "The Dark Knight",
    "director": "Christopher Nolan",
    "year": 2008,
    "genre": "Action",
    "rating": 9.0
  }
]
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
[
  {
    "id": 1,
    "title": "The Shawshank Redemption",
    "director": "Frank Darabont",
    "year": 1994,
    "genre": "Drama",
    "rating": 9.3
  },
  {
    "id": 2,
    "title": "The Godfather",
    "director": "Francis Ford Coppola",
    "year": 1972,
    "genre": "Crime",
    "rating": 9.2
  },
  {
    "id": 3,
    "title": "The Dark Knight",
    "director": "Christopher Nolan",
    "year": 2008,
    "genre": "Action",
    "rating": 9.0
  }
]
```

```json 500 Internal Server Error
{
  "message": "An internal server error occurred",
  "statusCode": 500
}
```

</ResponseExample>

## Use Cases

• Display all movies in your application
• Build a movie catalog or library
• Export movie data for reporting
• Populate dropdown lists or selection menus

## Code Examples

### C#

```csharp
using var client = new MovieApiClient("https://tech-fwd-poc-production.up.railway.app");

var movies = await client.GetAllMoviesAsync();

Console.WriteLine("All Movies:");
foreach (var movie in movies ?? new List<Movie>())
{
    Console.WriteLine($"[{movie.Id}] {movie.Title} ({movie.Year}) - {movie.Director} - Rating: {movie.Rating}");
}
```

### JavaScript

```javascript
const response = await fetch(
  "https://tech-fwd-poc-production.up.railway.app/api/movies",
  {
    method: "GET",
    headers: {
      "Content-Type": "application/json",
    },
  },
);

const movies = await response.json();

movies.forEach((movie) => {
  console.log(`${movie.title} (${movie.year}) - ${movie.rating}/10`);
});
```

### Python

```python
import requests

response = requests.get(
    "https://tech-fwd-poc-production.up.railway.app/api/movies",
    headers={"Content-Type": "application/json"}
)

if response.status_code == 200:
    movies = response.json()

    for movie in movies:
        print(f"{movie['title']} ({movie['year']}) - {movie['rating']}/10")
else:
    print(f"Error: {response.status_code}")
```

## Related Endpoints

• [Get Movie by ID](/api-reference/endpoints/get-movie-by-id) - Retrieve specific movie details
• [Create Movie](/api-reference/endpoints/create-movie) - Add new movies to the list
• [Update Movie](/api-reference/endpoints/update-movie) - Modify existing movie information
