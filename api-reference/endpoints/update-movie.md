---
title: "Update Movie"
openapi: "PUT /posts/{id}"
description: "Updates an existing movie's information."
---

## Overview

Update all information for an existing movie in the database. This is a full update (PUT operation) where all fields must be provided, even if only some fields are being changed.

<Warning>
  This is a full update - all fields are required. Any field not provided will result in a validation error.
</Warning>

<ParamField path="id" type="integer" required placeholder="1">
  Movie ID to update (minimum: 1)
</ParamField>

<ParamField body="title" type="string" required default="The Shawshank Redemption (Updated)">
  Movie title (1-200 characters)
</ParamField>

<ParamField body="director" type="string" required default="Frank Darabont">
  Director name (1-100 characters)
</ParamField>

<ParamField body="year" type="integer" required default="1994">
  Release year (1888-2100)
</ParamField>

<ParamField body="genre" type="string" required default="Drama">
  Movie genre (1-50 characters)
</ParamField>

<ParamField body="rating" type="number" required default="9.4">
  Rating score (0.0-10.0)
</ParamField>

## Response Example

Status Code: `200 OK`

```json
{
  "id": 1,
  "title": "The Shawshank Redemption (Updated)",
  "director": "Frank Darabont",
  "year": 1994,
  "genre": "Drama",
  "rating": 9.4
}
```

---

Status Code: `400 Bad Request`

```json
{
  "message": "Invalid movie data",
  "statusCode": 400
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
  "title": "The Shawshank Redemption (Updated)",
  "director": "Frank Darabont",
  "year": 1994,
  "genre": "Drama",
  "rating": 9.4
}
```

```json 400 Bad Request
{
  "message": "Invalid movie data",
  "statusCode": 400
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

• Correct incorrect movie information
• Update movie ratings based on new reviews
• Modify movie metadata after release
• Implement administrative editing features

## Code Examples

### C#

```csharp
using var client = new MovieApiClient("https://tech-fwd-poc-production.up.railway.app");

int movieId = 1;

var updatedMovie = new Movie
{
    Title = "The Shawshank Redemption",
    Director = "Frank Darabont",
    Year = 1994,
    Genre = "Drama",
    Rating = 9.4  // Updated rating
};

try
{
    var result = await client.UpdateMovieAsync(movieId, updatedMovie);

    Console.WriteLine("Movie updated successfully:");
    Console.WriteLine($"ID: {result.Id}");
    Console.WriteLine($"Title: {result.Title}");
    Console.WriteLine($"New Rating: {result.Rating}/10");
}
catch (MovieNotFoundException ex)
{
    Console.WriteLine($"Movie not found: {ex.Message}");
}
catch (ValidationException ex)
{
    Console.WriteLine($"Validation error: {ex.Message}");
}
catch (Exception ex)
{
    Console.WriteLine($"Error updating movie: {ex.Message}");
}
```

### JavaScript

```javascript
const movieId = 1;

const updatedMovie = {
  title: "The Shawshank Redemption",
  director: "Frank Darabont",
  year: 1994,
  genre: "Drama",
  rating: 9.4, // Updated rating
};

const response = await fetch(
  `https://tech-fwd-poc-production.up.railway.app/api/movies/${movieId}`,
  {
    method: "PUT",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify(updatedMovie),
  },
);

if (response.ok) {
  const result = await response.json();
  console.log("Movie updated successfully:");
  console.log(`${result.title} - New Rating: ${result.rating}/10`);
} else if (response.status === 404) {
  console.error("Movie not found");
} else if (response.status === 400) {
  const error = await response.json();
  console.error("Validation error:", error.message);
} else {
  console.error("Error updating movie");
}
```

### Python

```python
import requests

movie_id = 1

updated_movie = {
    "title": "The Shawshank Redemption",
    "director": "Frank Darabont",
    "year": 1994,
    "genre": "Drama",
    "rating": 9.4  # Updated rating
}

response = requests.put(
    f"https://tech-fwd-poc-production.up.railway.app/api/movies/{movie_id}",
    json=updated_movie,
    headers={"Content-Type": "application/json"}
)

if response.status_code == 200:
    result = response.json()
    print("Movie updated successfully:")
    print(f"{result['title']} - New Rating: {result['rating']}/10")
elif response.status_code == 404:
    print("Movie not found")
elif response.status_code == 400:
    error = response.json()
    print(f"Validation error: {error['message']}")
else:
    print(f"Error: {response.status_code}")
```

## Related Endpoints

• [Get Movie by ID](/api-reference/endpoints/get-movie-by-id) - Retrieve current movie data before updating
• [Get All Movies](/api-reference/endpoints/get-all-movies) - View all movies in the database
• [Delete Movie](/api-reference/endpoints/delete-movie) - Remove movie instead of updating
