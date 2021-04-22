# CodeTutor

## 1 - Running Project by Docker

### 1.1 - Build images
```bash
docker build -t code-tutor:2021.1.0 .
````

### 1.2 - Run container
```bash
docker run -v ${PWD}:/app -v /app/node_modules -p 4201:4200 --rm code-tutor:dev
```

### 1.3 - Run docker compose on dev
```bash
docker-compose up -d --build
```

### 1.4 - Run docker compose files on production
```bash
docker-compose -f docker-compose-prod.yml up -d --build
```

## 2 - Configure Angular App to Deploy Properly on Heroku

### 2.1 - Ensure you have the latest version of angular CLI and angular compiler cli:
```bash
npm install @angular/cli@latest @angular/compiler-cli --save-de
```

### 2.2 - Copy below dependencies from devDependencies to dependencies

_Note: check the correct version against that created application_

```bash
"@angular/cli": "^11.0.2"
"@angular/compiler-cli": "^10.0.14"
"typescript": "~3.9.5"
````

### 2.3 - Create heroku-postbuild script in package.json. Under “scripts”, add a “heroku-postbuild” command like so
```bash
"heroku-postbuild": "ng build --prod"
```

### 2.4 - Add Node and NPM engines
```bash
"engines": {
"node": "12.18.2",
"npm": "6.14.5"
}
```

### 2.5 - Install Server to run your app
```bash
npm install express path --save
```

### 2.6 - Create a server.js file in the root of the application and paste the following code.
```bash
//Install express server
const express = require('express');
const path = require('path');

const app = express();

// Serve only the static files form the dist directory
app.use(express.static('./dist/angular-app-heroku'));

app.get('/*', (req, res) =>
res.sendFile('index.html', {root: 'dist/angular-app-heroku/'}),
);

// Start the app by listening on the default Heroku port
app.listen(process.env.PORT || 8080);
```

### 2.7 - Change start command In package.json, change the “start” command to node server.js so it becomes
```bash
"start": "node server.js"
```
