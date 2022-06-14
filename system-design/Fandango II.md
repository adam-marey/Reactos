# Background

System Design questions test a candidate's ability to create a system based on a user prompt. A user prompt may consist of either a client telling you what they want, or a team making an assumption of what a potential user might want. What is the difference between this prompt and an OOP Design question? This prompt focuses less on the application's components and more of the flow of data and how we scale that data based on our understanding of the various trade offs as we modify our system.

## Learning Objective
Understand the flow of data and how to scale.

## Design Fandango II

User prompt: I want to be able to book movie theater tickets/seats online.

Today, we are going to focus on scaling Fandango.

Draw data flow diagrams from client to server to database that encompass caching, load balancing, and microservices. The scenarios/case studies that we need to address are:

1. Our server is being hit with multiple requests from many different clients at once and the server can barely handle it anymore
2. We have a feature that shows the most popular new movie releases, but the query is really slow
3. Our application is a monolith. If we make changes to a specific part of it, we need to redeploy the entire application afterward

## Strategy

Before addressing scaling, we should encourage the candidate to draw the layers of a web application as they have already been taught in the context of Fandango.

Interviewers should try to guide them by using the case studies above as launch off points. This keeps the question from being too open ended and keeps it true to the lecture material.

### Sample Diagram of Web Application Layers

![Sample Diagram of Web Application Layers ](https://user-images.githubusercontent.com/83469447/144760643-2bc937d3-4627-48d1-92f3-c39a018bd103.png)

Interviewees should be encouraged to discuss why the current design is not scalable.
They should discuss how a load balancer or cache would fit into Fandango here.

### Case Study 1: Our server is being hit with multiple requests from many different clients at once and the server can barely handle it anymore

This would be a great use case for a load balancer.

A loader balancer distributes network or application traffic across a number of servers. Load balancers keep track of the status of our resources by performing regular health checks on our servers. This is also known as horizontal scaling: adding more servers to help us scale. Think of it like adding more buildings to a neighborhood (horizontal scaling) vs. adding more floors to a building (vertical scaling). Load balancing can optimize the response time and avoid unevenly overloading some servers while others are left idle.

Interviewees should also discuss some possible issues that may pop up while using this type of architecture:

- What if we were storing our sessions in the node process, so a user can close their browser, open it again and pick up where they left off on Fandango? 
A possible solution is something called "sticky sessions." We would configure our load balancer to send user session to the same machine that the request is being sent to.

![Sample Diagram of Web Application Layers With Load Balancing](https://user-images.githubusercontent.com/83469447/144760658-19a8520f-daf3-4dd6-97d6-a71dd0a8dcd2.png)

### Case Study 2: We have a feature that shows the most popular new movie releases, but the query is really slow

The goal of this case study is to try to get interviewees to think about caching. Specifically, you will want them to discuss a cache with the least frequently used eviction policy. The idea here is that we can keep track of new releases and their ticket sales. The ones with the least ticket sales get bumped off the cache in favor of the ones with higher ticket sales.

![Sample Diagram of Web Application Layers with Caching](https://user-images.githubusercontent.com/58145741/112518643-ef438d00-8d6f-11eb-907c-ae5939fb6019.png)

### Case Study 3: Our application is a monolith. If we make changes to a specific part of it, we need to redeploy the entire application afterward

The solution to this issue would be microservices. Microservices are their own environments of codebases, deployment strategies, servers, etc. They are usually used to separate tightly coupled functionality or separate different business use cases into their own services (e.g., Uber Taxi and Uber Eats could be two different microservices). Interviewees should be able to identify what feature on Fandango would be appropriate to split into microservices.

Fandango's booking service is a great example of a feature that can be split into microservices. In addition, interviewees should be able to note how the services would work individually, as well as how they would interact with each other. 

Expectation:

- Activity Diagram or Step By Step Process
- Data flow without the items in the previous case studies

It is important for interviewers and interviewees to discuss how these microservices would communicate with each other.
We have learned of many ways to approach this, such as a REST API. We can also use pub/sub design pattern. This is similar to addEventListener, but out of scope for the purposes of this REACTO.

### Sample Active Reservation Service (Microservice)

- Keep the incomplete reservation on a cache for about 5 minutes
- Update the booking table in the database with an expiration time. The status would be set to “On Hold”
- If 5 minutes pass, release the hold
    - Notify the current user of the hold release
    - Send an update to the Seat Waiting Service
- If seats are purchased, update the status in the database to “Booked”
    - Send an update to Seat Waiting Service

### Sample Seat Waiting Service (Microservice)

- Queue of users waiting for seats
    - First come, first serve
    - Users can add themselves to the queue for a possible seat/ticket booking 
- Relating back to Active Reservation Service:
    - If the timer for the Active Reservation Service runs out, the person first in queue will be notified
    - If seat/ticket is booked, notify everyone in the queue

## Sample Activity Diagram

![Microservice Activity Diagram](https://user-images.githubusercontent.com/58145741/112518728-08e4d480-8d70-11eb-8249-e7cdb52ce26c.png)

## Sample Data Flow Diagram

Microservices do not have their own databases because they depend on the same data. 

![Microservice Diagram](https://user-images.githubusercontent.com/83469447/144760667-1f2e7223-b4cc-4b43-992e-2ac6f6a45532.png)

## Resources
* Slides: https://docs.google.com/presentation/d/1pXi2akBSn4p44InoWOT8SRm60Zy6Y7DIlLTxR60vKJg/edit?usp=sharing