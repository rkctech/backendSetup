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
- .gitignore
- .prettierignore
- .prettierrc
- package-lock.json
- package.json
- README.md
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






