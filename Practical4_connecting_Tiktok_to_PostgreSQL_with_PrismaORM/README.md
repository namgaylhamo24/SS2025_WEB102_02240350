**Note:** This folder only contains the `README.md` and `Reflection.md` files. The actual implementation of the practical work can be found in the `Practical2_API_Design_and_Implementation_Tiktok` folder.

## Objectives
- Set up a PostgreSQL database for our TikTok clone application
- Configure Prisma ORM to interact with the database
- Migrate from in-memory data models to persistent database storage
- Implement authentication with password encryption
- Update our RESTful API endpoints to use the database

---

## Part 1: Setting Up PostgreSQL Database

### Step 1: Create a Database
1. Access the PostgreSQL command line:
```bash
sudo -u postgres psql
```

2. Create a new database:
```sql
CREATE DATABASE tiktok_db;
```

3. Create a user for our application:
```sql
CREATE USER tiktok_user WITH ENCRYPTED PASSWORD 'your_password';
GRANT ALL PRIVILEGES ON DATABASE tiktok_db TO tiktok_user;
```

4. Exit the PostgreSQL command line:
```sql
\q
```

---

## Part 2: Setting Up Prisma ORM

### Step 1: Install Prisma Dependencies
Navigate to your server project and install Prisma:
```bash
cd server
npm install @prisma/client
npm install prisma --save-dev
```

### Step 2: Initialize Prisma
```bash
npx prisma init
```
This creates:
- A prisma directory with a schema.prisma file
- A .env file for environment variables

### Step 3: Configure Database Connection
Edit the .env file to include your database connection string:
```env
DATABASE_URL="postgresql://tiktok_user:your_password@localhost:5432/tiktok_db?schema=public"
```

### Step 4: Define Prisma Schema
Replace the contents of prisma/schema.prisma with the schema definition that matches our TikTok data model.

### Step 5: Install Additional Dependencies
```bash
npm install bcrypt jsonwebtoken
```

---

## Part 3: Creating the Database Schema with Prisma

### Step 1: Create Prisma Migration
```bash
npx prisma migrate dev --name init
```
This command:
1. Creates SQL migration files in the prisma/migrations directory  
2. Applies the migration to your database  
3. Generates the Prisma Client  

### Step 2: Create Prisma Client Instance
Create a new file at src/lib/prisma.js:
```javascript
const { PrismaClient } = require('@prisma/client');

const prisma = new PrismaClient();

module.exports = prisma;
```

---

## Part 4: Update Controllers to Use Prisma

Refer here for updated codes for Part 4, 5 and 7:
https://github.com/syangche/TikTok_Server.git

### Step 1: Update User Controller
Update src/controllers/userController.js to use Prisma to interact with the database.

Key concepts:
- Password hashing using bcrypt
- JWT token generation for authentication
- Database queries using Prisma Client

### Step 2: Update Video Controller
Update src/controllers/videoController.js.

Key concepts:
- Complex queries with relationships
- Transactions for operations that affect multiple tables
- Using Prisma's count and aggregation features

### Step 3: Update Comment Controller
Update src/controllers/commentController.js.

### Step 4: Update app.js to Register Routes

---

## Part 5: Implementing Authentication

### Step 1: Create Authentication Middleware
Create src/middleware/auth.js with the provided code that handles JWT token verification.

### Step 2: Update Routes to Use Authentication
Update your route files to use the authentication middleware for protected routes.

### Step 3: Configure Environment Variables
```env
# Server settings
PORT=5000  
NODE_ENV=development  

# Database settings
DATABASE_URL="postgresql://tiktok_user.your_password@localhost:5432/tiktok_db?schema=public"

# JWT settings
JWT_SECRET=yourverylongandsecurerandomsecret  
JWT_EXPIRE=30d
```

---

## Part 6: Testing Your Database Integration

### Step 1: Start the Server  
```bash
npm run dev
```

### Step 2: Test Registration and Login  
Use Postman to test:
- Register a new user  
- Login  

### Step 3: Test Protected Routes  
Use the token from login to test protected routes:
- Create a video (protected route)  

---

## Part 7: Creating Test Data  

### Step 1: Create a seed file  
Create a new file at prisma/seed.js  

### Step 2: Add a Script to package.json  
Add a seed script to your package.json file:
```json
"scripts": {  
    "dev": "nodemon src/index.js",  
    "start": "node src/index.js",  
    "seed": "node prisma/seed.js" 
}
```

### Step 3: Install Required Dependencies
```bash
npm install bcrypt
```

### Step 4: Run the Seed Script
```bash
npm run seed
```
This will populate your database with:
- 10 users
- 50 videos (5 per user)
- 200 comments
- 300 video likes
- 150 comment likes
- 40 follow relationships

### Part 6: Test Again
Postman Guide:
https://docs.google.com/document/d/1QInYRUqXZYWUi5AksoGOQYFqaT7
1KYJ6wiDU03y40FK/edit?usp=sharing

---

## Key Concepts

### Database Schema Design
- **Tables**: Represent entities like users, videos, comments  
- **Relationships**: Connections between tables (one-to-many, many-to-many)  
- **Indexes**: Improve query performance  
- **Foreign Keys**: Maintain data integrity  

### Object-Relational Mapping (ORM)
- Maps database tables to programming objects  
- Simplifies database operations with type safety  
- Reduces boilerplate SQL code  
- Handles database migrations  

### Authentication and Security
- **Password Hashing**: Never store plain-text passwords  
- **JWT Tokens**: Secure, stateless authentication  
- **Protected Routes**: Middleware to secure endpoints  

### Prisma Specific Features
- **Model Definitions**: Define data structure in schema.prisma  
- **Migrations**: Version control for database schema  
- **Relationships**: Define connections between models  
- **Transactions**: Ensure data consistency across operations  

## Resources
- Prisma Documentation  
- PostgreSQL Documentation  
- JWT Authentication
```