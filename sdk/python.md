---
title: "Python SDK"
description: "Use the Movies API with Python"
---

# Python SDK

Python client library for the Movies API.

## Installation

```bash
pip install movies-api-client
```

## Quick Start

```python
from movies_api import MoviesClient

# Initialize client
client = MoviesClient(base_url="http://localhost:5065")

# Get all movies
movies = client.movies.get_all()

# Get movie by ID
movie = client.movies.get(1)

# Create a movie
new_movie = client.movies.create({
    "title": "The Matrix",
    "director": "Lana Wachowski",
    "year": 1999,
    "genre": "Sci-Fi",
    "rating": 8.7
})

# Update a movie
updated = client.movies.update(1, {
    "rating": 9.0
})

# Delete a movie
client.movies.delete(1)
```

## Configuration

```python
client = MoviesClient(
    base_url="http://localhost:5065",
    timeout=30,
    verify_ssl=True
)
```

## Error Handling

```python
from movies_api.exceptions import MovieNotFound, ValidationError

try:
    movie = client.movies.get(999)
except MovieNotFound:
    print("Movie not found")
except ValidationError as e:
    print(f"Validation error: {e}")
```

## Async Support

```python
from movies_api import AsyncMoviesClient

async def main():
    async with AsyncMoviesClient() as client:
        movies = await client.movies.get_all()
        print(movies)
```

## Type Hints

```python
from movies_api.types import Movie, CreateMovieInput

def process_movie(movie: Movie) -> None:
    print(f"Title: {movie.title}")
    print(f"Director: {movie.director}")
```
