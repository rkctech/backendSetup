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








