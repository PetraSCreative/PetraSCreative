# PetraSCreative API Documentation

## Base URL

```
https://api.petrascreative.com/api
```

## Authentication

All authenticated endpoints require a JWT token in the `Authorization` header:

```
Authorization: Bearer <token>
```

---

## Endpoints

### Auth

#### Register

```
POST /auth/register
```

**Request:**
```json
{
  "email": "user@example.com",
  "password": "securepassword",
  "fullName": "John Doe"
}
```

**Response:**
```json
{
  "token": "jwt_token_here",
  "user": {
    "id": "uuid",
    "email": "user@example.com",
    "fullName": "John Doe"
  }
}
```

#### Login

```
POST /auth/login
```

**Request:**
```json
{
  "email": "user@example.com",
  "password": "securepassword"
}
```

**Response:** Same as Register

---

### Videos

#### Generate Video

```
POST /videos/generate
```

**Request:**
```json
{
  "scriptContent": "Your script text here",
  "title": "Video Title",
  "description": "Video description",
  "voiceId": "en-US-Neural2-A",
  "templateId": "default"
}
```

**Response:**
```json
{
  "videoId": "uuid",
  "status": "processing",
  "createdAt": "2024-01-15T10:30:00Z",
  "estimatedCompletion": "2024-01-15T10:45:00Z"
}
```

#### Get Video Details

```
GET /videos/:videoId
```

**Response:**
```json
{
  "id": "uuid",
  "title": "Video Title",
  "description": "Video description",
  "status": "completed",
  "videoUrl": "https://s3.amazonaws.com/...",
  "thumbnailUrl": "https://s3.amazonaws.com/...",
  "duration": 120,
  "createdAt": "2024-01-15T10:30:00Z",
  "updatedAt": "2024-01-15T10:45:00Z"
}
```

#### List User Videos

```
GET /videos?page=1&limit=10
```

**Response:**
```json
{
  "videos": [...],
  "pagination": {
    "page": 1,
    "limit": 10,
    "total": 25
  }
}
```

#### Update Video

```
PUT /videos/:videoId
```

**Request:**
```json
{
  "title": "Updated Title",
  "description": "Updated description",
  "segments": [...],
  "transitions": [...]
}
```

#### Export Video

```
POST /videos/:videoId/export
```

**Request:**
```json
{
  "format": "mp4",
  "quality": "1080p"
}
```

**Response:**
```json
{
  "exportId": "uuid",
  "status": "processing",
  "downloadUrl": null,
  "estimatedCompletion": "2024-01-15T11:00:00Z"
}
```

---

### Narration

#### Generate Voiceover

```
POST /narration/generate
```

**Request:**
```json
{
  "text": "Your narration text",
  "voiceId": "en-US-Neural2-A",
  "speed": 1.0,
  "pitch": 1.0
}
```

**Response:**
```json
{
  "narrationId": "uuid",
  "audioUrl": "https://s3.amazonaws.com/...",
  "duration": 45,
  "createdAt": "2024-01-15T10:30:00Z"
}
```

#### Get Available Voices

```
GET /narration/voices?language=en-US
```

**Response:**
```json
{
  "voices": [
    {
      "id": "en-US-Neural2-A",
      "name": "English US - Female A",
      "language": "en-US",
      "gender": "female"
    }
  ]
}
```

#### Preview Voice

```
POST /narration/preview
```

**Request:**
```json
{
  "voiceId": "en-US-Neural2-A",
  "text": "Sample text to preview"
}
```

**Response:**
```json
{
  "audioUrl": "https://s3.amazonaws.com/...",
  "duration": 5
}
```

---

### Templates

#### Get Transitions

```
GET /templates/transitions
```

**Response:**
```json
{
  "transitions": [
    {
      "id": "fade",
      "name": "Fade",
      "duration": 500
    }
  ]
}
```

#### Get Animations

```
GET /templates/animations
```

**Response:**
```json
{
  "animations": [
    {
      "id": "slide-in",
      "name": "Slide In",
      "duration": 300
    }
  ]
}
```

---

## Error Handling

All errors follow this format:

```json
{
  "error": "Error message",
  "code": "ERROR_CODE",
  "statusCode": 400
}
```

### Common Status Codes

- `200` - Success
- `201` - Created
- `400` - Bad Request
- `401` - Unauthorized
- `403` - Forbidden
- `404` - Not Found
- `500` - Server Error
