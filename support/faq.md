---
title: "FAQ"
description: "Frequently asked questions"
---

# Frequently Asked Questions

## General

### What is the Movies API?

The Movies API is a RESTful API for managing movie data with full CRUD operations.

### Is authentication required?

Currently, the development environment doesn't require authentication. Production will support API keys.

### What's the rate limit?

Development: Unlimited
Production: 1000 requests/hour per API key

## API Usage

### What response format is used?

All responses are in JSON format with a consistent structure:

```json
{
  "success": true,
  "message": "Operation successful",
  "data": {},
  "errors": null
}
```

### How do I paginate results?

Use `page` and `limit` query parameters:

```
GET /movies?page=1&limit=20
```

### Can I filter movies?

Yes, use query parameters:

- `genre`: Filter by genre
- `year`: Filter by year
- `director`: Filter by director
- `minRating`: Minimum rating

## Data Operations

### What movie genres are supported?

Action, Adventure, Animation, Biography, Comedy, Crime, Documentary, Drama, Fantasy, Horror, Musical, Mystery, Romance, Sci-Fi, Thriller, Western

### What's the valid year range?

Movies must have years between 1900 and 2100.

### What's the rating scale?

Ratings are from 0.0 to 10.0

### Can I update partial movie data?

Yes, PUT requests support partial updates. Only include the fields you want to change.

## SDKs & Integration

### Which programming languages are supported?

Official SDKs:

- JavaScript/TypeScript
- C# / .NET

Community SDKs:

- Python

### Do SDKs support TypeScript?

Yes, JavaScript SDK includes full TypeScript definitions.

### Can I use the API in the browser?

Yes, but ensure CORS is properly configured on the backend.

## Technical

### What HTTP methods are supported?

- GET: Retrieve data
- POST: Create new movies
- PUT: Update existing movies
- DELETE: Remove movies

### Are there any size limits?

- Request body: 1MB max
- Movie title: 200 characters
- Director name: 100 characters

### How do I handle errors?

Check the `success` field and `errors` array in the response. See [Error Reference](/support/errors) for details.

## Deployment

### Where can I deploy the documentation?

- Mintlify Cloud (recommended)
- Vercel
- Netlify
- GitHub Pages

### Can I customize the documentation?

Yes, edit `mint.json` and markdown files to customize branding, colors, and content.

## Support

### How do I report bugs?

Contact support with:

- Error message
- Request details
- Expected vs actual behavior

### Where can I get help?

- Check this FAQ
- Review [Troubleshooting Guide](/support/troubleshooting)
- Contact support
