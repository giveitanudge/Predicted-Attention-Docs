# Predictions API Documentation
## Overview

This API allows clients to retrieve predicted attention (pAttention) for given URLs. The Attention score is a predictive measure indicating the potential engagement or interest a URL might generate among a target audience.

## Authentication

All API requests require authentication via an API token. Include your API token in the header of each request using the Authorization key.

Example:
```
Authorization: Bearer <YOUR_API_TOKEN>
```

## Base URL
All requests are made to the following base URL:

```
https://attention.ndg.io/api
```

## Endpoints
### GET /url

Retrieve the pAttention for a single URL.

**Request Parameters:**

`url`: *The* URL you wish to predict the attention of. Must be URL-encoded.

**Example Request:**

```
GET /url?url=https%3A%2F%2Fwww.example.com
Authorization: Bearer <YOUR_API_TOKEN>
```

### POST /urls

Submit a list of URLs in bulk, up to a maximum of 100, to retrieve their pAttention asynchronously.

**Request Body:**

`urls`: A JSON array of URLs for which to predict Attention for.


**Example Request:**

```
POST /urls
Authorization: Bearer <YOUR_API_TOKEN>
Content-Type: application/json

{
  "urls": [
    "https://www.example.com",
    "https://www.anotherexample.com"
  ]
}
```

**Example Response:**
```
{
  "requestId": "12345abcd"
}
```

### GET /results
Retrieve the results of a bulk URL submission using the request ID.

*Please note, results for URLs can take up to 48 hours to generate.*

**Request Parameters:**

`requestId`: The ID of the request for which to retrieve the results.

**Example Request:**

```
GET /results?requestId=12345abcd
Authorization: Bearer <YOUR_API_TOKEN>
```

**Example Response:**

```
[
  {
    "url": "https://www.example.com",
    "attentionScore": 0.92
  },
  {
    "url": "https://www.anotherexample.com",
    "attentionScore": 0.75
  }
]
```

## Status Codes

The API uses the following status codes:

- **200 OK**: The request was successful.
- **202 Accepted**: The request has been accepted for processing, but the processing has not been completed.
- **401 Unauthorized**: Authentication failed, likely due to an invalid API token.
- **400 Bad Request**: The request was malformed. This could be due to missing parameters or invalid URL formats.
- **404 Not Found**: The specified request ID was not found.
- **500 Internal Server Error**: An error occurred on the server.

## Error Handling

Errors are returned as JSON objects with a message field describing the error.

**Example Error Response:**
```
{
  "message": "Authentication failed. Invalid API token."
}
```