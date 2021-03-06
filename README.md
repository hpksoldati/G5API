# G5API - API Backend for Get5
_**Status: Early Alpha.**_

G5API is going to be a replacement for the get5-webpanel. _Currently_ this is the backend only, as it will allow the plugin to interface with a database, as well as make various calls to functionality that is seen in the [get5-webpanel](https://github.com/phlexplexico/get5-webpanel)


## What does it do?
Currently, very basic CRUD operations, as well as legacy calls that the get5-web api used, as referenced [here](https://github.com/PhlexPlexico/get5-web/blob/development/get5/api.py). Right now, this is a very early build to try my hand at using Express as a middleware, and try some JavaScript technologies so that others may create their own front-end applications, with all the difficult back-end stuff being completed. 


This API should be complete enough to do basic operations to the game. **Server operations are currently not in place** and these will exist in different routes. ~~I was thinking sending to game server should be within the `/server` route, as it would make sense that a match should interact with the server, and make any rcon request to the game server?~~

Server interaction will most likely take place in `/match/:match_id/server/{rcon|start|etc}`. These command should probably be logged in the database for audit purposes as well.

## What does it NOT do?
Basically every "advanced" feature the current web panel has, from editing matches while in game (should be done with a front-end and calls to steam), to displaying any of the data. This is simply a back-end to get myself used to JavaScript and Node. Maybe eventually I will work on a front-end in React or Vue, but it depends on how long I stay motivated with this.

## Why?
[Get5-webpanel](https://github.com/phlexplexico/get5-webpanel) is a now out-dated webpanel, with python2.7 being officially EOL. Being built all on Flask, with ORM (SQLAlchemy), and Jinja2, its tech spans more than a few years old. While it works really well for now, it is becoming increasingly harder to deploy to more modern hardware (such as Ubuntu 19).

The intent will to be provide similar functionality with the use of NodeJS and Express, and this API will take care of session authentication as well, via the use of `passport-steam`, and rcon server commands via `rcon-srcds`, as well as more normalization in the database.

## Building
In order to build this application, I've opted to use [Yarn](https://yarnpkg.com/lang/en/).

First you will need to copy over the ```development.json.template``` and update any values that are required. These include server values, and database passwords and connections.

If you wish to roll a production build, please copy ```production.json.template``` and fill out all the values.

### Migrate dev database: 
```yarn migrate-dev up:dev```

The database must be existing before you run this, and you can change which database to use in the `development.json` area. **Please Note this only works with Node <= 11.15.0.** The entire App can work with Node v12, but there is currently an outstanding issue with the migrate function. Also note that there are some tables that have changed. I've opted to normalizing the spectators and team authentication values, as BLOBS were not playing nicely with Node. My current fork deals with inserting into these tables, so I hope that it will eventually be a smooth transition where you can keep all your data from the old Flask app.

### Migrate production database:
```yarn migrate-prod-upgrade```

### Build and run: 
```yarn start``` 

Spins up a development server where you can make all your calls. Since steam authentication is enabled, you will need to auth with steam first before making any calls.

### Docs: 
```yarn doc```

This will generate all the API information that I've created in the app, in the hopes of making it more readable and easier to pickup for anyone who wants to try more implementation, or even creating a front-end for this API.

### Coverage Tests
WIP - These will most likely be created with [Supertest](https://npmjs.com/package/supertest) and [Jest](https://jestjs.io).

Steam OAuth will be mocked in order to check if a user is "logged in", and create a temporary database (`get5test`) that will insert new values, and check various features of routes.

```yarn test```

Will *require* `development.json` to exist in projects `config` folder.

## Contribution
Sure! If you have a knack for APIs and a penchant for JavaScript, I could always use help! Create a fork of this application, make your changes, and submit a PR. I will be using the [Issues](https://github.com/g5api/issues) page to track what calls still need to be completed. This project won't be finished anytime soon, as I would like to make sure there is a proper handle on authentication with the API, as well as proper security implemented to prevent any unwanted uses with the application. 

If you so choose to contribute, please make sure you include documentation for the API calls, as it is how I am keeping track of all the functionality. I'm using [JSDoc](https://devdocs.io/jsdoc/) to provide documentation, which will eventually be moved over to a wiki.

# License
This project is licensed under [MIT License](http://opensource.org/licenses/MIT). A copy of this license **must be included with the software**.
