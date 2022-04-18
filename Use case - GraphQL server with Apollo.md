# Use case: GraphQL server with Apollo

## Introduction
The CODA suite is a collection of different web-applications that aim to resolve certain business problems using cutting-edge technologies. While building the web-applications our team soon had to realize that the original architecture of this ecosystem needs to be improved, otherwise, our delivery rate and performance will decrease. 

## Problem Statement
Originally, each of our applications had a frontend (React, Vue) and a backend (Node) part. The backend was built to connect to the databases to fetch data. It exposed certain REST API endpoints to make it possible for the frontend to communicate with it. 

Using this architecture, the backend servers often used the same technical ids to connect to the databases, moreover, for each of the applications an SSO authentication service had to be registered and implemented. This resulted a lot of code duplicates throughout our projects and with the reused technical ids and certificates there was a serious risk exposure. Besides, in case we had to change the password of an expired technical id, we also had to make sure that secrets (storing passwords of the technical ids in Cirrus) were also updated accordingly in all applications, which could also lead to human error.

## Goal
We came to the conclusion that outsourcing all of the REST API endpoints into one Node application would be beneficial, as:
- the code duplicates would be removed
- the technical ids would be used only in this application
- the password of the technical ids needs to be updated only in one Cirrus application
- the API endpoints would be centralized
- the format of the API endpoints would be consistent
- it would give us further options to implement a robust logging service for all of our applications

It’s important to note here that although we wanted to have pure frontend applications in the end, it was not possible as Cirrus currently doesn’t support serverless deployment. So, we still need to have a basic Node backend in each of our applications.

## REST API vs GraphQL
Another issue that we soon faced was that the traditional REST API design was simply not suitable for our application suite. We often had to query different columns from the same resource using the same API endpoint that also returned data that in the end we didn’t use. This is called overfetching. A possible solution would have been to create more micro level API endpoints to avoid fetching unnecessary data, however, we didn’t want to have 10-20 endpoints for querying the same resource. We were looking for a more sophisticated solution. Finally, we learned about GraphQL.
GraphQL is an open-source API query language. It’s deployed using a single API endpoint that exposes not only a single resource but a whole service. You send queries to this endpoint, based on the query the server will return only the requested amount of data. 

GraphQL offers a solution for the overfetching and underfetching dilemmas without the need to build more API endpoints. This makes the data fetching process of the application more optimized. You don’t need to worry about API versioning either as only a single API endpoint is exposed, and you define the schema of the required data set by writing queries. Since Typescript has introduced type-safety coding to JavaScript we can see that it shortly became a trend. GraphQL also makes sure that the types are declared and that the queries are written correctly on the client-side using scalars.

Considering the above benefits, I would recommend using GraphQL for larger or more complex applications or application suites, however, for simple applications using REST API could be a better choice.

## Building Crimson
GraphQL is running on a plain Node server with Express framework at the top. To make it easier to use, we started to use the opensource Apollo GraphQL platform which can be used on both server- and client-side. Using Apollo on the server-side makes it easier to prepare your schemas and to build and design your backend service. On the other hand, using it on the client-side we can write queries and by using built-in hooks we can execute them easily on the frontend. In addition, Apollo also offers a cloud-based UI called Apollo Studio to build, validate and secure graphs.

Using plain Apollo won’t be enough for larger projects, we had to realize this shortly after starting to build the schemas for different applications. To modulate our GraphQL backend, we needed to use an external library called GraphQL Modules. As the name implies, you can separate your schema definitions into different modules which will result an easier to read and maintainable code. 

GraphQL Scalars is another library that I would recommend using. In GraphQL there are five built-in scalars that you can use to define the types in your schema: Int, Float, String, Boolean and ID. You can of course construct your own types, however, to save yourself some time installing GraphQL Scalars could be helpful. It includes such basic types like Currency, EmailAddress, JSON, Timestamp, URL, UUID… etc. Both libraries are quite helpful when you want to design a complex yet consistent GraphQL service.

We started to build our GraphQL server named Crimson in January 2022. We connected two of our applications to it already: Z-Automation Portal 2 and Catacomb 2. Our plan is to connect all of our frontend applications to Crimson, so that we will end up having a centralized service-oriented platform that eventually could be also used by other teams.

## Conclusion
Based on our experience so far, we dare to say that switching to this relatively new technology and refactoring our applications was worth the effort. We experience the advantages of this new centralized server already: We only have one SSO service registered for production, we removed a lot of code duplicates (DRY), we have cleaner code and last but not least, we have an informative yet easy to read logging already. Although only two frontend applications are connected to it at the moment, Crimson already plays and will play an important role in the CODA ecosystem. It can be seen already that it will boost the performance of our team in the future.

**Resources:**

[GraphQL Vs. REST APIs by Ronak Ganatra](https://graphcms.com/blog/graphql-vs-rest-apis)

[GraphQL vs. REST by Sashko Stubailo](https://www.apollographql.com/blog/graphql/basics/graphql-vs-rest/)

[GraphQL is the better REST](https://www.howtographql.com/basics/1-graphql-is-the-better-rest/)

[GraphQL Modules](https://www.graphql-modules.com/)

[GraphQL Scalars](https://www.graphql-scalars.dev/)

[Apollo Studio](https://www.apollographql.com/docs/studio/)
