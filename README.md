# Haiper AI API Unofficial

This is a detailed README for the Haiper AI API Unofficial endpoints. These endpoints allow you to interact with the Haiper AI service to generate video content based on a given prompt and image.

## Endpoints

### 1. Upload URL

**Endpoint:** `https://haiper-ai-api-unofficial.p.rapidapi.com/api/v1/haiper/upload-url`

**Method:** `POST`

**Description:** This endpoint is used to upload an image to the Haiper AI service. It returns a URL and metadata about the uploaded image.

**Request Body:**
```json
{
  "image_url": "https://cdn.midjourney.com/d94c00fd-cde0-4b14-9f9a-f24c8bcdf8ca/0_0.webp"
}
```

**Request Headers:**
```json
{
  "content-type": "application/json",
  "X-RapidAPI-Key": "your_rapid_api_key",
  "X-RapidAPI-Host": "haiper-ai-api-unofficial.p.rapidapi.com"
}
```

**Response Example:**
```json
{
  "source_image": "gs://haiper_vb/u/662bf07cd4f67a74c23641fb/2024-04-26T18-20-44-c5f83f6d.jpg",
  "input_content_type": "image",
  "input_width": 1920,
  "input_height": 2400,
  "source_image_url": null
}
```

### 2. Generate

**Endpoint:** `https://haiper-ai-api-unofficial.p.rapidapi.com/v1/haiper/generate`

**Method:** `POST`

**Description:** This endpoint is used to generate a video based on the provided prompt and configuration.

**Request Body:**
```json
{
  "prompt": "ice chunks moving in the icey water.",
  "config": {
    "source_image": "gs://haiper_vb/u/662bf07cd4f67a74c23641fb/2024-04-26T18-20-44-c5f83f6d.jpg",
    "input_content_type": "image",
    "input_width": 1920,
    "input_height": 2400,
    "source_image_url": null
  },
  "duration": 2,
  "seed": -1,
  "is_public": true
}
```

**Request Headers:**
```json
{
  "content-type": "application/json",
  "X-RapidAPI-Key": "your_rapid_api_key",
  "X-RapidAPI-Host": "haiper-ai-api-unofficial.p.rapidapi.com"
}
```

**Response Example:**
```json
{
  "status": "success",
  "value": {
    "job_id": "662bf1123e4fa7bd0711e39a"
  }
}
```

### 3. Job Results

**Endpoint:** `https://haiper-ai-api-unofficial.p.rapidapi.com/v1/haiper/job-results`

**Method:** `POST`

**Description:** This endpoint is used to check the status and retrieve the results of a previously submitted job.

**Request Body:**
```json
{
  "job_id": "662bf1123e4fa7bd0711e39a"
}
```

**Request Headers:**
```json
{
  "content-type": "application/json",
  "X-RapidAPI-Key": "your_rapid_api_key",
  "X-RapidAPI-Host": "haiper-ai-api-unofficial.p.rapidapi.com"
}
```

**Response Example (Processing):**
```json
{
  "status": "success",
  "value": {
    "status": "processing",
    "progress": 0.1
  }
}
```

**Response Example (Completed):**
```json
{
  "status": "success",
  "value": {
    "status": "succeed",
    "video_url": "https://cdnvb1.haiper.ai/jobs/662bf07cd4f67a74c23641fb/662bf1123e4fa7bd0711e39a/result.mp4",
    "thumbnail_url": "https://cdnvb1.haiper.ai/jobs/662bf07cd4f67a74c23641fb/662bf1123e4fa7bd0711e39a/result.mp4.thumbnail"
  }
}
```

## Request Validation

Here are the required request schemas for each endpoint:

### Upload URL

```javascript
const schema = {
  type: "object",
  properties: {
    image_url: { type: "string", optional: false },
  },
  required: ["image_url"],
};
```

### Generate

```javascript
const schema = {
  type: "object",
  properties: {
    prompt: { type: "string", optional: false },
    config: {
      type: "object",
      optional: true,
      properties: {
        source_image: { type: "string", optional: false },
        input_content_type: { type: "string", optional: false },
        input_width: { type: "integer", optional: false },
        input_height: { type: "integer", optional: false },
        source_image_url: { type: "string", optional: true },
      },
    },
    duration: { type: "integer", optional: true },
    resolution: { type: "integer", optional: true },
    seed: { type: "integer", optional: true },
    is_public: { type: "boolean", optional: true },
  },
  required: ["prompt"],
};
```

### Job Results

```javascript
const schema = {
  type: "object",
  properties: {
    job_id: { type: "string", optional: false },
  },
  required: ["job_id"],
};
```

Make sure to validate the request payloads against these schemas before making the API calls to ensure data integrity.

## Example Usage in Python 3

```python
import requests
import json

rapid_api_key = "your_rapid_api_key"

# Upload URL
upload_url = "https://haiper-ai-api-unofficial.p.rapidapi.com/api/v1/haiper/upload-url"
upload_payload = {
    "image_url": "https://cdn.midjourney.com/d94c00fd-cde0-4b14-9f9a-f24c8bcdf8ca/0_0.webp"
}
upload_headers = {
    "content-type": "application/json",
    "X-RapidAPI-Key": rapid_api_key,
    "X-RapidAPI-Host": "haiper-ai-api-unofficial.p.rapidapi.com"
}
upload_response = requests.post(upload_url, json=upload_payload, headers=upload_headers)
upload_data = json.loads(upload_response.text)

# Generate
generate_url = "https://haiper-ai-api-unofficial.p.rapidapi.com/v1/haiper/generate"
generate_payload = {
    "prompt": "ice chunks moving in the icey water.",
    "config": upload_data,
    "duration": 2,
    "seed": -1,
    "is_public": True
}
generate_headers = {
    "content-type": "application/json",
    "X-RapidAPI-Key": rapid_api_key,
    "X-RapidAPI-Host": "haiper-ai-api-unofficial.p.rapidapi.com"
}
generate_response = requests.post(generate_url, json=generate_payload, headers=generate_headers)
generate_data = json.loads(generate_response.text)

# Job Results
job_results_url = "https://haiper-ai-api-unofficial.p.rapidapi.com/v1/haiper/job-results"
job_results_payload = {
    "job_id": generate_data["value"]["job_id"]
}
job_results_headers = {
    "content-type": "application/json",
    "X-RapidAPI-Key": rapid_api_key,
    "X-RapidAPI-Host": "haiper-ai-api-unofficial.p.rapidapi.com"
}
job_results_response = requests.post(job_results_url, json=job_results_payload, headers=job_results_headers)
job_results_data = json.loads(job_results_response.text)

print(job_results_data)
```

This example shows how to use the Haiper AI API Unofficial endpoints in Python 3. It demonstrates the complete workflow of uploading an image, generating a video, and checking the job status and results.
