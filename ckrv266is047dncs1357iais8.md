# Getting started with Prisma ORM for Nodejs

Many things in the tech sound intimidating when you first learn of them than they actually are. ORMs (Object Relational Mappers) are just one of them; we also have quite a number of options for any Nodejs application but I'll share some incites into getting started with Prisma ORM.

## What are ORMs?
Object-relational mapping conceptualizes the idea of being able to write queries like the one below, as well as much more complicated ones, using the object-oriented paradigm of your preferred programming language.
Essentially, ORMs help abstract the complexities of writing custom SQL queries to a database. 


```
SELECT * FROM user where countryId = 2
``` 

Well, just like any other tool or library, we have pros and cons to using them. For the most part, I stumbled on  [Prisma](prisma.io), and it's arguably an excellent tool for low to medium-level complexity and enterprise apps. 
- You easily scaffold a Nodejs server with a database quickly with the least amount of code.
- It's open-source,  it's compatible with a number of SQL such as PostgreSQL, MySQL, and SQLite.  
- Out of the box support for typescript.
- Many tools aren't properly documented and we have to rely on the community for help when stuck, but  [Prisma](https://www.prisma.io/docs/)  documentation is quite delightful and very detailed. 

Next up we will look at some prerequisites for this tutorial, featuring Prisma, Express.js + typescript, and PostgreSQL.

## Prerequisites
This tutorial assumes you've had some experience with Nodejs ;
- You have the Nodejs LTS installed
- Scaffold an Expressjs app 
- Basic knowledge of Javascript/typescript 
- Some idea of how ORMs interact with databases 

## Implementation
### Lets set Up the Express.js app

 - We’ll begin by setting up the Express.js app for a  Movies listing   API demo project. 
Navigate into a new folder First, then run the following to create a new project:

we also assume you're using a Unix shell/terminal

```
mkdir movies-listing-api
cd movies-listing-api
npm init -y 
 ```

- Next, install Express.js, our minimalistic framework for the Star Wars REST API:

```
npm install --save express
```

- Next, setup typescript dev dependencies

```
npm install typescript --save-dev
npm install @types/node --save-dev

```
- Next, Create a tsconfig.json file
The tsconfig.json is where we define the TypeScript compiler options. We can create a tsconfig with several options sets. it should be at the root directory of the project.

```
npx tsc --init --rootDir src --outDir build \
--esModuleInterop --resolveJsonModule --lib es6 \
--module commonjs --allowJs true --noImplicitAny true
```
At this point, you should have a tsconfig.json that looks like this:

```
{
  "compilerOptions": {
    /* Basic Options */
    // "incremental": true,                   /* Enable incremental compilation */
    "target": "es5",                          /* Specify ECMAScript target version: 'ES3' (default), 'ES5', 'ES2015', 'ES2016', 'ES2017', 'ES2018', 'ES2019' or 'ESNEXT'. */
    "module": "commonjs",                     /* Specify module code generation: 'none', 'commonjs', 'amd', 'system', 'umd', 'es2015', or 'ESNext'. */
    "lib": ["es6"],                     /* Specify library files to be included in the compilation. */
    "allowJs": true,                          /* Allow javascript files to be compiled. */
    // "checkJs": true,                       /* Report errors in .js files. */
    // "jsx": "preserve",                     /* Specify JSX code generation: 'preserve', 'react-native', or 'react'. */
    // "declaration": true,                   /* Generates corresponding '.d.ts' file. */
    // "declarationMap": true,                /* Generates a sourcemap for each corresponding '.d.ts' file. */
    // "sourceMap": true,                     /* Generates corresponding '.map' file. */
    // "outFile": "./",                       /* Concatenate and emit output to single file. */
    "outDir": "build",                          /* Redirect output structure to the directory. */
    "rootDir": "src",                         /* Specify the root directory of input files. Use to control the output directory structure with --outDir. */
    // "composite": true,                     /* Enable project compilation */
    // "tsBuildInfoFile": "./",               /* Specify file to store incremental compilation information */
    // "removeComments": true,                /* Do not emit comments to output. */
    // "noEmit": true,                        /* Do not emit outputs. */
    // "importHelpers": true,                 /* Import emit helpers from 'tslib'. */
    // "downlevelIteration": true,            /* Provide full support for iterables in 'for-of', spread, and destructuring when targeting 'ES5' or 'ES3'. */
    // "isolatedModules": true,               /* Transpile each file as a separate module (similar to 'ts.transpileModule'). */

    /* Strict Type-Checking Options */
    "strict": true,                           /* Enable all strict type-checking options. */
    "noImplicitAny": true,                    /* Raise error on expressions and declarations with an implied 'any' type. */
    // "strictNullChecks": true,              /* Enable strict null checks. */
    // "strictFunctionTypes": true,           /* Enable strict checking of function types. */
    // "strictBindCallApply": true,           /* Enable strict 'bind', 'call', and 'apply' methods on functions. */
    // "strictPropertyInitialization": true,  /* Enable strict checking of property initialization in classes. */
    // "noImplicitThis": true,                /* Raise error on 'this' expressions with an implied 'any' type. */
    // "alwaysStrict": true,                  /* Parse in strict mode and emit "use strict" for each source file. */

    /* Additional Checks */
    // "noUnusedLocals": true,                /* Report errors on unused locals. */
    // "noUnusedParameters": true,            /* Report errors on unused parameters. */
    // "noImplicitReturns": true,             /* Report error when not all code paths in function return a value. */
    // "noFallthroughCasesInSwitch": true,    /* Report errors for fallthrough cases in switch statement. */

    /* Module Resolution Options */
    // "moduleResolution": "node",            /* Specify module resolution strategy: 'node' (Node.js) or 'classic' (TypeScript pre-1.6). */
    // "baseUrl": "./",                       /* Base directory to resolve non-absolute module names. */
    // "paths": {},                           /* A series of entries which re-map imports to lookup locations relative to the 'baseUrl'. */
    // "rootDirs": [],                        /* List of root folders whose combined content represents the structure of the project at runtime. */
    // "typeRoots": [],                       /* List of folders to include type definitions from. */
    // "types": [],                           /* Type declaration files to be included in compilation. */
    // "allowSyntheticDefaultImports": true,  /* Allow default imports from modules with no default export. This does not affect code emit, just typechecking. */
    "esModuleInterop": true,                  /* Enables emit interoperability between CommonJS and ES Modules via creation of namespace objects for all imports. Implies 'allowSyntheticDefaultImports'. */
    // "preserveSymlinks": true,              /* Do not resolve the real path of symlinks. */
    // "allowUmdGlobalAccess": true,          /* Allow accessing UMD globals from modules. */

    /* Source Map Options */
    // "sourceRoot": "",                      /* Specify the location where debugger should locate TypeScript files instead of source locations. */
    // "mapRoot": "",                         /* Specify the location where debugger should locate map files instead of generated locations. */
    // "inlineSourceMap": true,               /* Emit a single file with source maps instead of having a separate file. */
    // "inlineSources": true,                 /* Emit the source alongside the sourcemaps within a single file; requires '--inlineSourceMap' or '--sourceMap' to be set. */

    /* Experimental Options */
    // "experimentalDecorators": true,        /* Enables experimental support for ES7 decorators. */
    // "emitDecoratorMetadata": true,         /* Enables experimental support for emitting type metadata for decorators. */

    /* Advanced Options */
    "resolveJsonModule": true                 /* Include modules imported with '.json' extension */
  }
}
```
- Next, set up the entry point of the application by checking out into the project/src directory and create two files app.ts and server.ts .

```
mkdir &&  cd src
touch server.ts
touch app.ts

```

the files should look like this :
This will contain the express routing and handle API requests.
```
#app.ts
import express, { Application } from 'express';

// express app
const app: Application = express();


export default app;

```
This Nodejs server, serves the express application and exposes it over  process.env.PORT or 8000;
```
#server.ts
import app from './app';

// constants
const PORT = process.env.PORT || 8000;

// listen
app.listen(PORT, () => console.log(`⚡ on http://localhost:${PORT}/api`));
```


- Next up, we’ll set up a PostgreSQL database to create our tables for the star-wars movie API.

### Set Up a Local PostgreSQL Database with docker
Create a Postgres image at the root of the project directory, build the image, and run it.
```
touch postgres.yaml
```
the docker-compose file should look this: 

```
version: '3'
services:

  postgres:
    # For more details on configuring the Postgres Docker image, see:
    #   https://hub.docker.com/_/postgres/
    image: postgres:10.3-alpine #light weight 

    # Expose the default Postgres port on localhost
    ports:
      - '5432:5432'
      # allow connections from the host machine
      # an instance of the postgres image = postgres_docker(container)
    network_mode: bridge # port available on localhost 
    container_name: postgres_docker

    environment:
      POSTGRES_USER: 'postgres'
      POSTGRES_PASSWORD: 'pgpass'
      POSTGRES_DB: 'postgres'

    # Copy files from dbinit into the image so that they will be run on boot
    # mount db
    volumes:
      - ./initdb:/docker-entrypoint-initdb.d

```
If you want to run a PostgreSQL database with Docker, use the following command:

```
docker compose -f postgres.yaml up
```

The docker run command creates a new docker container named postgres_docker, exposing the container port 5432 to the local port 5432. 

Our database is up and running. Next up, we will add Prisma to our Node.js project and create our schema.


### Install Prisma ORM to Your Node.js Project
Run the following command, which installs Prisma as a dev dependency

```
npm install prisma --save-dev
```
After that, initialize the Prisma schema with the following command:

```
npx prisma init
```
Next, add the database connection string to the env file,
it should look this:

```
#.env
DATABASE_URL="postgresql://postgres:pgpass@localhost:5432/movies_storage?schema=public"
```

### Add Models and Run Prisma Migration
the Prisma folder at the root directory contains prisma.schema file, in it we define our database tables for Movies and Users with their relation.

```
datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

generator client {
  provider = "prisma-client-js"
}

model Movie {
  id        Int      @id @default(autoincrement())
  title   String @unique
  description     String

  createdAt DateTime @default(now())
}

```

We have added One table to the schema. The Movies table, which has an auto-increment ID, the title field that must be unique, description, and createdAt field that defaults to the current date.


To convert these models into PostgreSQL database tables, run the following command:

```
prisma migrate dev --name init --preview-feature

```
This generates the migration file, that contains the SQL that creates the tables in the PostgreSQL database.

### API to interact with movie listings
We will Update the app.ts file we created in a previous step and add the POST Movies and GET Movies API.

```
const app: Application = express();

import { PrismaClient } from '@prisma/client';
const prisma = new PrismaClient();

app.get('/', (req, res) => {
  res.json({message: 'alive'});
});

app.post('/movies', async (req, res) => {
  const {title, description} from 'req.body';
await prisma.movie.create({
     data: {
     title,
     description,
}
    });
   res.json({
    message: "movie created"
  });
});

app.get('/movies:id', async (req, res) => {
const movie =  await prisma.movie.findUnique({
   where:{
   id: req.params.id
},
  });

  res.json({
    data: movie,
  });
}

app.get('/movies', async (req, res) => {

  const allMovies =  await prisma.movie.findMany({});

  res.json({
    data: allMovies,
  });
});

export default app;

```


### Wrap-up
We built a Movie listing API with Express.js+typescript and Prisma ORM, using a PostgreSQL database running on Docker. I know we barely scratched the surface of what Prisma offers, nonetheless, I'm hoping this gives you some insights into adding Prisma ORM to your Nodejs apps.

To finish up, I suggest you further explore some great Prisma ORM features. For instance, query optimization, database transactions, and aggregate queries are good reads.

Thanks for the audience and I hope you found this article helpful 🤗. Feel free to reach out to me on  [Github](https://github.com/nextwebb), [Twitter](https://twitter.com/i_am_nextwebb) and [LinkedIn](https://www.linkedin.com/in/peterson-oaikhenah-102645144/).
Do drop a like, comment, and share 😌.


Happy coding!








