## 1. Introduction

Welcome to the Hearts Game API!

This is a simple, open API designed for a fun and educational programming challenge. Each API call returns a link to an image that contains a random number of hearts and carrots, along with the correct count of each.

The API is open and does not require an API key for access.

## 2. Base URL

All endpoints are relative to the following base URL:

```
http://marcconrad.com/uob/heart/api.php
```

## 3. Authentication

No authentication is required to use this API. You can make requests directly to the endpoints.

## 4. Endpoints

There is a single, versatile endpoint that provides the game data.

### GET `/api.php`

This is the primary endpoint for retrieving a new game challenge. It returns a question (an image) and the corresponding solution.

#### **Query Parameters**

You can customize the response from this endpoint by using the following optional query parameters:

| Parameter | Type   | Description                                                                 | Default |
|-----------|--------|-----------------------------------------------------------------------------|---------|
| `out`     | string | Defines the format of the response. Can be `json` or `csv`.                 | `json`  |
| `base64`  | string | If set to `yes`, the image data is returned as a Base64 encoded string. | `no`    |


## 5. Response Objects and Data Models

Depending on the `out` parameter, the API will return data in one of two formats:

### **JSON (`out=json`)**

This is the default response format. It provides a structured object with clear key-value pairs.

**Data Model:**

| Key        | Type    | Description                                   |
|------------|---------|-----------------------------------------------|
| `question` | string  | The URL to the image file or a Base64 string. |
| `solution` | integer | The correct number of hearts in the image.    |
| `carrots`  | integer | The number of carrots in the image.           |

**Example JSON Response:**
```json
{
  "question": "https://www.sanfoh.com/uob/heart/data/heart_c11a2e_232686_005_006.png",
  "solution": 6,
  "carrots": 5
}
```

### **CSV (`out=csv`)**

When `out=csv`, the API returns the data in a single line of comma-separated values.

**Data Model:**

The CSV format provides the data in the following order:
`question,solution,carrots`

**Example CSV Response:**
```csv
"https://www.sanfoh.com/uob/heart/data/heart_c11a2e_232686_005_006.png",6,5
```

---

## 6. Examples

Here are some examples of how to call the API with different parameters.

### **Example 1: Standard JSON Response**

This is the most common use case. It returns a JSON object with a link to the image.

**Request:**
```
GET http://marcconrad.com/uob/heart/api.php
```

**Response:**
```json
{
  "question": "https://www.sanfoh.com/uob/heart/data/heart_example.png",
  "solution": 8,
  "carrots": 3
}
```

### **Example 2: CSV Response**

This request returns the data in CSV format.

**Request:**
```
GET http://marcconrad.com/uob/heart/api.php?out=csv
```

**Response:**
```csv
"https://www.sanfoh.com/uob/heart/data/heart_example.png",8,3
```

### **Example 3: JSON Response with Base64 Image**

This is useful if you want to avoid making a second HTTP request to download the image. The image data is embedded directly in the JSON response.

**Request:**
```
GET http://marcconrad.com/uob/heart/api.php?base64=yes
```

**Response:**
```json
{
  "question": "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAB... (long Base64 string)",
  "solution": 5,
  "carrots": 7
}
```

### **Example 4: CSV Response with Base64 Image**

This combines both `out=csv` and `base64=yes`.

**Request:**
```
GET http://marcconrad.com/uob/heart/api.php?out=csv&base64=yes
```

**Response:**
```csv
"data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAB... (long Base64 string)",5,7
```

## 7. Error Handling

The documentation does not specify error codes or formats. It is assumed that a successful request will always return a `200 OK` status code with the response body in the requested format. You should implement your client to handle potential network errors or unexpected API responses gracefully.