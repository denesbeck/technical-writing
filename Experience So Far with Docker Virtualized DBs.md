---
title: Experience So Far with Docker Virtualized DBs
created: '2022-04-15T13:59:37.918Z'
modified: '2022-04-16T14:32:09.660Z'
---

# Experience So Far with Docker Virtualized DBs 

## What is Docker? 

As you probably know, Docker is a quite popular development tool these days, especially in the world of DevOps. It’s really exciting to see how many things you can do with Docker by virtualizing environments on your machine. Let’s just mention the most basic example: if you want to share your application, you only need to build the application image with Docker which can be used to create a Docker container (that is basically an OS-level virtualized and separated environment) on another machine in which the application runs. The prerequisite is that Docker is installed on the other machine. 

In our team we mostly work with Docker when we want to deploy an application in Red Hat Cirrus using a Dockerfile. Beside this example, you can also use Docker to set up an entire database in a virtual environment that could make the life of the developers easier. In this post I would like to share with you the experience which we gathered during the usage of such containerized databases in our team. 

## Problems with shared databases 

When developers work in shared development databases they have to share that one resource with eachother meaning that if a developer changes something (configuration or data) it will be visible for the other developers too. That could lead to some confusion. 

Also, there are dependencies like waiting for the database/table creation/configuration, granting access to schemas, views, tables. 

You are also limited at some degree. You might have multiple possible solutions to a certain issue but you can’t test those because either you don’t have the right access or that would result a data change which could have negative effect on the work of some other developers. 

## Benefits of using Docker dev databases 

Obviously the above problems can be resolved even in a shared database, however, I believe that a virtualized database in Docker is a better and simplier solution. I started to work with Docker containerized databases this year and I noticed the following benefits: 

- I have the freedom to do whatever I want in my personal virtualized database. Having this freedom I can be more creative when it comes to database architecture, I can experiment with things I only read or heard about without affecting the work of other developers. 
- I don’t have to worry that I make mistakes that would affect the work of other developers. 
- I only use the database when I need it meaning that the development database shouldn’t run in the cloud 0-24 which is more resource friendly. 
- If I need to reload the content of a table I don’t need to warn the other developers. 

## Downside of segmented dev databases 

A negative effect of working in such environments is that all developers need to track the configuration/table changes consistently. They need to communicate with eachother. If for example you change the length of a column in a table that needs to be shared with the other developers otherwise before the release you will have some confusing times. 

For tracking changes I recommend to save at least the create scripts in GitHub that is shared with your team. Anyone who changes something in the database should also update the script file in the repository immediately. This will reduce the risk of having multiple versions of configurations for a database/table. 

## How to create my dev database using Docker 

For initializing such environments there is a great article on towardsdatascience.com. I’ve also created a GitHub repository that has instructions on how to create your own database using PostgreSQL or MongoDB. There are two files for each database engine: a yaml file which you can execute with docker-compose (that’s an easier way to initialize everything with one line of command) and a docker run file that contains the docker run commands. 

## Okay... but how is this related to CODA? 

Although, CODA is a small web development team, we are proud that we always find time to experiment with new and innovative technologies. Sometimes we notice that the technology we are investigating is not as beneficial as we first thought or it’s not suitable for the team. Sometimes we have a breakthrough and we have clear benefits that change our work processes or the initial design of an application. A good example is the Zowe framework that is used to interact with mainframes and which is now a core library in our Z-Automation Portal project. We plan to discover further this framework and to extend the usage of it (e.g. automating/modernizing the IT processes of mainframe systems), stay tuned for further updates! Another example (still in discovery phase) is the Tailwind CSS framework that can be used to style your application without writing a single line of CSS code. 

From using Docker we expect that the development process will be simplified and the database planning will be more consistent (also by using create scripts) and clearer. It will be also easier to be complient with the SoD rules by separating the development environments entirely from the test and production environments without worrying about permissions and id segmentation. One way or another, working with Docker is a must these days so getting familiar with it should be benefitial for the team in the long term.  

 

**Resources:** 

[Docker Compose](https://docs.docker.com/compose/) 

[Denes Beck’s personal GitHub repo](https://github.com/denesbeck/docker-dev-env)

[Local Development Set-Up of PostgreSQL with Docker](https://towardsdatascience.com/local-development-set-up-of-postgresql-with-docker-c022632f13ea)

 
