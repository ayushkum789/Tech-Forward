---
title: "Delete Movie"
openapi: "openapi.json DELETE /api/movies/{id}"
description: "Permanently removes a movie from the database."
---

## Overview

Permanently remove a movie from the database by its unique ID. This operation is irreversible and will completely delete the movie record from the system.

<Warning>
  This operation is permanent and cannot be undone. Make sure you have the correct movie ID before proceeding.
</Warning>

<ParamField path="id" type="integer" required placeholder="5">
  Movie ID to delete (minimum: 1)
</ParamField>

## Response Example

Status Code: `204 No Content`

No content returned. A 204 status code indicates successful deletion.

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
```json 204 No Content
// No content returned. Status code 204 indicates successful deletion.
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

• Remove outdated or incorrect movie entries
• Clean up duplicate records
• Implement administrative deletion features
• Maintain database integrity by removing invalid data

## Code Examples

### C#

```csharp
using var client = new MovieApiClient("https://tech-fwd-poc-production.up.railway.app");

int movieIdToDelete = 5;

try
{
    await client.DeleteMovieAsync(movieIdToDelete);
    Console.WriteLine($"Movie with ID {movieIdToDelete} has been successfully deleted.");
}
catch (MovieNotFoundException ex)
{
    Console.WriteLine($"Error: Movie not found - {ex.Message}");
}
catch (Exception ex)
{
    Console.WriteLine($"Error deleting movie: {ex.Message}");
}
```

### JavaScript

```javascript
const movieId = 5;

const response = await fetch(
  `https://tech-fwd-poc-production.up.railway.app/api/movies/${movieId}`,
  {
    method: "DELETE",
    headers: {
      "Content-Type": "application/json",
    },
  },
);

if (response.status === 204) {
  console.log(`Movie with ID ${movieId} has been successfully deleted.`);
} else if (response.status === 404) {
  const error = await response.json();
  console.error("Movie not found:", error.message);
} else {
  console.error("Failed to delete movie");
}
```

### Python

```python
import requests

movie_id = 5

response = requests.delete(
    f"https://tech-fwd-poc-production.up.railway.app/api/movies/{movie_id}",
    headers={"Content-Type": "application/json"}
)

if response.status_code == 204:
    print(f"Movie with ID {movie_id} has been successfully deleted.")
elif response.status_code == 404:
    error = response.json()
    print(f"Error: Movie not found - {error['message']}")
else:
    print(f"Error deleting movie: {response.status_code}")
```

## Related Endpoints

• [Get Movie by ID](/api-reference/endpoints/get-movie-by-id) - Verify movie exists before deletion
• [Get All Movies](/api-reference/endpoints/get-all-movies) - View all available movies
• [Update Movie](/api-reference/endpoints/update-movie) - Modify movie instead of deleting
