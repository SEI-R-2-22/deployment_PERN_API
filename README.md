# PERN API Deployment

- Deploy server to Heroku


## Preparing Your Database Connection

Open the `config/config.json` file for Sequelize. Rename the file to `config.js`. And refactor it to a module exports like below:

```js
require('dotenv').config()
module.exports = {
  development: {
    database: '<Your Dev Database>',
    dialect: 'postgres'
  },
  test: {
    database: '<Your Database Test Name>',
    dialect: 'postgres'
  },
  production: {
    use_env_variable: 'DATABASE_URL',
    dialect: 'postgres',
    dialectOptions: {
      ssl: {
        rejectUnauthorized: false,
        require: true
      }
    }
  }
}
```

In `models/index.js`, we need to fix the require for the config. Change the config require statement to the one below:

```js
const config = require(__dirname + '/../config/config.js')[env]
```

## Creating A Heroku Project

In your project directory, run:

```sh
heroku create <name of backend>
```

Once your app is created, we need to add postgres as an addon:

```sh
heroku addons:create heroku-postgresql:hobby-dev
```

Next we'll add any `environment variables` for your project (from your backend .env file):

```sh
heroku config:set VARIABLE_NAME=<variable>
```

### Wiring Up Our Code


In your `package.json`, add a new script in your `scripts` section:

```json
    "build": "npm install && npx sequelize-cli db:migrate"
```

Make sure you have a `start` and `dev` script:

```json
{
  "dev": "nodemon <entry>.js",
  "start": "node <entry>.js"
}
```


## Deploying Our Project

Publish a build by adding and committing your changes and running `git push heroku main`.

## Monitoring Your Server

You can run `heroku logs --tail` to monitor what's happening with your server.

## Seeding Files

If you have seeds that you want to run, you can ssh into your Heroku server with `heroku run bash` and then running `npx sequelize-cli db:seed:all`.