# guestbook-nodejs

In Development!
- Loopback port of the Guestbook App written in Go

## To run

### Option 1: Running locally using node

```bash
cd src
npm install
npm start
```

### Option 2: Running the container

```bash
docker run -p 3000:3000 odrodrig/guestbook-nodejs:key-value
```

The application can be accessed through http://localhost:3000/

The API Explorer can be accessed through http://localhost:3000/explorer

## Datasources

This application has a KeyValueModel representation for the data model which is compatible with Redis and other key value stores. By default the app stores data in memory which means that the data does not persist if the app crashes or goes down for any reason. You have the option to use Redis to persist the data from the application by adding the following environment variables:

Required:

- REDIS_HOST - The hostname to access Redis
- REDIS_PORT - The port to access Redis

Optional:

- REDIS_PASS - The password to access Redis. If you are using an unsecured Redis instance, leave this blank.
- REDIS_DB - The name of the database within Redis. This can be left blank and the default database name '0' will be used.

You must also change the datasource listed in `src/server/model-config.json` to `redis` as seen below:

  ```json
  "list": {
      "dataSource": "redis",
      "public": true
    }
  ```


Next, you will need to replace the `src/server/datasources.json` file with the following:

```json
{
  "in-memory": {
    "name": "in-memory",
    "connector": "kv-memory"
  },
  "redis": {
    "host": "${REDIS_HOST}",
    "port": "${REDIS_PORT}",
    "url": "",
    "password": "${REDIS_PASS}",
    "name": "redis",
    "db": "${REDIS_DB}",
    "connector": "kv-redis"
  }
}
```

### Generate from OpenAPI

```
% lb -v
4.2.1 (generator-loopback@5.9.4 loopback-workspace@4.5.0)

% git clone https://github.com/IBM/guestbook-nodejs.git
% cd guestbook-nodejs

% lb guestbook    
? What's the name of your application? guestbook
? Enter name of the directory to contain the project: src
   create src/
     info change the working directory to src

? Which version of LoopBack would you like to use? 3.x (Active Long Term Support)
? What kind of application do you have in mind? api-server (A LoopBack API server with local User auth)
Generating .yo-rc.json

I'm all done. Running npm install for you to install the required dependencies. If this fails, try running the command yourself.

   create .editorconfig
   create .eslintignore
   create .eslintrc
   create server/boot/root.js
   create server/middleware.development.json
   create server/middleware.json
   create server/server.js
   create README.md
   create server/boot/authentication.js
   create .gitignore
   create client/README.md
npm WARN deprecated loopback-boot@2.28.0: This version is no longer supported, please upgrade to 3.x
npm WARN deprecated swagger-ui@2.2.10: No longer maintained, please upgrade to swagger-ui@3.
npm WARN deprecated request@2.88.2: request has been deprecated, see https://github.com/request/request/issues/3142
npm WARN deprecated har-validator@5.1.5: this library is no longer supported
npm WARN deprecated circular-json@0.3.3: CircularJSON is in maintenance only, flatted is its successor.

> ejs@2.7.4 postinstall /Users/remkohdev@us.ibm.com/dev/src/github/guestbook-nodejs/src/node_modules/ejs
> node ./postinstall.js

Thank you for installing EJS: built with the Jake JavaScript build tool (https://jakejs.com/)

npm notice created a lockfile as package-lock.json. You should commit this file.
added 445 packages from 469 contributors and audited 445 packages in 13.05s

9 packages are looking for funding
  run `npm fund` for details

found 3 moderate severity vulnerabilities
  run `npm audit fix` to fix them, or `npm audit` for details

   ╭────────────────────────────────────────────────────────────────╮
   │                                                                │
   │      New patch version of npm available! 6.14.6 → 6.14.8       │
   │   Changelog: https://github.com/npm/cli/releases/tag/v6.14.8   │
   │               Run npm install -g npm to update!                │
   │                                                                │
   ╰────────────────────────────────────────────────────────────────╯

Next steps:

  Change directory to your app
    $ cd src

  Create a model in your app
    $ lb model

  Run the app
    $ node .

The API Connect team at IBM happily continues to develop,
support and maintain LoopBack, which is at the core of
API Connect. When your APIs need robust management and
security options, please check out http://ibm.biz/tryAPIC

% cat > .gitignore <<EOF
.DS_Store
src/node_modules/
EOF

% cd src
% lb swagger
? Enter the swagger spec url or file path: ../api/guestbook-api.yaml
? Select models to be generated: (Press <space> to select, <a> to toggle all, <i> to inverse selection)Message, Person, Contact, Address, Ping
? Select the datasource to attach models to: db (memory)
Creating model definition for Message...
Creating model definition for Person...
Creating model definition for Contact...
Creating model definition for Address...
Creating model definition for Ping...
Model definition created/updated for Message.
Model definition created/updated for Ping.
Model definition created/updated for Contact.
Model definition created/updated for Person.
Model definition created/updated for Address.
Creating model config for Message...
Creating model config for Person...
Creating model config for Contact...
Creating model config for Address...
Creating model config for Ping...
Model config created for Message.
Model config created for Person.
Model config created for Contact.
Model config created for Address.
Model config created for Ping.
Generating /Users/remkohdev@us.ibm.com/dev/src/github/guestbook-nodejs/src/common/models/message.js
Generating /Users/remkohdev@us.ibm.com/dev/src/github/guestbook-nodejs/src/common/models/ping.js
Models are successfully generated from swagger spec.

% node .
hsts deprecated The "includeSubdomains" parameter is deprecated. Use "includeSubDomains" (with a capital D) instead. node_modules/loopback/lib/server-app.js:74:25
Web server listening at: http://localhost:3000
Browse your REST API at http://localhost:3000/explorer

```