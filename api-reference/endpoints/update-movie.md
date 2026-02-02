---
title: "Update Movie"
api: "PUT http://localhost:5065/api/movies/{id}"
description: "Updates all fields of an existing movie identified by ID. Provide the complete movie object in the request body."
---

Update an existing movie. This is a full update (PUT) - all fields must be provided.

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
