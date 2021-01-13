### What is REST ?
Representational State Transfer architectural style as well as an approach for communications purpose that is often used in various web services development.

#### Constraints :
1. **Stateless** :  REST APIs are stateless, meaning that calls can be made independently of one another, and each call contains all of the data necessary to complete itself successfully.
1. **Client-Server** : The client-server constraint works on the concept that the client and the server should be separate from each other and allowed to evolve individually and independently.
1. **Uniform Interface** : To obtain the uniformity throughout the application, REST has the following four interface constraints:
    * Resource identification.
    * Resource Manipulation using representations
    * Self-descriptive messages
    * Hypermedia as the engine of application state 
1. **Cacheable** : Because a stateless API can increase request overhead by handling large loads of incoming and outbound calls, a REST API should be designed to encourage the storage of cacheable data. 
1. **Layered System** : REST allows you to use a layered system architecture where you deploy the APIs on server A, and store data on server B and authenticate requests in Server C, for example. A client cannot ordinarily tell whether it is connected directly to the end server or an intermediary along the way.
1. **Code on Demand** (optional) : Most of the time, you will be sending the static representations of resources in the form of XML or JSON. But when you need to, you are free to return executable code to support a part of your application, e.g., clients may call your API to get a UI widget rendering code. It is permitted.

#### Resources 
The key abstraction of information in REST is a resource. 
Further, resource representations shall be self-descriptive: the client does not need to know if a resource is employee or device.
It should act on the basis of media-type associated with the resource. 
So in practice, you will end up creating lots of custom media-types – normally one media-type associated with one resource.

#### Resource Methods
Roy Fielding has never mentioned any recommendation around which method to be used in which condition. 
All he emphasizes is that it should be uniform interface. 
If you decide HTTP POST will be used for updating a resource – rather than most people recommend HTTP PUT – 
it’s alright and application interface will be RESTful.

#### REST is not HTTP
In simplest words, in the REST architectural style, data and functionality are considered resources and are accessed 
using Uniform Resource Identifiers (URIs). The resources are acted upon by using a set of simple, well-defined operations.
The clients and servers exchange representations of resources by using a standardized interface and protocol – typically HTTP.

### REST best practises ?

#### Versioning
Versioning is important to manage breaking changes and upgrades to APIs.

1. Versioning through the URI path (Most popular) : http://localhost:9090/v1/investors or http://localhost:9090/v2/investors
1. Versioning through query parameters : http://localhost:9090/investors?version=1, http://localhost:9090/investors?version=2.1.0
1. Versioning through custom headers : <code>x-resource-version</code>
1. Versioning through content-negotiation (Recommended by RMM Level 3 REST Principle: Hypermedia Controls) : Accept: application/vnd.myapi.v2+json

#### Authentication and Authorization

##### Difference between Authorization and Authentication
In summary:
Authentication: Refers to proving correct identity
Authorization: Refers to allowing a certain action

An API might authenticate you but not authorize you to make a certain request.

##### Authentication techniques
1. Bearer token JWT etc : The Bearer authentication scheme was originally created as part of OAuth 2.0 in RFC-6750 but is sometimes also used on its own.
Similarly to Basic authentication, Bearer authentication should only be used over HTTPS (SSL).

1. OAuth (2.0) :
The most common implementations of OAuth use one or both of these tokens instead:
    * **access token**: sent like an API key, it allows the application to access a user’s data; optionally, access tokens can expire.
    * **refresh token**: optionally part of an OAuth flow, refresh tokens retrieve a new access token if they have expired. OAuth2 combines Authentication and Authorization to allow more sophisticated scope and validity control.

#### Uniform Contract
1. Set resources and methods and transfer data type.
1. Set naming convention, payload practises etc. Keep the naming of the same thing consistent. 
1. Common standard of headers and auth.
1. Set response type and error messages and correct status codes


#### Entity endpoints 
The entity endpoints expose reusable enterprise resources, so service consumers can reuse and share the entity resources. 
The investor service, exposes a couple of entity endpoints, such as /investors/investorId, and investor/stockId

#### Pagination when needed
Search calls when a lot results are possible. 

#### Circuit Breaker
Failure due to any issue can potentially create catastrophic effects across the application, and preventing cascading impacts is the sole aim of a circuit-breaker pattern.
Hence, this pattern helps subsystems to fail gracefully and also prevents complete system failure as a result of a subsystem failures.

### What is gRPC ?
gRPC is a modern open source high performance RPC framework that can run in any environment. It can efficiently connect services in and across
data centers with pluggable support for load balancing, tracing, health checking and authentication.
By default gRPC uses protocol buffers, Google’s mature open source mechanism for serializing structured data.
gRPC uses HTTP2 and takes the advantage of higher and better performance through that. 

#### Protocol Buffers
Protocol buffers are Google's language-neutral, platform-neutral, extensible mechanism for serializing structured data – think XML, but smaller, faster,
and simpler. Protobuf is a binary format created and surpasses JSON in performance. Protobuf performs up to 6 times faster than JSON.

##### Protobuf over JSON
1. **Saving Network Bandwidth** : Protocol Buffers are more efficient than JSON. The payload is serialized and is also smaller in size.
1. **Parsing Less CPU-Intensive** : Parsing JSON is high CPU intensive, Protobufs are in binary and hence is less CPU intensive.
1. **Simpler and Decoupled** : Easy to write message definition and APIs are independent from the implementation.
1. **Less Boiler-plate code** : as the code is auto-generated and need not be written by the developer.
1. **Easy Extensibility** : New fields could be easily introduced, and intermediate servers that didn’t need to inspect the data could simply parse it
and pass through the data without needing to know about all the fields.
1. **Interoperability** : Protocol Buffers are implemented in a variety of languages, they make interoperability between polyglot applications in
your architecture that much simpler.

##### JSON over protobuf
1. You need or want data to be human readable
1. Data from the service is directly consumed by a web browser
1. You aren’t prepared to tie the data model to a schema.

#### HTTP 2
The primary goals for HTTP/2 are to reduce latency by enabling full request and response multiplexing, minimize protocol overhead via efficient
compression of HTTP header fields, and add support for request prioritization and server push.
To implement these requirements, there is a large supporting cast of other protocol enhancements, such as new flow control, error handling, and
upgrade mechanisms.
HTTP/2 does not modify the application semantics of HTTP in any way. All the core concepts, such as HTTP methods, status codes, URIs, and
header fields, remain in place.
Instead, HTTP/2 modifies how the data is formatted (framed) and transported between the client and server, both of which manage the entire process,
and hides all the complexity from our applications within the new framing layer. 

### gRPC best practises ?
1. Use protobufs over JSON
1. Follow protobuf3 styleguide
1. Reuse gRPC channels : create a single connection and use it in the application lifetime.
1. Load Balancing : Client Side and L7 application load balancing. L4 does not work that well.
1. Use streaming wherever the use case permits : 
    * High throughput or low latency is required.
    * gRPC and HTTP/2 are identified as a performance bottleneck.
    * A worker in the client is sending or receiving regular messages with a gRPC service.

#### REST vs gRPC ?
1. Protobuf advantages over JSON if using JSON with REST
1. HTTP 2 advantages over HTTP1.1 (Multiplexing, Compressed headers)
1. Streaming APIs

### How does gRPC-Web work ?
The basic idea is to have the browser send normal HTTP requests (with Fetch or XHR) and have a small proxy in front of
the gRPC server to translate the requests and responses to something the browser can use.