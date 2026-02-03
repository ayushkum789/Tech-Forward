---
title: "Create Movie"
openapi: "openapi.json POST /api/movies"
description: "Creates a new movie entry in the database."
---

## Overview

Add a new movie to the database with complete details including title, director, release year, genre, and rating. All fields are required to ensure data consistency.

<Note>
  All fields are required. The movie will be assigned a unique ID automatically upon creation.
</Note>

<ParamField body="title" type="string" required default="Inception">
  Movie title (1-200 characters)
</ParamField>

<ParamField body="director" type="string" required default="Christopher Nolan">
  Director name (1-100 characters)
</ParamField>

<ParamField body="year" type="integer" required default="2010">
  Release year (1888-2100)
</ParamField>

<ParamField body="genre" type="string" required default="Sci-Fi">
  Movie genre (1-50 characters)
</ParamField>

<ParamField body="rating" type="number" required default="8.8">
  Rating score (0.0-10.0)
</ParamField>

## Response Example

Status Code: `201 Created`

```json
{
  "id": 6,
  "title": "Inception",
  "director": "Christopher Nolan",
  "year": 2010,
  "genre": "Sci-Fi",
  "rating": 8.8
}
```

---

Status Code: `400 Bad Request`

```json
{
  "message": "Movie data is required",
  "statusCode": 400
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
```json 201 Created
{
  "id": 6,
  "title": "Inception",
  "director": "Christopher Nolan",
  "year": 2010,
  "genre": "Sci-Fi",
  "rating": 8.8
}
```

```json 400 Bad Request
{
  "message": "Movie data is required",
  "statusCode": 400
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

• Add newly released movies to your catalog
• Import movie data from external sources
• Allow users to contribute movie entries
• Build comprehensive movie databases

## Code Examples

### C#

```csharp
using var client = new MovieApiClient("https://tech-fwd-poc-production.up.railway.app");

var newMovie = new Movie
{
    Title = "Inception",
    Director = "Christopher Nolan",
    Year = 2010,
    Genre = "Sci-Fi",
    Rating = 8.8
};

try
{
    var createdMovie = await client.CreateMovieAsync(newMovie);
    Console.WriteLine($"Movie created with ID: {createdMovie.Id}");
    Console.WriteLine($"{createdMovie.Title} ({createdMovie.Year}) - {createdMovie.Director}");
}
catch (ValidationException ex)
{
    Console.WriteLine($"Validation error: {ex.Message}");
}
catch (Exception ex)
{
    Console.WriteLine($"Error creating movie: {ex.Message}");
}
```

### JavaScript

```javascript
const newMovie = {
  title: "Inception",
  director: "Christopher Nolan",
  year: 2010,
  genre: "Sci-Fi",
  rating: 8.8,
};

const response = await fetch(
  "https://tech-fwd-poc-production.up.railway.app/api/movies",
  {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify(newMovie),
  },
);

if (response.status === 201) {
  const createdMovie = await response.json();
  console.log(`Movie created with ID: ${createdMovie.id}`);
  console.log(
    `${createdMovie.title} (${createdMovie.year}) - ${createdMovie.director}`,
  );
} else {
  const error = await response.json();
  console.error("Error:", error.message);
}
```

### Python

```python
import requests

new_movie = {
    "title": "Inception",
    "director": "Christopher Nolan",
    "year": 2010,
    "genre": "Sci-Fi",
    "rating": 8.8
}

response = requests.post(
    "https://tech-fwd-poc-production.up.railway.app/api/movies",
    json=new_movie,
    headers={"Content-Type": "application/json"}
)

if response.status_code == 201:
    created_movie = response.json()
    print(f"Movie created with ID: {created_movie['id']}")
    print(f"{created_movie['title']} ({created_movie['year']}) - {created_movie['director']}")
else:
    error = response.json()
    print(f"Error: {error['message']}")
```

## Related Endpoints

• [Get All Movies](/api-reference/endpoints/get-all-movies) - View all movies including the newly created one
• [Get Movie by ID](/api-reference/endpoints/get-movie-by-id) - Retrieve the created movie details
• [Update Movie](/api-reference/endpoints/update-movie) - Modify movie information after creation
