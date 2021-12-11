---
date: "2021-12-11T00:00:00+01:00"
draft: false
linktitle: Install Error Tracking
menu:
  example_tech:
    parent: Webdev
    weight: 2
title: Installing Error Tracking with Sentry
toc: true
type: docs
weight: 2
---

## Install Error Tracking with Sentry

**Situation**: Developers want error logging for their applications to track performance. Especially when deployed to actual users. 

The following example features error tracking with **Sentry.io**.

## Steps

### Sentry Setup

1. Figure out what frontend language your team is using, go through the installation guide (e.g., we used NextJS)
2. Install sentry 
3. Install configuration files. Sentry.io had a convenient installation wizard.
4. You'll need to add `options` to two `Sentry.init()` (i.e., client and server config files)

```{python}
# example sentry.init() file from sentry.client.config.js
Sentry.init({
  dsn:
    SENTRY_DSN ||
    "https://8cd5c6d0a0a14e2ab48371d22a9535b4@o1071534.ingest.sentry.io/6069034",
  // Adjust this value in production, or use tracesSampler for greater control
  tracesSampleRate: 1.0,
  debug,
  // ...
  // Note: if you want to override the automatic release value, do not set a
  // `release` value here - use the environment variable `SENTRY_RELEASE`, so
  // that it will also get attached to your source maps
});
```

5. Include SENTRY_DSN in `.env.local` file:

```{python}
# make sure the digits 8888888 match the sentry project when you login
SENTRY_DSN = "https://8alphanumericstring4.ingest.sentry.io/8888888"
```

6. Log into sentry account with username and password

### Setting up a Test Database connection

You want to run a local instance of the App. One way to ensure you're able to run your App locally is to also run a test database to confirm that data on the frontend, matches what's in the test database.

1. Create a .env.local file to store database credentials (**MONGODB_URI**)

```{python}
# specific mongo connection
MONGODB_URI=mongodb+srv://username:password@cluster0-m0sandbox.gamml.mongodb.net/project_name

# default
MONGODB_URI=mongodb://localhost:27017/project_name
```

2. Make sure a Docker container with Mongo DB installed is running
3. In parallel to your application running, fire up a Mongo instance locally (note how it's the **same** string as whats in the `MONGODB_URI` in your `.env.local` file.)

```{python}
# cd into project folder but mongo should be globally accessible
$ mongosh “mongodb+srv://username:password@cluster0-m0sandbox.gamml.mongodb.net/project_name”
```
4. Query something you see on the frontend (e.g., user_id) on the backend to see that data matches. 

### Run local instance of Application 

note: Bounty-board App

1. cd into root directory of the `app` (../packages/react-app)
2. yarn & yarn dev
3. enter `localhost:3000` on your browser to see the App running.
4. Also note *sentry logging* in your terminal
5. Query something you see on the frontend (e.g., user_id) on the backend to see that data matches. 


### Throw an error

1. Manually have sentry sent a message to the Sentry UI tool through `sentry.server.config.js`

```{python}

## capturing errors
Sentry.captureMessage("Insert message with captureMessage.");
```

2. Throw a naturally occuring error like a wrong API route in the browser
3. Check sentry.io account for error logging. 

For more content on data science, R, and Python [find me on Twitter](https://twitter.com/paulapivat).