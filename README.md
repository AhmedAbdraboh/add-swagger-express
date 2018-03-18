# Integrate Swagger-UI with Express Application
This is a simple tutorial to document Your Express App using Swagger-UI and Swagger-JSDoc. 

## Getting Started

These instructions will make you use Swagger-JSDoc to document your code incliuding adding JWT tokens to your app. 
The official way is to download Swagger repo and add the dist folder to your public folder. 
Here, I will use [Swagger-ui-express]. Then, I will use [Swagger-jsdoc] package to go through all your routes and read any documentation written above the routes.


### Prerequisites

NodeJS 

npm installed

Your already made Express Application

### Installing

```
npm install swagger-ui-express swagger-jsdoc --save

```

in your root folder add [swagger.json] file with the following configs: 

```


{
    "info": {
        "title": "Node Swagger API",
        "version": "1.0.0",
        "description": "Demonstrating how to describe a RESTful API with Swagger"
    },
    "host": "localhost:3000",
    "basePath": "/api",
    "securityDefinitions": {
        "bearerAuth": {
            "type": "apiKey",
            "name": "Authorization",
            "scheme": "bearer",
            "bearerFormat": "JWT"
        }
    },
    "security": [
        {
            "bearerAuth": []
        }
    ]
}
```

here I am using JWT authentication type. 

in your [app.js] add the following code: 

```
const swaggerUi = require('swagger-ui-express');
const swaggerDocument = require('./swagger.json');
const swaggerJSDoc = require('swagger-jsdoc');

// swagger definition
var swaggerDefinition = require('./swagger.json')
// options for the swagger docs
var options = {
  // import swaggerDefinitions
  swaggerDefinition: swaggerDefinition,
  // path to the API docs
  apis: ['./routes/*.js'],// pass all in array, here I am collecting all my routes in a folder and getting them all
  };

const swaggerSpec = swaggerJSDoc(options);

app.use('/api-docs', swaggerUi.serve, swaggerUi.setup(swaggerSpec));

```

### A simple User Model Docs will be as this: 

add this comment just before the GET all users API in your userRoutes.js

```
/**
 * @swagger
 * /user:
 *   get:
 *     tags:
 *       - users
 *     description: Returns all users
 *     produces:
 *       - application/json
 *     responses:
 *       200:
 *         description: An array of users
 *         schema:
 *           $ref: '#/definitions/users'
 */
```

you can refer to any definitions and in my case I am using this reference code in routes/index.js

```
/**
 * @swagger
 * definition:
 *   users:
 *     properties:
 *       name:
 *         type: string
 *       email:
 *         type: string
 *       age:
 *         type: integer
 *       sex:
 *         type: string
 */
 
```

Start Your app 

Go to http://localhost:3000/api-docs/

click [Authorize] Button to add your JWT and execute the get all users request
