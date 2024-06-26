# Predictions API Documentation
## Overview

This API allows clients to retrieve predicted attention (attentionCategory) for given URLs. The Attention score is a predictive measure indicating the potential engagement or interest a URL might generate among a target audience.

pAttention (Prediction Attention) is returned in seconds along with an Attention Category. 
- Low >15
- Medium 16-30
- Good 31-48
- High 49-59
- Excellent 60+

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
### GET /prediction

Retrieve the attentionCategory for a single URL.

**Request Parameters:**

`url`: *The* URL you wish to predict the attention of. Must be URL-encoded.

**Example Request:**

```
GET /prediction?url=https://www.example.com/example/
Authorization: Bearer <YOUR_API_TOKEN>
```

**Example Successful Response:**

```
[
    {
        "url": "https://www.example.com/example-url",
        "pAttention": "79.0",
        "attentionCategory": "Excellent",
        "insights": [
            "Consider making the content longer.",
            "Make it easier to read."
        ]
    }
]
```

**Example Failed Response:**

```
{
  "statusCode":404,"error":"Not Found",
  "message":"URL not found. URL added to prediction queue, check back in 48 hours for prediction."
}
```

### POST /prediction

Submit a list of URLs in bulk, up to a maximum of 100, to retrieve their pAttention, attentionCategory and insights.

**Request Body:**

`urls`: A JSON array of URLs for which to predict Attention for.


**Example Request:**

```
POST /urls
Authorization: Bearer <YOUR_API_TOKEN>
Content-Type: application/json

{
  "urls": [
    "https://www.example.com/example-url",
    "https://www.example.com/another-example-url"
  ]
}
```

**Example Response:**
```
[
    {
        "url": "https://www.example.com/example-url",
        "pAttention": "64.0",
        "attentionCategory": "Excellent",
        "insights": [
            "Revisit the tone of the piece, is it engaging?",
            "Make it easier to read.",
            "Your content could be more objective."
        ]
    },
    {
        "https://www.example.com/another-example-url",
        "pAttention": "79.0",
        "attentionCategory": "Excellent",
        "insights": [
            "Consider making the content longer.",
            "Make it easier to read."
        ]
    }
]
```

## Status Codes

The API uses the following status codes:

- **200 OK**: The request was successful.
- **202 Accepted**: The request has been accepted for processing, but the processing has not been completed.
- **401 Unauthorized**: Authentication failed, likely due to an invalid API token.
- **400 Bad Request**: The request was malformed. This could be due to missing parameters or invalid URL formats.
- **404 Not Found**: The specified request ID or URL was not found.
- **500 Internal Server Error**: An error occurred on the server.

## Error Handling

Errors are returned as JSON objects with a message field describing the error.

**Example Error Response:**
```
{
  "statusCode":401,"error":"Unauthorized",
  "message": "Authentication failed. Invalid API token."
}
```