---
date: "2021-11-09T00:00:00+01:00"
draft: false
linktitle: Testing Database in Docker
menu:
  example_tech:
    parent: Database
    weight: 2
title: Testing databases in a Docker Container
toc: true
type: docs
weight: 2
---

## Setting up Docker locally

**Situation**: We need have an separate frontend-backend work environment instead of spinning up a test database in our *actual* database.

We need to separate `test environment` from `dev/prod environment`.

For demonstration, I'm pulling a docker container into my local machine to test a database for the bounty board project.

Enter Docker. Assuming a dev has **already setup** the docker container in a specific feature branch `feature/docker-compose-mongo`.

```{python}
# clone repo that contains docker container (bountyboard-docker directory is installed locally)
git clone https://github.com/jordaniza/bounty-board.git bountyboard-docker

# change directory into that directory's root
cd bountyboard-docker

# create new feature branch to match branch you want to pull locally (containing the docker container) 
git checkout -b feature/docker-compose-mongo

# pull container to your local environment
git pull origin feature/docker-compose-mongo

# change directory to folder with "Dockerfile"
cd mongo

# run command to start Docker up
docker-compose up --build

# start up (frontend) App
# install first
yarn && yarn dev

## OPEN LOCALHOST:3000 http://localhost:3000

```

## ENVIRONMENT VARIABLES

We have to change directory into `packages/react-app/.env.local` to create the `.env.local` file (changed for security)

```{python}
# terminal
$ code .env.local

# paste this into the newly opened file

BUILD_ENV=development
MONGODB_DB=bountyboard
#PROD_MONGODB_URI=
MONGODB_URI=mongodb://localhost:27017/bountyboard
NEXT_PUBLIC_DISCORD_SERVER_ID=8******************0
NEXT_PUBLIC_DISCORD_CHANNEL_BOUNTY_BOARD_ID=8******************0

# Public Environments
NEXT_PUBLIC_DAO_CURRENT_SEASON=1
NEXT_PUBLIC_DAO_CURRENT_SEASON_END_DATE=2021-08-31T04:00:00.000Z

# URLs
NEXT_PUBLIC_DAO_BOUNTY_BOARD_URL=https://bountyboard.bankless.community
DISCORD_BOUNTY_BOARD_WEBHOOK=

```

## What happens when you pull a Docker container locally

We are: 
- Pulling a folder for MongoDB with the latest scripts, schema, and Dockerfiles
- Using the docker-compose utility, `docker-compose up --build` to fire up a Mongo container, and a temporary seeding container
- The seeding container runs a set of bash scripts to populate the DB
you can see this in `mongo/seed.sh`

## Opening Additional Terminal windows: Mongo

Once, I ran `yarn dev` I got the Application to fire up, `ready - started server on 0.0.0.0:3000, url: http://localhost:3000`

but `Mongo` wasn't turned on. 

First, open up a new terminal, then:

```{python}
docker exec -it mongo_mongo_1 bash
```

**NOTE:** Install DOCKER in VSCode to follow along

Find the Container that's a GREEN TRIANGLE, right click, then `attach shell`.

**NOTE:** This did not work, so I needed to open a 3rd Terminal to type in:

```{python}
docker exec -it mongo_mongo_1 bash
```

`exec` executes a command `-it` starts an interactive terminal session `mongo_mongo_1` is the name of the container and `bash` is the shell.

**NOTE:** You should see something like `root@4d5cedd1a8a7:/#`, then type in the following to fire up **Mongo Shell**:

```{python}
mongosh
```

Another confirmation is `Connecting to:          mongodb://127.0.0.1:27017/?directConnection=true&serverSelectionTimeoutMS=2000`

At this point you can do basic Mongo commands:

```{python}
show collections
```

**NOTE**: I was on the `test>` database and needed to switch to `bountyboard>` database where the "test" data was located and ready for testing.

Other commands to run are `findOne()` or `find()` just to see the *seeded* data:

```{python}
db.bounties.findOne()
```

**NOTE:** I did not get the front end to work so for this session, we only tested the database by querying in Mongo shell. 








For more content on data science, R, and Python [find me on Twitter](https://twitter.com/paulapivat).