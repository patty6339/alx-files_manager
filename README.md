# alx-files_manager

### **How to Create an API with Express**
1. **Install Dependencies**: Install Express using `npm install express`.
2. **Setup Express App**:
   - Import Express: `const express = require('express');`
   - Create an app instance: `const app = express();`
3. **Define Routes**:
   - Use `app.get`, `app.post`, etc., to define endpoints:  
     ```javascript
     app.get('/api/endpoint', (req, res) => {
       res.send('Hello, API!');
     });
     ```
4. **Use Middleware**:
   - Parse JSON with `express.json()`.
   - Handle CORS with a package like `cors`.
5. **Start the Server**:
   - Call `app.listen(port, () => console.log(`Server running on port ${port}`));`.

---

### **How to Authenticate a User**
1. **Register Users**:
   - Store user credentials (e.g., email and hashed password) in a database.
   - Use a library like `bcrypt` to hash passwords.
2. **Login Flow**:
   - Verify credentials by checking the hashed password with `bcrypt.compare()`.
3. **Generate Token**:
   - Use JWT (`jsonwebtoken`) to generate tokens for authenticated users.
4. **Protect Routes**:
   - Verify tokens using a middleware:  
     ```javascript
     const jwt = require('jsonwebtoken');
     const authMiddleware = (req, res, next) => {
       const token = req.headers.authorization?.split(' ')[1];
       if (!token) return res.status(401).send('Unauthorized');
       jwt.verify(token, 'secret_key', (err, user) => {
         if (err) return res.status(403).send('Forbidden');
         req.user = user;
         next();
       });
     };
     ```

---

### **How to Store Data in MongoDB**
1. **Install Dependencies**:
   - Use `npm install mongoose` for MongoDB integration.
2. **Connect to MongoDB**:
   - Import Mongoose and connect:  
     ```javascript
     const mongoose = require('mongoose');
     mongoose.connect('mongodb://localhost:27017/dbname', {
       useNewUrlParser: true,
       useUnifiedTopology: true,
     });
     ```
3. **Define a Schema**:
   - Create a schema for your data:  
     ```javascript
     const schema = new mongoose.Schema({ field: String });
     const Model = mongoose.model('CollectionName', schema);
     ```
4. **Perform CRUD Operations**:
   - Insert data: `await Model.create({ field: 'value' });`
   - Retrieve data: `await Model.find({});`

---

### **How to Store Temporary Data in Redis**
1. **Install Redis Client**:
   - Use `npm install redis` to install the Redis client.
2. **Connect to Redis**:
   - Create a Redis client:  
     ```javascript
     const redis = require('redis');
     const client = redis.createClient();
     client.on('connect', () => console.log('Connected to Redis'));
     client.connect();
     ```
3. **Set and Get Data**:
   - Store data: `await client.set('key', 'value', { EX: 60 });` (expires in 60 seconds).
   - Retrieve data: `const value = await client.get('key');`

---

### **How to Setup and Use a Background Worker**
1. **Install Job Queue**:
   - Use a library like `bull` or `node-resque` for queue management.
2. **Connect to Redis**:
   - Background workers often use Redis for task management.
3. **Create a Job Queue**:
   - With `bull`:  
     ```javascript
     const Queue = require('bull');
     const queue = new Queue('jobQueue');
     ```
4. **Add Jobs**:
   - `queue.add({ data: 'job data' });`
5. **Process Jobs**:
   - Define a worker to process tasks:  
     ```javascript
     queue.process(async (job) => {
       console.log('Processing job:', job.data);
       // Task logic here
     });
     ```
6. **Monitor Queue**:
   - Use tools like Bull Dashboard for monitoring.