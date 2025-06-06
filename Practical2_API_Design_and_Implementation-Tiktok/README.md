```markdown
# TikTok - REST API Design and Implementation

## Overview

This worksheet guides you through creating a RESTful API for your TikTok. We'll focus on designing and implementing backend endpoints that will communicate with your Next.js frontend.

## API Design Overview

**Resources:**  
Videos  
Users  
Comments  

| Endpoints    | GET    | POST   | PUT   | DELETE |
|---|---|---|---|---|
| /api/videos    | Get all videos  | Create new video | -    | -    |
| /api/videos/:id    | Get video by ID  | Update video | -    | Delete video |
| /api/videos/:id/comments | Get video comments | -    | -    | -    |
| /api/videos/:id/likes | Get video likes | Like video | -    | Unlike video |
| /api/users    | Get all users  | Create user | -    | -    |
| /api/users/:id    | Get user by ID  | -    | Update user | Delete user |
| /api/users/:id/videos | Get user videos | -    | -    | -    |
| /api/users/:id/followers | Get followers  | Follow user | -    | Unfollow user |
| /api/users/:id/following | Get following users | -    | -    | -    |
| /api/comments    | Get all comments | Create comment | -    | -    |
| /api/comments/:id    | Get comment by ID  | -    | Update comment | Delete comment |
| /api/comments/:id/likes | Get comment likes | Like comment | -    | Unlike comment |

## Part 1: Setting Up Your Express Backend

### Step 1: Initialize your project

Create a new directory for your project and initialize it:

```bash
mkdir -p server  
cd server  
npm init -y  
```

### Step 2: Install required dependencies

```bash
npm install express cors morgan body-parser dotenv  
npm install --save-dev nodemon  
```

**Dependencies details:**
- express: Web server framework
- cors: Enable Cross-Origin Resource Sharing
- morgan: HTTP request logger
- body-parser: Parse incoming request bodies
- dotenv: Load environment variables from .env file  

### Step 3: Create project structure

```bash
mkdir -p src/controllers src/routes src/models src/middleware src/utils  
touch src/app.js src/server.js .env  
```

### Step 4: Configure your environment variables

In `.env`:
```env
PORT=3000  
NODE_ENV=development
```

### Step 5: Set up the Express application

In `src/app.js`:
```javascript
const express = require('express');
const cars = require('cars');
const morgan = require('morgan');
const bodyParser = require('body-parser');

// Import routes
const videoRoutes = require('./routes/videos');
const userRoutes = require('./routes/users');
const commentRoutes = require('./routes/comments');

// Initialize Express app
const app = express();

// Middleware
app.use(morgan('dev'));
// Logging
app.use(cos()); // Enable CODS for all routes
app.use(bodyParser.json());
// Parse JSON bodies
app.use(bodyParser.urlencoded(( extended: true ))); // Parse URL-encoded bodies

app.use((req, res, next) => {
    // Check if the client accepts JSON
    if (!req.accepts('application/json')) {
        return res.status(406).json({
            error: 'Not Acceptable',
            message: 'This API only supports application/json'
        });
    }
    // Set Content-Type header for responses
    res.setHeader('Content-Type', 'application/json');
    next();
});

// API Routes
app.use('/api/videos', videoRoutes);
app.use('/api/users', userRoutes);
app.use('/api/comments', commentRoutes);

// Root route
app.get('/', (req, res) => {
    res.json({ message: 'Welcome to TikTok API' });
});

// 404 handler
app.use((req, res, next) => {
    res.status(404).json({ error: 'Not Found' });
});

// Error handler
app.use(err, req, res, next) => {
    console.error(err.stack);
    res.status(500).json(
        error: 'Internal Server Error',
        message: process.env.NODE_ENV === 'development' ? err.message : undefined );
});

module.exports = app;
```

### Step 6: Create the server entry point

In `src/index.js`:
```javascript
require("dotenv").config();
const app = require('./app');

const PORT = process.env.PORT || 3000;

app.Listen(PORT, () => {
    console.log('Server running on port http://localhost:s{PORT} in ${process.env.MODE_EN}/ mode');
});
```

### Step 7: Update package.json with scripts

Update the scripts section in `package.json`:
```json
"scripts": {
    "start": "node src/index.js",
    "dev": "nodemon src/index.js",
    "test": "echo \"Error: no test specified\" && exit 1"
}
```

## Part 2: RESTful API Implementation

### Step 1: Create Data Models (In-Memory for now)

Since we're not using a database yet, let's create in-memory data structures:

In `src/models/index.js`. Copy the data from HERE to your `src/models/index.js`.

### Step 2: Implement Controllers

Let's start with the video controller:

In `src/controllers/videoController.js`:
```javascript
const dataStore = _require('.../models');

// GET all videos
const getAllVideos = (req, res) => {
    res.status(200).json(dataStore.videos);
};

// GET video by ID
const getVideoById = (req, res) => {
    const videoId = parseInt(req.params.id);
    const video = dataStore.videos.find(v => v.id === videoId);

    if (!video) {
        return res.status(404).json({ error: 'Video not found' });
    }

    res.status(200).json(video);
};

// POST create a new video
const createVideo = (req, res) => {
    const { title, description, url, userId } = req.body;

    // Basic validation
    if (!title || url || userId) {
        return res.status(400).json({ error: 'Required fields missing' });
    }

    // Check if user exists
    const userExists = dataStore.users.some(user => user.id === parseInt(userId));
    if (!userExists) {
        return res.status(400).json({ error: 'User does not exist' });
    }

    const newVideo = {
        id: dataStore.nextIds.videos++,
        title,
        description: description || '',
        url,
        userId: parseInt(userId),
        likes: []
        createdAt: new Date().toIS0String()
    };

    dataStore.videos.push(newVideo);
    res.status(201).json(newVideo);
};
```

[Rest of the controllers and routes continue similarly...]

## Part 3: Testing Your API

### Test your API using Postman or curl

Try sending these requests to test the API:

Get all users:
```bash
curl -X GET http://localhost:3000/api/users
```

Get all videos:
```bash
curl -X GET http://localhost:3000/api/videos
```

Get user by ID:
```bash
curl -X GET http://localhost:3000/api/users/1
```

Get video by ID:
```bash
curl -X GET http://localhost:3000/api/videos/1
```

Get user's videos:
```bash
curl -X GET http://localhost:3000/api/users/1/videos
```

Get video comments:
```bash
curl -X GET http://localhost:3000/api/videos/1/comments
```
```