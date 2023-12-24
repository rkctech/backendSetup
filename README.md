# backendSetup
## file Stracture

backendSetup
- node_modules 
- public
- src
   - controllers
   - db
   - middlewares
   - modeles
   - routes
   - utils
   - app.js
   - constants.js
   - index.js
- .env
- .gitignore
- .prettierignore
- .prettierrc
- package-lock.json
- package.json
- README.md
## how to get this file stracture (follow step which is given below)
1. Create a fresh folder<br>

2. Create .gitignore file as well as Readme.md during initialisation of new repo. or we can create manually <br>
- In case of manually created .gitignore file
- To get content for .gitignore file from [gitignore Genrator](https://mrkandreev.name/snippets/gitignore-generator/)<br>
- To learn how to use Git as well how to write markdown file [visit](https://github.com/rkctech/MD-Git-Command-Guide)

3. Create package.json file
```powershell
node init
```

4. To create new folder like public & src we used this commond

```bash
mkdir folder_name_1, folder_name_2, folder_name_3
```
- After creating new folder you can check git status then you see that these empty folder not tracked so avaid to this problem you should create `.gitkeep `file in created new empty folders.
```bash
touch .gitkeep
```
- to create multiple file using touch command use 

```bash
touch .gitkeep file_1 file_2 file_3
```
5. dev dependency<br>
- nodemon (npm i -D nodemon)
- prettier(npm i -D prettier)

## node_modules & package-lock.json
Autogentrated (whenever we install anything by npm i)

## package.json file

Made some changes in package.json file "type":"module" and start script added like "start":"nodemon ./src/index.js"

## Prettier
now we need create two file .prettierrc for config and other one .prettierignore<br> 

 .prettierrc

```
{
    "singleQuote": false,
    "bracketSpacing": true,
    "tabWidth": 2,
    "trailingComma": "es5",
    "semi": true
}
```
.prettierignore file

```
/.vscode
/node_modules
./dist

*.env
.env
.env.*
```
## Connet with database 
Sign in at MongoDB Atlas [click](https://www.mongodb.com/atlas/database)

Data Service >> database >> connect >> compass <br>

- Copy the connection string, then open MongoDB Compass
```
mongodb+srv://rohitchaurasiya1830:<password>@cluster0.fqdmot9.mongodb.net/
```
Now back at vs code (.env)
```
PORT=8000
MONGODB_URI=mongodb+srv://hitesh:your-password@cluster0.lxl3fsq.mongodb.net
```
don't use ending slash from MONGODB_URI & replace your-password with database password 

now back src >> constants.js
```
export const DB_NAME = "videotube"
```
We need to install mongoose dotenv express
```
npm i mongoose dotenv express
```
### Approch-1
come back src >> index.js

```javascript
import dotenv from "dotenv"
import mongoose from "mongoose";
import { DB_NAME } from "../constants.js";
import express from "express"

dotenv.config({
    path: './.env'
})

const app = express()

( async () => {
    try {
        await mongoose.connect(`${process.env.MONGODB_URI}/${DB_NAME}`)
        app.on("errror", (error) => {
            console.log("ERRR: ", error);
            throw error
        })

        app.listen(process.env.PORT, () => {
            console.log(`App is listening on port ${process.env.PORT}`);
        })

    } catch (error) {
        console.error("ERROR: ", error)
        throw err
    }
})()
```
### Approch-2
go back db folder create index.js file
```javascript
import mongoose from "mongoose";
import { DB_NAME } from "../constants.js";


const connectDB = async () => {
    try {
        const connectionInstance = await mongoose.connect(`${process.env.MONGODB_URI}/${DB_NAME}`)
        console.log(`\n MongoDB connected !! DB HOST: ${connectionInstance.connection.host}`);
    } catch (error) {
        console.log("MONGODB connection FAILED ", error);
        process.exit(1)
    }
}

export default connectDB
```
src >> imdex.js
```javascript
// require('dotenv').config({path: './env'})
import dotenv from "dotenv"
import connectDB from "./db/index.js";

dotenv.config({
    path: './.env'
})

connectDB()
```
### key point
- whenever we talk with database then we can see some problem that's why we should always use try-catch or promise.

- Database is always in other continent that's why we should always used Async & await.

- Approch-1 we used 
```javascript
;()()
;(()=>{})()
;(async ()=>{})()
;(async ()=>{
    try{

    }catch(err){
        console.log(err)
    }
})()
//; use it just cleening purpose
// process.exit() // learn it

```
## basic setup for an Express.js application (src >> app.js)

### Step -1
terminal
``` 
 npm i cookie-parser cors 
```
### Step -2

backendSetup >> src >> app.js

```javascript
import express from "express"
import cors from "cors"
import cookieParser from "cookie-parser"

const app = express()

app.use(cors({
    origin: process.env.CORS_ORIGIN,
    credentials: true
}))

app.use(express.json({limit: "16kb"}))
app.use(express.urlencoded({extended: true, limit: "16kb"}))
app.use(express.static("public"))
app.use(cookieParser())

export { app }

```
This code is a basic setup for an Express.js application using some commonly used middleware. Let's break it down step by step:

1. **Importing Dependencies:**
   - `express`: This is the main library for building web applications with Node.js.
   - `cors`: Cross-Origin Resource Sharing middleware, which enables the server to handle requests from different origins.
   - `cookieParser`: Middleware for parsing cookies in the incoming request.

```javascript
import express from "express";
import cors from "cors";
import cookieParser from "cookie-parser";
```

2. **Create Express App:**
   - The `express()` function is called to create an instance of the Express application.

```javascript
const app = express();
```

3. [**Middleware:**](https://github.com/rkctech/Middleware)
   - `cors` middleware is used to handle Cross-Origin Resource Sharing. It is configured to allow requests from the origin specified in the `process.env.CORS_ORIGIN` environment variable. Additionally, `credentials` is set to `true` to allow cookies to be sent and received in cross-origin requests.

```javascript
app.use(cors({
    origin: process.env.CORS_ORIGIN,
    credentials: true
}));
```

   - `express.json()` middleware is used to parse incoming JSON requests. The `limit` option is set to "16kb" to limit the payload size to 16 kilobytes.

```javascript
app.use(express.json({ limit: "16kb" }));
```

   - `express.urlencoded()` middleware is used to parse incoming requests with URL-encoded payloads. The `extended` option is set to `true`, and the `limit` option is set to "16kb".

```javascript
app.use(express.urlencoded({ extended: true, limit: "16kb" }));
```

   - `express.static()` middleware is used to serve static files from the "public" directory.

```javascript
app.use(express.static("public"));
```

   - `cookieParser` middleware is used to parse cookies in the incoming request.

```javascript
app.use(cookieParser());
```

4. **Export the Express App:**
   - The created Express app is exported to be used in other parts of the application.

```javascript
export { app };
```

This code sets up a basic Express server with middleware for handling CORS, parsing JSON and URL-encoded data, serving static files, and parsing cookies. The configuration for CORS is taken from the `process.env.CORS_ORIGIN` environment variable, allowing you to define the allowed origin dynamically.

### Step -3

Now we need to add environment varible `CORS_ORIGIN` in `.env` file
```
CORS_ORIGIN = *
```
### Step -4

Make some changes in `src >> index.js` file

```javascript
connectDB()
  .then(() => {
    // Database connection successful
    app.listen(process.env.PORT || 8000, () => {
      console.log(`⚙️ Server is running at port : ${process.env.PORT || 8000}`);
    });
  })
  .catch((err) => {
    // Database connection failed
    console.log("MONGO db connection failed !!! ", err);
  });
```

1. **`connectDB()`:**
   - This function, named `connectDB`, is expected to be a promise that handles the connection to a MongoDB database. It is likely an asynchronous function that returns a promise.
   - The `connectDB` function is invoked, and it returns a promise. The subsequent `then` and `catch` blocks handle the resolution and rejection of this promise, respectively.

2. **Promise Handling:**
   - The `then` block is executed if the promise returned by `connectDB` is resolved successfully. Inside this block, the Express application is configured to listen for incoming requests.
   - The `catch` block is executed if there is an error in connecting to the MongoDB database. In this case, an error message is logged to the console.

3. **Express Server Initialization:**
   - Inside the `then` block, after a successful database connection, the Express server is started using the `app.listen()` method.
   - The server listens on the port specified by `process.env.PORT` or defaults to port 8000 if the environment variable is not set.

4. **Logging Server Information:**
   - A console log statement is used to print a message indicating that the server is running and specifying the port on which it is listening.


In summary, this code connects to a MongoDB database using the `connectDB` function, starts an Express server upon successful database connection, and logs relevant information. If there is an error in connecting to the database, an error message is logged to the console.

### Step -5
`src >> utils` let's make some files <br>
#### terminal
```bash
touch asyncHandler.js ApiError.js ApiResponse.js cloudinary.js
```
#### `asyncHandler.js`
```javascript
const asyncHandler = (requestHandler) => {
    return (req, res, next) => {
        Promise.resolve(requestHandler(req, res, next)).catch((err) => next(err))
    }
}


export { asyncHandler }

// const asyncHandler = () => {}
// const asyncHandler = (func) => () => {}
// const asyncHandler = (func) => async () => {}


// const asyncHandler = (fn) => async (req, res, next) => {
//     try {
//         await fn(req, res, next)
//     } catch (error) {
//         res.status(err.code || 500).json({
//             success: false,
//             message: err.message
//         })
//     }
// }

```
The `asyncHandler` function is a utility function used in the context of Express.js middleware to handle asynchronous operations and errors. It ensures that asynchronous route handlers can safely use `async/await` syntax and properly handle errors.

Here are the different forms of the `asyncHandler` function that you provided:

1. **Original `asyncHandler` Function:**
   ```javascript
   const asyncHandler = (requestHandler) => {
       return (req, res, next) => {
           Promise.resolve(requestHandler(req, res, next)).catch((err) => next(err))
       }
   };
   ```

   - This version of `asyncHandler` takes a `requestHandler` function as an argument.
   - It returns a new middleware function that wraps the `requestHandler` in a Promise and handles any asynchronous errors using `catch`. If an error occurs during the execution of `requestHandler`, it calls `next(err)` to pass the error to the Express error handling middleware.

2. **Alternate Forms (commented out in the code):**
   ```javascript
   // const asyncHandler = () => {}
   // const asyncHandler = (func) => () => {}
   // const asyncHandler = (func) => async () => {}
   ```

   - These are commented-out forms and do not contain any logic. They are just placeholders or examples of different possible implementations.

3. **Modified `asyncHandler` Function (commented out in the code):**
   ```javascript
   // const asyncHandler = (fn) => async (req, res, next) => {
   //     try {
   //         await fn(req, res, next)
   //     } catch (error) {
   //         res.status(err.code || 500).json({
   //             success: false,
   //             message: err.message
   //         })
   //     }
   // }
   ```

   - This version of `asyncHandler` is similar to the original one but includes a `try/catch` block inside the returned middleware function. It catches any errors thrown during the execution of `fn` and responds with an error JSON if an error occurs.

Usage example:

```javascript
// Using the original asyncHandler
const asyncRouteHandler = asyncHandler(async (req, res, next) => {
    // Asynchronous operations using await
    const result = await someAsyncFunction();
    res.json(result);
});

// Using the modified asyncHandler with try/catch
const asyncRouteHandlerWithTryCatch = asyncHandler(async (req, res, next) => {
    try {
        // Asynchronous operations using await
        const result = await someAsyncFunction();
        res.json(result);
    } catch (error) {
        res.status(error.code || 500).json({
            success: false,
            message: error.message
        });
    }
});
```

In summary, the `asyncHandler` function is a utility that simplifies the handling of asynchronous operations and errors within Express.js route handlers. It's designed to be used as middleware to wrap asynchronous route handlers. The commented-out forms show different possible implementations or variations.

#### `ApiError.js`

This code defines a custom error class named `ApiError` in JavaScript, specifically for handling errors in an API context. Let's break down the key components of this class:

```javascript
class ApiError extends Error {
    constructor(
        statusCode,
        message = "Something went wrong",
        errors = [],
        stack = ""
    ) {
        super(message);

        // Custom properties for the ApiError instance
        this.statusCode = statusCode;
        this.data = null; // Additional data (null by default)
        this.message = message;
        this.success = false; // Indicates that the operation was not successful
        this.errors = errors; // Array of error details

        // Capture stack trace if available, otherwise generate one
        if (stack) {
            this.stack = stack;
        } else {
            Error.captureStackTrace(this, this.constructor);
        }
    }
}

export { ApiError };
```

Explanation:

1. **Extending the `Error` Class:**
   - The `ApiError` class extends the built-in JavaScript `Error` class. This allows `ApiError` to inherit properties and methods from the base `Error` class.

2. **Constructor:**
   - The `constructor` method is called when a new instance of `ApiError` is created.
   - It takes four parameters:
      - `statusCode`: The HTTP status code to be associated with the error.
      - `message`: A human-readable error message, with a default value of "Something went wrong" if not provided.
      - `errors`: An array containing details about specific errors (e.g., validation errors).
      - `stack`: The stack trace associated with the error (if available).

3. **Setting Custom Properties:**
   - Custom properties are assigned to the `ApiError` instance:
      - `statusCode`: The HTTP status code associated with the error.
      - `data`: Additional data (set to `null` by default).
      - `message`: The error message.
      - `success`: A boolean indicating that the operation was not successful (set to `false`).
      - `errors`: An array containing details about specific errors.

4. **Capturing Stack Trace:**
   - If a `stack` parameter is provided, it is used as the stack trace. Otherwise, `Error.captureStackTrace` is called to capture the stack trace associated with the instance.

5. **Exporting the `ApiError` Class:**
   - The `ApiError` class is exported so that it can be used in other parts of the application.

Usage Example:

```javascript
import { ApiError } from './path/to/ApiError';

// Creating an instance of ApiError
const apiError = new ApiError(404, "Resource not found", [{ field: "id", message: "Invalid ID" }]);

// Logging the properties of the ApiError instance
console.log(apiError.statusCode); // 404
console.log(apiError.message); // Resource not found
console.log(apiError.errors); // [{ field: "id", message: "Invalid ID" }]
console.log(apiError.success); // false
console.log(apiError.stack); // Stack trace
```

In summary, the `ApiError` class is designed to represent errors in an API context, allowing customization of error details, status codes, and additional data. Instances of this class can be used to communicate detailed error information in a standardized way.

#### ` ApiResponse.js`

This code defines a class named `ApiResponse` in JavaScript, which is used to structure and represent responses from an API. Let's break down the key components of this class:

```javascript
class ApiResponse {
    constructor(statusCode, data, message = "Success") {
        this.statusCode = statusCode;
        this.data = data;
        this.message = message;
        this.success = statusCode < 400;
    }
}

export { ApiResponse };
```

Explanation:

1. **Constructor:**
   - The `constructor` method is called when a new instance of `ApiResponse` is created.
   - It takes three parameters:
      - `statusCode`: The HTTP status code to be associated with the response.
      - `data`: The actual data or payload of the response.
      - `message`: A human-readable message associated with the response, with a default value of "Success" if not provided.

2. **Setting Properties:**
   - Properties are assigned to the `ApiResponse` instance:
      - `statusCode`: The HTTP status code associated with the response.
      - `data`: The actual data or payload of the response.
      - `message`: A human-readable message associated with the response.
      - `success`: A boolean indicating whether the response is considered successful. It is `true` if the status code is less than 400 (indicating success) and `false` otherwise.

3. **Exporting the `ApiResponse` Class:**
   - The `ApiResponse` class is exported so that it can be used in other parts of the application.

Usage Example:

```javascript
import { ApiResponse } from './path/to/ApiResponse';

// Creating an instance of ApiResponse for a successful response
const successResponse = new ApiResponse(200, { message: "Hello, World!" });

// Creating an instance of ApiResponse for an error response
const errorResponse = new ApiResponse(404, null, "Resource not found");

// Logging the properties of the ApiResponse instances
console.log(successResponse.statusCode); // 200
console.log(successResponse.data); // { message: "Hello, World!" }
console.log(successResponse.message); // Success
console.log(successResponse.success); // true

console.log(errorResponse.statusCode); // 404
console.log(errorResponse.data); // null
console.log(errorResponse.message); // Resource not found
console.log(errorResponse.success); // false
```

In summary, the `ApiResponse` class is designed to structure API responses with properties such as status code, data, message, and a boolean indicating success. Instances of this class can be used to consistently format responses in a standardized way throughout an API.


## Creating Models

### User Schema

Here's the code:

```javascript
import mongoose, { Schema } from "mongoose";
import jwt from "jsonwebtoken";
import bcrypt from "bcrypt";

const userSchema = new Schema(
  {
    username: {
      type: String,
      required: true,
      unique: true,
      lowercase: true,
      trim: true,
      index: true,
    },
    email: {
      type: String,
      required: true,
      unique: true,
      lowercase: true,
      trim: true,
    },
    fullName: {
      type: String,
      required: true,
      trim: true,
      index: true,
    },
    avatar: {
      type: String, // cloudinary url
      required: true,
    },
    coverImage: {
      type: String, // cloudinary url
    },
    watchHistory: [
      {
        type: Schema.Types.ObjectId,
        ref: "Video",
      },
    ],
    password: {
      type: String,
      required: [true, "Password is required"],
    },
    refreshToken: {
      type: String,
    },
  },
  {
    timestamps: true,
  }
);

userSchema.pre("save", async function (next) {
  if (!this.isModified("password")) return next();

  this.password = await bcrypt.hash(this.password, 10);
  next();
});

userSchema.methods.isPasswordCorrect = async function (password) {
  return await bcrypt.compare(password, this.password);
};

userSchema.methods.generateAccessToken = function () {
  return jwt.sign(
    {
      _id: this._id,
      email: this.email,
      username: this.username,
      fullName: this.fullName,
    },
    process.env.ACCESS_TOKEN_SECRET,
    {
      expiresIn: process.env.ACCESS_TOKEN_EXPIRY,
    }
  );
};

userSchema.methods.generateRefreshToken = function () {
  return jwt.sign(
    {
      _id: this._id,
    },
    process.env.REFRESH_TOKEN_SECRET,
    {
      expiresIn: process.env.REFRESH_TOKEN_EXPIRY,
    }
  );
};

export const User = mongoose.model("User", userSchema);
```

This code defines a Mongoose schema for a user in a MongoDB database. It includes fields such as `username`, `email`, `fullName`, `avatar`, `coverImage`, `watchHistory`, `password`, and `refreshToken`. Additionally, there are methods for password hashing, password validation, and generating JWT tokens for authentication.

Let's break down the key components:

1. **User Schema:**
   - The `userSchema` defines the structure of the user document with various fields and their types. These include `username`, `email`, `fullName`, `avatar`, `coverImage`, `watchHistory`, `password`, and `refreshToken`.
   - `timestamps: true` is set to automatically add `createdAt` and `updatedAt` fields.

2. **Pre-Save Middleware:**
   - A pre-save middleware is defined using `userSchema.pre("save", ...)`. This middleware hashes the password using bcrypt before saving the user to the database. It only hashes the password if it has been modified.

3. **Password Validation Method:**
   - The `isPasswordCorrect` method is defined to compare a provided password with the hashed password stored in the database. It uses bcrypt's `compare` method.

4. **JWT Token Generation Methods:**
   - Two methods, `generateAccessToken` and `generateRefreshToken`, are defined for generating JWT tokens for authentication. They use the `jwt.sign` method and include specific user information in the token payload.

5. **Mongoose Model:**
   - The Mongoose model `User` is created based on the `userSchema`. This model is then exported for use in other parts of the application.



Ensure that the necessary environment variables (`ACCESS_TOKEN_SECRET`, `ACCESS_TOKEN_EXPIRY`, `REFRESH_TOKEN_SECRET`, `REFRESH_TOKEN_EXPIRY`) are properly set in your application.

### Video Schema
This code defines a Mongoose schema for storing video-related information in a MongoDB database. It includes fields such as `videoFile`, `thumbnail`, `title`, `description`, `duration`, `views`, `isPublished`, and `owner`. Additionally, it utilizes a plugin called `mongoose-aggregate-paginate-v2` for pagination capabilities.

Let's break down the code:

```javascript
import mongoose, { Schema } from "mongoose";
import mongooseAggregatePaginate from "mongoose-aggregate-paginate-v2";

const videoSchema = new Schema(
  {
    videoFile: {
      type: String, // Cloudinary URL
      required: true,
    },
    thumbnail: {
      type: String, // Cloudinary URL
      required: true,
    },
    title: {
      type: String,
      required: true,
    },
    description: {
      type: String,
      required: true,
    },
    duration: {
      type: Number,
      required: true,
    },
    views: {
      type: Number,
      default: 0,
    },
    isPublished: {
      type: Boolean,
      default: true,
    },
    owner: {
      type: Schema.Types.ObjectId,
      ref: "User",
    },
  },
  {
    timestamps: true,
  }
);

// Plugin for pagination support
videoSchema.plugin(mongooseAggregatePaginate);

export const Video = mongoose.model("Video", videoSchema);
```

Explanation:

1. **Video Schema:**
   - The `videoSchema` defines the structure of a video document with fields like `videoFile`, `thumbnail`, `title`, `description`, `duration`, `views`, `isPublished`, and `owner`.
   - `timestamps: true` is set to automatically add `createdAt` and `updatedAt` fields.

2. **Pagination Plugin:**
   - The `mongoose-aggregate-paginate-v2` plugin is applied to the `videoSchema` using `videoSchema.plugin(mongooseAggregatePaginate)`. This plugin adds pagination capabilities to queries involving this schema.

3. **Mongoose Model:**
   - The Mongoose model `Video` is created based on the `videoSchema`. This model is then exported for use in other parts of the application.

This schema is structured to store video-related data such as video files, thumbnails, titles, descriptions, durations, views, publication status, and references to the video owner. The pagination plugin enhances querying capabilities by enabling paginated results when working with this schema in MongoDB through Mongoose.

### Uploading file 
- Express file upload 
- multer middlewar with cloudinary (now handle by it)

#### Step - 1
src >> util

terminal
```
npm i cloudinary multer
```

#### `cloudinary.js`
This code defines a function named `uploadOnCloudinary` that is responsible for uploading a file to the Cloudinary cloud storage service. Here's a breakdown of the code:

```javascript
import { v2 as cloudinary } from "cloudinary";
import fs from "fs";

// Configure Cloudinary using environment variables
cloudinary.config({
  cloud_name: process.env.CLOUDINARY_CLOUD_NAME,
  api_key: process.env.CLOUDINARY_API_KEY,
  api_secret: process.env.CLOUDINARY_API_SECRET,
});

// Define the function for uploading a file to Cloudinary
const uploadOnCloudinary = async (localFilePath) => {
  try {
    // Check if the localFilePath is provided
    if (!localFilePath) return null;

    // Upload the file on Cloudinary
    const response = await cloudinary.uploader.upload(localFilePath, {
      resource_type: "auto",
    });

    // File has been uploaded successfully
    // Remove the locally saved temporary file
    fs.unlinkSync(localFilePath);

    // Return the Cloudinary response
    return response;
  } catch (error) {
    // Remove the locally saved temporary file as the upload operation failed
    fs.unlinkSync(localFilePath);
    return null;
  }
};

// Export the uploadOnCloudinary function
export { uploadOnCloudinary };
```

Explanation:

1. **Cloudinary Configuration:**
   - The code imports the Cloudinary v2 SDK (as `v2`) and configures it using the provided environment variables (`CLOUDINARY_CLOUD_NAME`, `CLOUDINARY_API_KEY`, and `CLOUDINARY_API_SECRET`).

2. **`uploadOnCloudinary` Function:**
   - This function takes a local file path (`localFilePath`) as an argument.
   - Inside the function:
     - It checks if the `localFilePath` is provided. If not, it returns `null`.
     - It uses the `cloudinary.uploader.upload` method to upload the file to Cloudinary. The `resource_type: "auto"` option is set to automatically determine the resource type.
     - If the upload is successful:
       - The locally saved temporary file is removed using `fs.unlinkSync`.
       - The Cloudinary response is returned.
     - If there is an error during the upload:
       - The locally saved temporary file is still removed.
       - `null` is returned.

3. **Exporting the Function:**
   - The `uploadOnCloudinary` function is exported so that it can be used in other parts of the application.

Usage Example:

```javascript
import { uploadOnCloudinary } from './path/to/uploadOnCloudinary';

const localFilePath = 'path/to/local/file.jpg';

// Upload the file to Cloudinary
const cloudinaryResponse = await uploadOnCloudinary(localFilePath);

if (cloudinaryResponse) {
  console.log('File uploaded to Cloudinary:', cloudinaryResponse.url);
} else {
  console.log('File upload to Cloudinary failed.');
}
```

This code provides a reusable function for uploading files to Cloudinary and handles cleanup (removing the local temporary file) in case of success or failure.

#### Step - 2
src >> middlewars >> multer.middlewar.js<br>

[Multer docomentation](https://www.npmjs.com/package/multer)

 ```javascript
 import multer from "multer";

const storage = multer.diskStorage({
    destination: function (req, file, cb) {
      cb(null, "./public/temp")
    },
    filename: function (req, file, cb) {
      
      cb(null, file.originalname)
    }
  })
  
export const upload = multer({ 
    storage, 
})
 ```
 basic setup for handling file uploads using the `multer` middleware in a Node.js application. Let me break down the code for you:

1. `multer` is a popular middleware for handling `multipart/form-data`, which is commonly used for file uploads.

2. `multer.diskStorage` is used to configure the storage engine for uploaded files. In your example:
   - `destination`: Specifies the destination directory where the uploaded files will be stored. In this case, it's set to "./public/temp".
   - `filename`: Specifies the name of the uploaded file. Here, it's set to the original name of the file.

3. The configuration object (`storage`) is then passed to `multer` to create the middleware.

4. `upload` is an instance of the `multer` middleware configured with the storage settings.

Here's a brief explanation of the properties you've set in the `multer.diskStorage`:

- `destination`: The directory where uploaded files will be stored. In this case, it's "./public/temp".
- `filename`: The function that determines the name of the uploaded file. In this case, it uses the original name of the file.

You can use this `upload` middleware in your route handlers to process file uploads. For example, if you have an endpoint that accepts file uploads, you can use it like this:

```javascript
import express from "express";

const app = express();

// Use the 'upload' middleware for handling file uploads
app.post("/upload", upload.single("file"), (req, res) => {
  // req.file contains information about the uploaded file
  res.send("File uploaded successfully!");
});

// Start the server
const PORT = 3000;
app.listen(PORT, () => {
  console.log(`Server is running on port ${PORT}`);
});
```

In this example, the `upload.single("file")` middleware is used to handle a single file upload. The uploaded file information is then available in `req.file` for further processing. Adjust the route and handling based on your specific use case.

### Using Router and Controller for User Registration

#### Step 1: Create User Controller
In the `src/controllers/user.controller.js` file, define the user controller for handling user registration.

```javascript
// src/controllers/user.controller.js
import { asyncHandler } from "../utils/asyncHandler.js";

/**
 * @desc    Register a new user
 * @route   POST /api/users/register
 * @access  Public
 */
const registerUser = asyncHandler(async (req, res) => {
  // Add your registration logic here
  res.status(200).json({
    message: "User registered successfully",
  });
});

export { registerUser };
```

#### Step 2: Set Up User Routes
In the `src/routes/user.routes.js` file, create routes using Express Router and link them to the corresponding controllers.

```javascript
// src/routes/user.routes.js
import { Router } from "express";
import { registerUser } from "../controllers/user.controller.js";

const router = Router();

// @desc    Register a new user
// @route   POST /api/users/register
// @access  Public
router.route("/register").post(registerUser);

export default router;
```

#### Step 3: Integrate Routes into Main App
In your main application file (e.g., `src/app.js`), integrate the user routes into the express app.

```javascript
// src/app.js
import express from "express"
import cors from "cors"
import cookieParser from "cookie-parser"

const app = express()

app.use(cors({
    origin: process.env.CORS_ORIGIN,
    credentials: true
}))

app.use(express.json({limit: "16kb"}))
app.use(express.urlencoded({extended: true, limit: "16kb"}))
app.use(express.static("public"))
app.use(cookieParser())


//routes import
import userRouter from './routes/user.routes.js'


//routes declaration
app.use("/api/v1/users", userRouter)

// http://localhost:8000/api/v1/users/register


export { app }

```

#### step -4 
**API testing **
ways 
- Thunder client (VS code plugin)
- [Postman download](https://www.postman.com/downloads/) (open it >>My Workspace>> collections)
  - click om + icon
  - select POST 
  - Put URL
  - Send















