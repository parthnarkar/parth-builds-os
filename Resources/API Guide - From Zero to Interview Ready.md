# Complete MERN Stack API Guide: From Zero to Interview Ready

![Node.js](https://img.shields.io/badge/node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)
![Express.js](https://img.shields.io/badge/express.js-000000?style=for-the-badge&logo=express&logoColor=white)
![MongoDB](https://img.shields.io/badge/MongoDB-47A248?style=for-the-badge&logo=mongodb&logoColor=white)
![React](https://img.shields.io/badge/React-61DAFB?style=for-the-badge&logo=react&logoColor=black)

---

## Table of Contents
1. [What is an API?](#what-is-an-api)
2. [Understanding REST APIs](#understanding-rest-apis)
3. [Setting Up Your Development Environment](#setting-up-your-development-environment)
4. [Building Your First Express API](#building-your-first-express-api)
5. [Connecting to MongoDB](#connecting-to-mongodb)
6. [CRUD Operations in Detail](#crud-operations-in-detail)
7. [Middleware Explained](#middleware-explained)
8. [Authentication & Authorization](#authentication--authorization)
9. [Error Handling](#error-handling)
10. [Validation](#validation)
11. [File Upload](#file-upload)
12. [API Security Best Practices](#api-security-best-practices)
13. [Testing Your API](#testing-your-api)
14. [Deployment](#deployment)
15. [Common Interview Questions](#common-interview-questions)

---

## What is an API?

**API stands for Application Programming Interface.** Think of it as a waiter in a restaurant:

- **You (Frontend/Client)**: Order food
- **Waiter (API)**: Takes your order to the kitchen
- **Kitchen (Backend/Database)**: Prepares the food
- **Waiter (API)**: Brings food back to you

In technical terms, an API is a set of rules that allows different software applications to communicate with each other.

### Real-World Example
When you use a weather app on your phone, the app sends a request to a weather API, which fetches data from a database and sends it back to your app.

---

## Understanding REST APIs

**REST (Representational State Transfer)** is a set of rules for building APIs.

### The 6 Key Principles of REST

1. **Client-Server Architecture**: Frontend and backend are separate
2. **Stateless**: Each request contains all information needed (no session memory)
3. **Cacheable**: Responses can be cached to improve performance
4. **Uniform Interface**: Consistent way of interacting with resources
5. **Layered System**: Client doesn't know if it's connected directly to the server
6. **Code on Demand** (optional): Server can send executable code

### HTTP Methods (CRUD Operations)

| Method | Purpose | Example |
|--------|---------|---------|
| GET | Read/Retrieve data | Get all users |
| POST | Create new data | Create a new user |
| PUT | Update entire resource | Update all user details |
| PATCH | Update partial resource | Update user email only |
| DELETE | Delete data | Delete a user |

### HTTP Status Codes You Must Know

**2xx - Success**
- `200 OK`: Request succeeded
- `201 Created`: New resource created
- `204 No Content`: Success but no data to return

**4xx - Client Errors**
- `400 Bad Request`: Invalid data sent
- `401 Unauthorized`: Not authenticated
- `403 Forbidden`: Authenticated but not allowed
- `404 Not Found`: Resource doesn't exist
- `422 Unprocessable Entity`: Validation failed

**5xx - Server Errors**
- `500 Internal Server Error`: Something went wrong on server
- `503 Service Unavailable`: Server is down

---

## Setting Up Your Development Environment

### Prerequisites
Install these on your computer:

1. **Node.js** (v14 or higher): Download from nodejs.org
2. **MongoDB**: 
   - Local: Download MongoDB Community Server
   - Cloud: Sign up for MongoDB Atlas (free tier)
3. **Postman**: For testing APIs
4. **VS Code**: Code editor with useful extensions

### Initialize Your Project

```bash
# Create project folder
mkdir mern-api-project
cd mern-api-project

# Initialize npm
npm init -y

# Install required packages
npm install express mongoose dotenv cors

# Install development dependencies
npm install --save-dev nodemon
```

### Package Explanations

- **express**: Web framework for building APIs
- **mongoose**: ODM (Object Data Modeling) library for MongoDB
- **dotenv**: Load environment variables from .env file
- **cors**: Allow cross-origin requests (frontend can call backend)
- **nodemon**: Auto-restart server when code changes

### Project Structure

```
mern-api-project/
│
├── config/
│   └── db.js
├── models/
│   └── User.js
├── controllers/
│   └── userController.js
├── routes/
│   └── userRoutes.js
├── middleware/
│   ├── auth.js
│   └── errorHandler.js
├── .env
├── .gitignore
├── server.js
└── package.json
```

---

## Building Your First Express API

### Step 1: Create server.js

```javascript
// server.js
const express = require('express');
const dotenv = require('dotenv');
const cors = require('cors');

// Load environment variables
dotenv.config();

// Initialize express app
const app = express();

// Middleware
app.use(express.json()); // Parse JSON bodies
app.use(cors()); // Enable CORS

// Simple test route
app.get('/', (req, res) => {
  res.json({ message: 'Welcome to MERN API' });
});

// Start server
const PORT = process.env.PORT || 5000;
app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
```

### Step 2: Update package.json Scripts

```json
"scripts": {
  "start": "node server.js",
  "dev": "nodemon server.js"
}
```

### Step 3: Create .env File

```env
PORT=5000
MONGO_URI=your_mongodb_connection_string
JWT_SECRET=your_secret_key_here
```

### Step 4: Run Your Server

```bash
npm run dev
```

Visit `http://localhost:5000` in your browser. You should see:
```json
{ "message": "Welcome to MERN API" }
```

---

## Connecting to MongoDB

### Step 1: Get MongoDB Connection String

**For MongoDB Atlas (Cloud):**
1. Create account at mongodb.com/cloud/atlas
2. Create a cluster (free tier)
3. Click "Connect" → "Connect your application"
4. Copy the connection string

**For Local MongoDB:**
```
mongodb://localhost:27017/your_database_name
```

### Step 2: Create Database Connection File

```javascript
// config/db.js
const mongoose = require('mongoose');

const connectDB = async () => {
  try {
    const conn = await mongoose.connect(process.env.MONGO_URI, {
      useNewUrlParser: true,
      useUnifiedTopology: true,
    });
    console.log(`MongoDB Connected: ${conn.connection.host}`);
  } catch (error) {
    console.error(`Error: ${error.message}`);
    process.exit(1); // Exit with failure
  }
};

module.exports = connectDB;
```

### Step 3: Update server.js

```javascript
// Add at the top
const connectDB = require('./config/db');

// After dotenv.config()
connectDB();
```

---

## CRUD Operations in Detail

### Step 1: Create a Model

```javascript
// models/User.js
const mongoose = require('mongoose');

const userSchema = new mongoose.Schema({
  name: {
    type: String,
    required: [true, 'Please add a name'],
    trim: true,
    maxlength: [50, 'Name cannot be more than 50 characters']
  },
  email: {
    type: String,
    required: [true, 'Please add an email'],
    unique: true,
    lowercase: true,
    match: [
      /^\w+([\.-]?\w+)*@\w+([\.-]?\w+)*(\.\w{2,3})+$/,
      'Please add a valid email'
    ]
  },
  password: {
    type: String,
    required: [true, 'Please add a password'],
    minlength: [6, 'Password must be at least 6 characters'],
    select: false // Don't return password by default
  },
  age: {
    type: Number,
    min: [1, 'Age must be at least 1'],
    max: [120, 'Age cannot be more than 120']
  },
  role: {
    type: String,
    enum: ['user', 'admin'],
    default: 'user'
  }
}, {
  timestamps: true // Adds createdAt and updatedAt automatically
});

module.exports = mongoose.model('User', userSchema);
```

### Step 2: Create Controller

```javascript
// controllers/userController.js
const User = require('../models/User');

// @desc    Get all users
// @route   GET /api/users
// @access  Public
exports.getUsers = async (req, res) => {
  try {
    const users = await User.find();
    
    res.status(200).json({
      success: true,
      count: users.length,
      data: users
    });
  } catch (error) {
    res.status(500).json({
      success: false,
      error: error.message
    });
  }
};

// @desc    Get single user
// @route   GET /api/users/:id
// @access  Public
exports.getUser = async (req, res) => {
  try {
    const user = await User.findById(req.params.id);
    
    if (!user) {
      return res.status(404).json({
        success: false,
        error: 'User not found'
      });
    }
    
    res.status(200).json({
      success: true,
      data: user
    });
  } catch (error) {
    res.status(500).json({
      success: false,
      error: error.message
    });
  }
};

// @desc    Create user
// @route   POST /api/users
// @access  Public
exports.createUser = async (req, res) => {
  try {
    const user = await User.create(req.body);
    
    res.status(201).json({
      success: true,
      data: user
    });
  } catch (error) {
    res.status(400).json({
      success: false,
      error: error.message
    });
  }
};

// @desc    Update user
// @route   PUT /api/users/:id
// @access  Public
exports.updateUser = async (req, res) => {
  try {
    const user = await User.findByIdAndUpdate(
      req.params.id,
      req.body,
      {
        new: true, // Return updated document
        runValidators: true // Run model validations
      }
    );
    
    if (!user) {
      return res.status(404).json({
        success: false,
        error: 'User not found'
      });
    }
    
    res.status(200).json({
      success: true,
      data: user
    });
  } catch (error) {
    res.status(400).json({
      success: false,
      error: error.message
    });
  }
};

// @desc    Delete user
// @route   DELETE /api/users/:id
// @access  Public
exports.deleteUser = async (req, res) => {
  try {
    const user = await User.findByIdAndDelete(req.params.id);
    
    if (!user) {
      return res.status(404).json({
        success: false,
        error: 'User not found'
      });
    }
    
    res.status(200).json({
      success: true,
      data: {},
      message: 'User deleted successfully'
    });
  } catch (error) {
    res.status(500).json({
      success: false,
      error: error.message
    });
  }
};
```

### Step 3: Create Routes

```javascript
// routes/userRoutes.js
const express = require('express');
const router = express.Router();
const {
  getUsers,
  getUser,
  createUser,
  updateUser,
  deleteUser
} = require('../controllers/userController');

router.route('/')
  .get(getUsers)
  .post(createUser);

router.route('/:id')
  .get(getUser)
  .put(updateUser)
  .delete(deleteUser);

module.exports = router;
```

### Step 4: Use Routes in server.js

```javascript
// Import routes
const userRoutes = require('./routes/userRoutes');

// Mount routes
app.use('/api/users', userRoutes);
```

---

## Middleware Explained

Middleware is code that runs between receiving a request and sending a response.

### Types of Middleware

```javascript
// 1. Application-level middleware
app.use(express.json()); // Runs for all routes

// 2. Router-level middleware
router.use(authMiddleware); // Runs for specific router

// 3. Error-handling middleware (must have 4 parameters)
app.use((err, req, res, next) => {
  res.status(500).json({ error: err.message });
});

// 4. Built-in middleware
express.static('public'); // Serve static files

// 5. Third-party middleware
const cors = require('cors');
app.use(cors());
```

### Creating Custom Middleware

```javascript
// middleware/logger.js
const logger = (req, res, next) => {
  console.log(`${req.method} ${req.protocol}://${req.get('host')}${req.originalUrl}`);
  next(); // MUST call next() to move to next middleware
};

module.exports = logger;

// Use in server.js
const logger = require('./middleware/logger');
app.use(logger);
```

### Request Flow with Middleware

```
Request → Middleware 1 → Middleware 2 → Route Handler → Response
```

---

## Authentication & Authorization

### Step 1: Install Required Packages

```bash
npm install bcryptjs jsonwebtoken
```

### Step 2: Hash Password Before Saving

```javascript
// models/User.js
const bcrypt = require('bcryptjs');

// Add this before module.exports
userSchema.pre('save', async function(next) {
  // Only hash if password is modified
  if (!this.isModified('password')) {
    next();
  }
  
  const salt = await bcrypt.genSalt(10);
  this.password = await bcrypt.hash(this.password, salt);
});

// Method to compare passwords
userSchema.methods.matchPassword = async function(enteredPassword) {
  return await bcrypt.compare(enteredPassword, this.password);
};

// Method to generate JWT token
userSchema.methods.getSignedJwtToken = function() {
  return jwt.sign({ id: this._id }, process.env.JWT_SECRET, {
    expiresIn: '30d'
  });
};
```

### Step 3: Create Auth Controller

```javascript
// controllers/authController.js
const User = require('../models/User');

// @desc    Register user
// @route   POST /api/auth/register
// @access  Public
exports.register = async (req, res) => {
  try {
    const { name, email, password } = req.body;
    
    // Check if user exists
    const userExists = await User.findOne({ email });
    if (userExists) {
      return res.status(400).json({
        success: false,
        error: 'User already exists'
      });
    }
    
    // Create user
    const user = await User.create({
      name,
      email,
      password
    });
    
    // Create token
    const token = user.getSignedJwtToken();
    
    res.status(201).json({
      success: true,
      token,
      data: {
        id: user._id,
        name: user.name,
        email: user.email
      }
    });
  } catch (error) {
    res.status(500).json({
      success: false,
      error: error.message
    });
  }
};

// @desc    Login user
// @route   POST /api/auth/login
// @access  Public
exports.login = async (req, res) => {
  try {
    const { email, password } = req.body;
    
    // Validate email & password
    if (!email || !password) {
      return res.status(400).json({
        success: false,
        error: 'Please provide email and password'
      });
    }
    
    // Check for user (include password this time)
    const user = await User.findOne({ email }).select('+password');
    
    if (!user) {
      return res.status(401).json({
        success: false,
        error: 'Invalid credentials'
      });
    }
    
    // Check if password matches
    const isMatch = await user.matchPassword(password);
    
    if (!isMatch) {
      return res.status(401).json({
        success: false,
        error: 'Invalid credentials'
      });
    }
    
    // Create token
    const token = user.getSignedJwtToken();
    
    res.status(200).json({
      success: true,
      token,
      data: {
        id: user._id,
        name: user.name,
        email: user.email
      }
    });
  } catch (error) {
    res.status(500).json({
      success: false,
      error: error.message
    });
  }
};

// @desc    Get current logged in user
// @route   GET /api/auth/me
// @access  Private
exports.getMe = async (req, res) => {
  try {
    const user = await User.findById(req.user.id);
    
    res.status(200).json({
      success: true,
      data: user
    });
  } catch (error) {
    res.status(500).json({
      success: false,
      error: error.message
    });
  }
};
```

### Step 4: Create Auth Middleware

```javascript
// middleware/auth.js
const jwt = require('jsonwebtoken');
const User = require('../models/User');

exports.protect = async (req, res, next) => {
  let token;
  
  // Check if token exists in headers
  if (req.headers.authorization && req.headers.authorization.startsWith('Bearer')) {
    // Extract token from "Bearer TOKEN"
    token = req.headers.authorization.split(' ')[1];
  }
  
  // Make sure token exists
  if (!token) {
    return res.status(401).json({
      success: false,
      error: 'Not authorized to access this route'
    });
  }
  
  try {
    // Verify token
    const decoded = jwt.verify(token, process.env.JWT_SECRET);
    
    // Get user from token
    req.user = await User.findById(decoded.id);
    
    next();
  } catch (error) {
    return res.status(401).json({
      success: false,
      error: 'Not authorized to access this route'
    });
  }
};

// Grant access to specific roles
exports.authorize = (...roles) => {
  return (req, res, next) => {
    if (!roles.includes(req.user.role)) {
      return res.status(403).json({
        success: false,
        error: `User role '${req.user.role}' is not authorized to access this route`
      });
    }
    next();
  };
};
```

### Step 5: Protect Routes

```javascript
// routes/userRoutes.js
const { protect, authorize } = require('../middleware/auth');

// Protect all routes after this middleware
router.use(protect);

// Only admin can access these
router.use(authorize('admin'));

router.route('/')
  .get(getUsers)
  .post(createUser);
```

### Step 6: Create Auth Routes

```javascript
// routes/authRoutes.js
const express = require('express');
const router = express.Router();
const { register, login, getMe } = require('../controllers/authController');
const { protect } = require('../middleware/auth');

router.post('/register', register);
router.post('/login', login);
router.get('/me', protect, getMe);

module.exports = router;

// In server.js
app.use('/api/auth', require('./routes/authRoutes'));
```

---

## Error Handling

### Create Error Handler Middleware

```javascript
// middleware/errorHandler.js
const errorHandler = (err, req, res, next) => {
  let error = { ...err };
  error.message = err.message;
  
  // Log to console for dev
  console.log(err);
  
  // Mongoose bad ObjectId
  if (err.name === 'CastError') {
    const message = 'Resource not found';
    error = { message, statusCode: 404 };
  }
  
  // Mongoose duplicate key
  if (err.code === 11000) {
    const message = 'Duplicate field value entered';
    error = { message, statusCode: 400 };
  }
  
  // Mongoose validation error
  if (err.name === 'ValidationError') {
    const message = Object.values(err.errors).map(val => val.message);
    error = { message, statusCode: 400 };
  }
  
  res.status(error.statusCode || 500).json({
    success: false,
    error: error.message || 'Server Error'
  });
};

module.exports = errorHandler;
```

### Use Error Handler

```javascript
// server.js (at the very end, after all routes)
const errorHandler = require('./middleware/errorHandler');
app.use(errorHandler);
```

### Create Async Handler to Avoid Try-Catch

```javascript
// middleware/asyncHandler.js
const asyncHandler = fn => (req, res, next) =>
  Promise.resolve(fn(req, res, next)).catch(next);

module.exports = asyncHandler;

// Usage in controller
const asyncHandler = require('../middleware/asyncHandler');

exports.getUsers = asyncHandler(async (req, res, next) => {
  const users = await User.find();
  res.status(200).json({ success: true, data: users });
});
```

---

## Validation

### Using Express Validator

```bash
npm install express-validator
```

```javascript
// middleware/validators.js
const { body, validationResult } = require('express-validator');

exports.validateUser = [
  body('name')
    .trim()
    .notEmpty().withMessage('Name is required')
    .isLength({ min: 2, max: 50 }).withMessage('Name must be between 2 and 50 characters'),
  
  body('email')
    .trim()
    .notEmpty().withMessage('Email is required')
    .isEmail().withMessage('Please provide a valid email')
    .normalizeEmail(),
  
  body('password')
    .notEmpty().withMessage('Password is required')
    .isLength({ min: 6 }).withMessage('Password must be at least 6 characters'),
  
  body('age')
    .optional()
    .isInt({ min: 1, max: 120 }).withMessage('Age must be between 1 and 120'),
  
  // Middleware to check validation results
  (req, res, next) => {
    const errors = validationResult(req);
    if (!errors.isEmpty()) {
      return res.status(400).json({
        success: false,
        errors: errors.array()
      });
    }
    next();
  }
];

// Use in routes
const { validateUser } = require('../middleware/validators');
router.post('/', validateUser, createUser);
```

---

## File Upload

### Using Multer

```bash
npm install multer
```

```javascript
// middleware/upload.js
const multer = require('multer');
const path = require('path');

// Configure storage
const storage = multer.diskStorage({
  destination: function(req, file, cb) {
    cb(null, 'uploads/'); // Make sure this folder exists
  },
  filename: function(req, file, cb) {
    // Create unique filename
    cb(null, `${Date.now()}-${file.originalname}`);
  }
});

// File filter
const fileFilter = (req, file, cb) => {
  // Accept images only
  if (file.mimetype.startsWith('image')) {
    cb(null, true);
  } else {
    cb(new Error('Not an image! Please upload only images.'), false);
  }
};

const upload = multer({
  storage: storage,
  limits: {
    fileSize: 1024 * 1024 * 5 // 5MB limit
  },
  fileFilter: fileFilter
});

module.exports = upload;

// Usage in routes
const upload = require('../middleware/upload');

// Single file
router.post('/upload', upload.single('image'), (req, res) => {
  res.json({
    success: true,
    file: req.file
  });
});

// Multiple files
router.post('/upload-multiple', upload.array('images', 5), (req, res) => {
  res.json({
    success: true,
    files: req.files
  });
});
```

### Serve Static Files

```javascript
// server.js
app.use('/uploads', express.static('uploads'));
```

---

## API Security Best Practices

### 1. Use Helmet (Security Headers)

```bash
npm install helmet
```

```javascript
const helmet = require('helmet');
app.use(helmet());
```

### 2. Rate Limiting

```bash
npm install express-rate-limit
```

```javascript
const rateLimit = require('express-rate-limit');

const limiter = rateLimit({
  windowMs: 10 * 60 * 1000, // 10 minutes
  max: 100, // Limit each IP to 100 requests per windowMs
  message: 'Too many requests from this IP, please try again later'
});

app.use('/api/', limiter);
```

### 3. Prevent NoSQL Injection

```bash
npm install express-mongo-sanitize
```

```javascript
const mongoSanitize = require('express-mongo-sanitize');
app.use(mongoSanitize());
```

### 4. Prevent XSS Attacks

```bash
npm install xss-clean
```

```javascript
const xss = require('xss-clean');
app.use(xss());
```

### 5. Use HTTPS in Production

```javascript
// Redirect HTTP to HTTPS
if (process.env.NODE_ENV === 'production') {
  app.use((req, res, next) => {
    if (req.header('x-forwarded-proto') !== 'https') {
      res.redirect(`https://${req.header('host')}${req.url}`);
    } else {
      next();
    }
  });
}
```

### 6. Environment Variables

Never commit `.env` file. Add to `.gitignore`:

```
node_modules/
.env
uploads/
```

### 7. CORS Configuration

```javascript
const corsOptions = {
  origin: process.env.CLIENT_URL || 'http://localhost:3000',
  credentials: true,
  optionSuccessStatus: 200
};

app.use(cors(corsOptions));
```

---

## Testing Your API

### Using Postman

1. **Create Collection**: Organize your endpoints
2. **Set Environment Variables**: Store base URL and token
3. **Test Each Endpoint**:

**Register User (POST /api/auth/register)**
```json
{
  "name": "John Doe",
  "email": "john@example.com",
  "password": "password123"
}
```

**Login (POST /api/auth/login)**
```json
{
  "email": "john@example.com",
  "password": "password123"
}
```

Copy the token from response and use in Authorization header:
```
Authorization: Bearer YOUR_TOKEN_HERE
```

### Writing Automated Tests with Jest

```bash
npm install --save-dev jest supertest
```

```javascript
// tests/user.test.js
const request = require('supertest');
const app = require('../server');

describe('User API', () => {
  test('GET /api/users - should return all users', async () => {
    const response = await request(app).get('/api/users');
    expect(response.status).toBe(200);
    expect(response.body.success).toBe(true);
  });
  
  test('POST /api/users - should create new user', async () => {
    const newUser = {
      name: 'Test User',
      email: 'test@example.com',
      password: 'password123'
    };
    
    const response = await request(app)
      .post('/api/users')
      .send(newUser);
    
    expect(response.status).toBe(201);
    expect(response.body.data).toHaveProperty('_id');
  });
});
```

---

## Deployment

### Preparing for Deployment

```javascript
// server.js
const PORT = process.env.PORT || 5000;

// Set security headers
if (process.env.NODE_ENV === 'production') {
  app.use(helmet());
}

// Serve static assets in production
if (process.env.NODE_ENV === 'production') {
  app.use(express.static('client/build'));
  
  app.get('*', (req, res) => {
    res.sendFile(path.resolve(__dirname, 'client', 'build', 'index.html'));
  });
}
```

### Deploying to Render

1. Push code to GitHub
2. Create account on Render.com
3. Click "New" → "Web Service"
4. Connect GitHub repository
5. Configure:
   - **Build Command**: `npm install`
   - **Start Command**: `npm start`
   - **Environment Variables**: Add all from `.env`
6. Click "Create Web Service"

### Deploying to Railway

1. Install Railway CLI: `npm i -g @railway/cli`
2. Login: `railway login`
3. Initialize: `railway init`
4. Deploy: `railway up`
5. Add environment variables: `railway variables set KEY=value`

### Deploying to Heroku

```bash
# Install Heroku CLI
heroku login
heroku create your-app-name
git push heroku main
heroku config:set KEY=value
```

## 🤝 Contributing

Found an amazing resource? Want to add your favorite course? Check out our [CONTRIBUTING.md](../CONTRIBUTING.md) to learn how you can help grow this collection!

## 🔗 Connect With Me

[![Instagram](https://img.shields.io/badge/Instagram-E4405F?style=for-the-badge&logo=instagram&logoColor=white)](https://instagram.com/parth.builds)
[![GitHub](https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white)](https://github.com/parthnarkar)
[![LinkedIn](https://img.shields.io/badge/-LinkedIn-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://linkedin.com/in/parthnarkar)
[![LeetCode Profile](https://img.shields.io/badge/LeetCode-ParthNarkar-FFA116?style=for-the-badge&logo=leetcode)](https://leetcode.com/u/parthnarkar/)
[![Email](https://img.shields.io/badge/-Email-D14836?style=for-the-badge&logo=gmail&logoColor=white)](mailto:parthnarkarofficial@gmail.com)
[![Twitter](https://img.shields.io/badge/-Twitter-1DA1F2?style=for-the-badge&logo=twitter&logoColor=white)](https://twitter.com/parthnarkar)
[![Discord](https://img.shields.io/badge/-Discord-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.com/users/parth_narkar)

### ⭐ Found this helpful? Give this Repo a STAR!

[![parth-builds-os Github Repo Footer](https://github.com/user-attachments/assets/4bef3a04-16ee-4484-a52c-4f31182e1916)](https://github.com/parthnarkar/parth-builds-os)
