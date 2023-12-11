# Microservice application

Let's try and understand microservices architecture using a simple web application.
I'm going to use a simple application developed by Docker to demonstrate the various features available
in running and application stack on Docker


This is a sample voting application which provides an interface for a user to vote and another interface
to show the results.
The application consists of various components such as the voting app, which is a web application developed in Python to provide the user with an interface to choose between two options a cat and a dog.
When you make a selection, the vote is stored in Redis.
For those of you who are new to read, this read is in this case serves as a database in memory.
This vote is then processed by the worker, which is an application written in dot net.
The worker application takes the new vote and updates the persistent database, which is a PostgreSQL.
In our case, the PostgreSQL simply has a table with a number of votes for each category cats and dogs.
In this case, it increments the number of votes for cats as our vote was for cats.
Finally, the result of the vote is displayed in a web interface, which is another web application
developed in Node.js.
This resulting application rates the count of votes from the Postgres SQL database and displays it to the user.
So that is the architecture and data flow of this sample voting application stack.
As you can see, this sample application is built with a combination of different services, different
development tools and multiple different development platforms such as Python, Node.js, dot, net,