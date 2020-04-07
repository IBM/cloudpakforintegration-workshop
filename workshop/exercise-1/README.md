# Exercise 1: Introduction to the example Bee Travels Application

[re-write everything below this line]

In this exercise, we will introduce the sample application that we'll use for the workshop, a Node.js application called "Bee Travels". "Bee Travels" is written completely in Node.js with its frontend using React and backend using Express.

## Prerequisites

You should have already carried out the prerequisites defined in the [Pre-work](../pre-work/README.md).

> **NOTE:** In the exercises that follow you will see the actual command to run, followed by a separate example of running the command with the expected output. You only need to run the first example and never need to run a command you see preceded by a "$". You can even use the copy button on the right side of the command to make copying easier.

```bash
git clone https://github.com/bee-travels/bee-travels-node.git
```

You should see output similar to the following:

```bash
$ git clone https://github.com/bee-travels/bee-travels-node.git
Cloning into 'bee-travels-node'...
...
```

## Steps

1. [Clone the GitHub repo](#1-clone-the-github-repo)
1. [Use Docker Compose to spin up the application](#2-use-docker-compose-to-spin-up-the-application)
1. [Play around with the application](#3-play-around-with-the-application)

### 1. Clone the GitHub repo

In this section we'll configure our Appsody CLI to pull in Collections.

```bash
git clone https://github.com/bee-travels/bee-travels-node.git
cd bee-travels-node
```

### 2. Use Docker Compose to spin up the application

Now use Docker Compose to build all the microservices

```bash
docker-compose up --build
```

You should see output similar to the following:

```bash
$ docker-compose up --build

Building destination
Step 1/9 : FROM node:12.16.1-alpine3.11
 ---> f77abbe89ac1
...
Step 14/14 : CMD ["node", "index.js"]
 ---> Using cache
 ---> f2a648555cca

Successfully built f2a648555cca
Successfully tagged bee-travels-node_ui:latest
Starting bee-travels-node_hotel_1            ... done
Starting bee-travels-node_currencyexchange_1 ... done
Starting bee-travels-node_destination_1      ... done
Starting bee-travels-node_ui_1               ... done
Attaching to bee-travels-node_currencyexchange_1, bee-travels-node_hotel_1, bee-travels-node_destination_1, bee-travels-node_ui_1
currencyexchange_1  |
currencyexchange_1  | > currencyexchange@0.0.0 start /app
currencyexchange_1  | > NODE_ENV=production node -r esm ./src/bin/www
currencyexchange_1  |
destination_1       |
destination_1       | > destination@0.0.0 start /app
destination_1       | > NODE_ENV=production node -r esm ./src/bin/www
destination_1       |
hotel_1             |
hotel_1             | > hotel@0.0.0 start /app
hotel_1             | > NODE_ENV=production node -r esm ./src/bin/www
hotel_1             |
ui_1                | listening on port 9000
hotel_1             | Listening on port 9002
destination_1       | starting server on port 4000
currencyexchange_1  | Listening on port 4001
```

### 3. Play around with the application

Launch a browser by navigating to <http://localhost:9000> and search for a destination city, like Los Angeles.

<!-- TODO insert screenshot here -->

**Congratulations**! You've just completed the intro exercises for Appsody and Codewind!
