---
title: "Create Movie"
api: "POST http://localhost:5065/api/movies"
description: "Creates a new movie entry in the database. The ID will be automatically generated."
---

Create a new movie in the database. All fields are required.

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
