# 🎬 VideoTube - Video Streaming Platform Backend

A full-featured video streaming platform backend built with Express.js and MongoDB, providing RESTful APIs for video management, user authentication, playlists, subscriptions, and social interactions.

## ✨ Features

- **Authentication**: JWT-based auth with access/refresh tokens, secure password hashing
- **Video Management**: Upload, update, delete videos with thumbnails, pagination, search, and filtering
- **User Management**: Profile management, avatar/cover image uploads, watch history
- **Playlists**: Create, update, delete playlists; add/remove videos
- **Social Features**: Subscriptions, likes (videos/comments/tweets), tweets
- **Advanced**: MongoDB aggregation pipelines, Cloudinary integration, file upload handling

## 🛠 Tech Stack

- **Backend**: Express.js, Node.js
- **Database**: MongoDB, Mongoose
- **Authentication**: JWT, bcrypt
- **File Storage**: Cloudinary, Multer
- **Other**: CORS, cookie-parser, mongoose-aggregate-paginate-v2

## 🚀 Quick Start

### Prerequisites
- Node.js (v14+)
- MongoDB 
- Cloudinary account

### Installation

1. **Clone and install**
   ```bash
   git clone <repository-url>
   cd VideoTube/Backend-Project
   npm install
   ```

2. **Environment Setup**
   Create a `.env` file:
   ```env
   PORT=8000
   MONGODB_URI=mongodb://localhost:27017
   
   ACCESS_TOKEN_SECRET=your_access_token_secret
   REFRESH_TOKEN_SECRET=your_refresh_token_secret
   ACCESS_TOKEN_EXPIRY=15m
   REFRESH_TOKEN_EXPIRY=7d
   
   CLOUDINARY_CLOUD_NAME=your_cloudinary_cloud_name
   CLOUDINARY_API_KEY=your_cloudinary_api_key
   CLOUDINARY_API_SECRET=your_cloudinary_api_secret
   ```

3. **Run the server**
   ```bash
   npm run dev
   ```

   Server runs on `http://localhost:8000`

## 📁 Project Structure

```
src/
├── app.js              # Express app configuration
├── index.js            # Server entry point
├── controllers/        # Request handlers
├── models/             # Mongoose schemas
├── routes/             # API routes
├── middlewares/        # Auth & file upload middleware
├── utils/              # Utilities (ApiError, ApiResponse, Cloudinary)
└── db/                 # Database connection
```

## 🌐 API Endpoints

### Base URL: `http://localhost:8000/api/v1`

#### 🔐 Authentication (`/users`)
- `POST /users/register` - Register with avatar/cover image
- `POST /users/login` - Login (returns access/refresh tokens)
- `POST /users/logout` - Logout
- `POST /users/refresh` - Refresh access token
- `POST /users/changePassword` - Change password
- `GET /users/current-user` - Get current user
- `PATCH /users/updateAccount` - Update account details
- `PATCH /users/updateAvatar` - Update avatar
- `PATCH /users/updateCoverImage` - Update cover image
- `GET /users/profile/:username` - Get channel profile
- `GET /users/watchHistory` - Get watch history

#### 🎥 Videos (`/videos`)
- `POST /videos/publish` - Upload video (requires: title, description, videoFile, thumbnail)
- `GET /videos/get/:id` - Get video by ID
- `GET /videos/all` - Get all videos (query: page, limit, query, sortBy, sortType, userId)
- `PATCH /videos/update/:videoId` - Update video
- `DELETE /videos/delete/:videoId` - Delete video
- `PATCH /videos/toggle-publish/:videoId` - Toggle publish status

#### 📚 Playlists (`/playlist`)
- `POST /playlist/create` - Create playlist
- `GET /playlist/user/:userId` - Get user playlists
- `GET /playlist/:playlistId` - Get playlist by ID
- `PATCH /playlist/:playlistId` - Update playlist
- `DELETE /playlist/:playlistId` - Delete playlist
- `POST /playlist/add/:videoId/:playlistId` - Add video to playlist
- `POST /playlist/remove/:videoId/:playlistId` - Remove video from playlist

#### 👥 Subscriptions (`/subscriptions`)
- `POST /subscriptions/c/:channelId` - Toggle subscription
- `GET /subscriptions/c/:channelId` - Get subscriber count
- `GET /subscriptions/u/:subscriberId` - Get subscribed channels

#### ❤️ Likes (`/likes`)
- `POST /likes/toggle/v/:videoId` - Toggle video like
- `POST /likes/toggle/c/:commentId` - Toggle comment like
- `POST /likes/toggle/t/:tweetId` - Toggle tweet like
- `GET /likes/videos` - Get liked videos

#### 🐦 Tweets (`/tweet`)
- `POST /tweet/create` - Create tweet
- `GET /tweet/find` - Get user tweets
- `PATCH /tweet/:tweetId` - Update tweet
- `DELETE /tweet/:tweetId` - Delete tweet

## 💡 Usage Examples

### Register User
```javascript
const formData = new FormData();
formData.append('fullName', 'John Doe');
formData.append('email', 'john@example.com');
formData.append('username', 'johndoe');
formData.append('password', 'password123');
formData.append('avatar', avatarFile);

fetch('http://localhost:8000/api/v1/users/register', {
  method: 'POST',
  body: formData
});
```

### Login
```javascript
fetch('http://localhost:8000/api/v1/users/login', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    email: 'john@example.com',
    password: 'password123'
  }),
  credentials: 'include'
});
```

### Publish Video
```javascript
const formData = new FormData();
formData.append('title', 'My Video');
formData.append('description', 'Video description');
formData.append('videoFile', videoFile);
formData.append('thumbnail', thumbnailFile);

fetch('http://localhost:8000/api/v1/videos/publish', {
  method: 'POST',
  headers: { 'Authorization': `Bearer ${accessToken}` },
  body: formData
});
```

## 🔒 Authentication

- Uses JWT with access tokens (15min) and refresh tokens (7 days)
- Tokens stored in HTTP-only cookies or sent via `Authorization: Bearer <token>` header
- Most endpoints require authentication

## 📤 File Upload

- Files uploaded via Multer to `public/temp/`
- Automatically uploaded to Cloudinary
- Temporary files deleted after upload
- Supports videos and images (JPG, PNG, GIF, WebP)

## ⚠️ Error Response Format

```json
{
  "statusCode": 400,
  "message": "Error message",
  "success": false,
  "data": null
}
```

## 📜 Scripts

```bash
npm run dev    # Start development server with auto-reload
```

## 🗄 Database Models

- **User**: username, email, password, avatar, coverImage, watchHistory
- **Video**: videoFile, thumbnail, owner, title, description, views, isPublished
- **Playlist**: name, description, owner, videos[]
- **Subscription**: subscriber, channel
- **Like**: video/comment/tweet, likedBy
- **Tweet**: content, owner
- **Comment**: content, video, owner

## 📄 License

ISC License

---

**Note**: Ensure MongoDB and Cloudinary are configured before running the application.
