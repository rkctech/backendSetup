# backendSetup
I used to create .gitignore file from gitignore Genrator
- [gitignore Genrator](https://mrkandreev.name/snippets/gitignore-generator/)<br>
- Second way to create .gitignore file as well as Readme.md during initialisation of new repo.<br>

Whenever we create new folder like public src we used this commond (git should initilise in your root directory)
```bash
mkdir folder_name_1, folder_name_2, folder_name_3
```
- After creating new folder you can check git status then you see that these empty folder not tracked so avaid to this problem you should create `.gitkeep `file in created new empty folders.
```bash
touch .gitkeep
```
- to create multiple file using touch command use this
```bash
touch .gitkeep file_1 file_2 file_3
```
Made some changes in package.json file "type":"module" and start script added like "start":"nodemon ./src/index.js"

dev dependency<br>
- nodemon (npm i -D nodemon)
- prettier(npm i -D prettier)

file Stracture

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
- .gitignore
- .prettierignore
- .prettierrc
- package-lock.json
- package.json
- README.md




