### Introduction

Hello, I am Kartavya Ramnani, I work as a SDE2 Backend in BookMyShow. 
Before BMS, I was a SDE at Amazon and I have completed my education from IIT Ropar, Punjab. 

In BookMyShow, I work in the payments tribe. I was a part of 2019 wave of developers with the goal of improving the tech
and make it feature-rich. The tech was not able to catch-up to the product and business ambitions, primarily because of the 
sub-par aggressive hiring done in 2017. I am also an active member of the tech hiring interview panel in BMS. 

I joined in April 2019 and in the beginning I worked on existing systems and adding features to the existing systems like 
improving payments performances and have more observability over it but I would like to discuss my flagship project which is
Order Summary and Payment Listing Revamp. Our long term vision is improving the payments as a whole but the entryway to that
system is Order Summary and Payments configurations where you are getting the listing. So, we started with that in November last year
and it also paved way to set up a team culture, the technical standards and the best practises across design and coding. 

### Payment Listing

Before our project, a static payment listing was served at the payment page for all the users regardless of the type of the booking
or the location or the inventory purchased etc. For any customization to this static list or application of business use cases, either
there was hardcoding (client or backend) or a rudimentary or specific implementation of the usecases. The sponsored spot was configured on a flag
as part of the static listing and there was a logic at client side to handle it. Thus, for any new usecase or even similar usecases
the required time and effort was huge because a code change was involved. 

So, we started with the requirements gathering in the first phase :
Functional requirements :
1. Filtering of payment methods (Showing or not showing) : These filters can be on any of the parameters like :
    * Event type (movies, events, online movies, renting a movie)
    * Specific event (Suryavanshi or U2 concert or ED Sheeran concert)
    * Specific venue types and venue codes (PVR cards etc)
    * Type of user (superstar, regular, guest user, first time user)
    
1. Sponsor Spot : 
    * We are paid a lot of money on sponsor spots by payment types. 
    Currently, there was no field of sponsor message, they used to change the name of the payment type
    and add the sponsor details there. :P 
    * Freecharge sponsoring a movie or AmazonPay sponsoring an event or PhonePe sponsoring Sunburn.
    * Same above filters but on sponsor spots. 

1. Block and grey out :
    * Based on success rate

1. In-built sorting based on a field : 
    * Last used payment type ( Especially used in Saved Payments or Quick Pay)
    * Most used payment type
    * Based on success rate maybe.
    
    And with sorting if you want to limit the number to be shown.
    
1. Everything needs to happen on the fly, in real-time and without any code change.

1. Remove the deadweight fields and optimise the then payload which was 3000 to 1500 fields now and way-way more readable. 

1. Input Validation (My foresight) :
Sort of like a secret feature (part of the launch feature and everything but was ignored) of our system which is paying huge dividends recently. 
Our team was appreciated recently for silently implementing this. 
Cards, UPI Collect : These have input fields which customers enter text into, we can not figure out what bank or what bin range
they have entered beforehand and say a bank is performing poorly or it is a bank exclusive event. Then, until now
    * API call used to be done and hardcoding done on backend
    * Or It used to be hardcoded on frontend (4 days code and deploy cycle) and hardcoded in backend (for direct backend calls consistency)
    * We implemented a regex field and asked the FE guys in our teams to honor it. We also sent a regex failure message.
    This can be configured on runtime. 
    
Usecase: PhonePe Yes Bank Issue which had happened. 

1. Common payments configuration system (Single Source of Truth) :
    * Payments did not own its own configurations. 
    * They had their own configuration system. (it was horrible)
    * Now, we have a single source of truth as integrations has also integrated with our APIs.
    
1. CMS Revamp :
Payments did not have a CMS. Direct APIs calls or DB calls. It was very bad. 
Now, we have a CMS which can be used by QAs, Product heads and Tech Support people as well.
Also, used for Data Ingestion, adding new payment type or configuration. 
I have not built this. Just the Backend of it. 

### System Design
Order Summary and Payment Listing are closely related with each other.
And, Noone owned Order Summary, it was the dumping ground of BookMyShow. 
So, we decided to own it as well. Our vision in long term being :

Landing page is common, then it branches out in different verticals : movies, events, online, merchandise, (and this fit) movie on rent or buy
and then it merges into a Order Summary and Payment as a service integration with customized client-id behavior and once payment
is completely, the flow again diverges into multiple verticals. 

Order Summary and Payment Listing Ecosystem :
Three services :
1. Payment Listing/Configuration service
2. Order Summary service
3. Wrapper over the two with frontend APIs and provides a common ground between the two services

Tech stack :

Language :
Golang
It is a mission-critical application. 

Why ? 
1. Organizational Reasons : There is a general organization-wide push towards using Golang. 
1. Tech reason :
    * Payments is a sensitive service and it should be very stable. So, we wanted a statically and strongly typed language.
    * Spoiler Alert : We will be using rule-engine which is a operational heavy task : We tested a PoC in C# and Go and found Go to be more efficient and less memory footprint.
    * Lot of concurrency usecases and we preferred Go's concurrency model of Goroutines, Channels.
    * Go is a young, smaller and opinionated language. Not many ways to do thing and easy to set up best coding practises. 
 
Service Communication :
1. Decided to keep the communication simple and synchronous. 
1. Internal backend calls : gRPC and Frontend - Backend calls using REST.
1. Why gRPC : Two different types of Paradigms, both. 
For internal calls, gRPC seems better because a remote procedural call looks like a function call, makes it readable. (Messages vs Resources and Verbs)
Second, gRPC uses HTTP/2 and the performance benefits it provides. The data is serialized, goes in binary and already machine readable. 
Third, It needs to be a very stable, available service. The type checking which we get using protobufs is extremely helpful.
Fourth, less boilerplate code, code gets auto-generated, we just have to call it.
Fifth, Future usecases of streaming. Until the customer is done, we can maintain streaming connection rather than making. 

Why REST with HTTP for frontend-backend calls ?
We create multi-platform APIs and not just android and ios but web (mobile and normal) should also be supported.
With gRPC-WEB not as mature, we decided to not go with it. 

System :
Rule engine provided a mechanism to implement that dynamic behavior which we intended upon. 


Storage :
Aerospike with Hybrid memory.
1. The data model is very simple, it is like a configuration. 
1. The traffic is read-heavy and the latency should be very less.
1. The data in general is not a lot, rules and payment configuration. 
1. Additional in-memory caching also implemented over parameters. 

Why ? 
Aerospike VS Redis :
1. Organizational Reasons : Aerospike enterprise edition
1. Tech reasons : 
    * Aerospike is optimized for use with SSDs which provides a persistence safety as compared to Redis. 
    * Aerospike also provides cross-data centre support which is perfect for us.
    * Scaling is much easier in Aerospike as compared to Redis.
    * Aerospike also provides strong consistency on local cluster and eventual consistency over cross-dc.
    This is better for us as compared to Redis.
    

For Order Summary Revamp :
Refactor vs Rewrite : Part of a single big monolith with painful code and no test cases.
Refactor was eliminated. Long term vision for BMS is to move to microservices and Order Summary needed to be its own microservice.

We decided to go with Strangler Pattern : while we were decomposing the usecases, they need to co-exist, hence, we have components being called in the new service and the old service being called. 

Currently, we have only refactored the interfaces because of business priorities. 