# Microservice Training Notes
- Initially we had 3 layers: Presentation, Business and Data Access. This ensured separation of concerns.
- Tight coupling of components makes changes hard.
- Monolithic apps take more time to build and deploy.
- Then there was a shift to Service Oriented Architecture
- Issues with SOA
  1. Based on SOAP, SOAP ignores response codes except OK and Internal Server Error.
  2. Thin Aggregation Layer(We have to do lot of transformations of XML) . This introduced new level of coupling.
- Microservices based architecture are not a silver bullet, but they are more agile and extend into a cloud native world.

# Monolithic Application
- Large single file deployments. All components go into one single DLL, which had both related and unrelated components.
- A single deployment would deploy everything like web app, business apps in one single way.
- What if we have to make a change to monolithic application, we have to do regression testing of the entire component as part of the release. Every code change or fix or enhancement requires extensive testing.
- So we package together bug fixes, enhancements into one big release packaged together which impact our time to market.
- Deployments of monolith can take days to accomplish and even more time in UAT.
- Agility and maintainibility suffer a lot in a monolithic world. Also scaling out is very hard. We have to scale out everything.

# Advantages of SOAP
- SOAP is not inherently bad. Verbosity of XML adds some value. SOAP enforces strong contract. WSDL is the best part of SOAP.
- It provides an inherent business documentation.
- Problem lies in it implementation. Another problem is everything is either OK(200 response) or Fault(500 response.) There is nothing in between.

# Why Microservices
- Microservices is about decomposing the system into discrete units of work.Basically breaking down the problem.
- All communication over REST
- Allows polyglot development support
- Any microservice can be called from another microservice
- There are costs of moving to microservices based architecture.
- How we setup communication between microservices is also important. We tend to risk congestion. A single slow call can cause thread blocking. If one microservice is sick, then it can cascade quickly to the dependent microservices. We have to focus on reliability.
- Cloud native architecture include patterns for designing systems to run in a cloud based infrastructure. Cloud based infrastructure provide high availability ,scability and global distribution.


## Things to conside while moving to microservices based architecture
- A microservice based app is stored in a single code base. It is usually a self contained unit making move to cloud easier.
- Size of the microservice doesnt matter. Operations matter. All communications between microservices use REST or HTTP. Some use grpc or GraphQL.
- Interservice calls are always over HTTP.
- A microservice handles one set of operations without any cross domain communications.
- Using HTTP makes consuming of microservices also easy. DDD comes into play.
- In OOPS, a class is meant to provide one type of thing. Microservices also operate on a single domain. All operations revolve around that domain.
- A domain should be domain operation focussed. When people build microservices, either they are too fine grained or not fine grained enough. **This can lead us to create anti-patterns**
- Distribution tax hits us hard.

# Communication between microservices
- Communication between microservices can also be source of great pain.
- Protocol Aware Heterogeneous Interoperatibility. This explains services are bound to a protocol and all communication is over that protocol only.
- All services expose endpoints which can be accessed over HTTP.
- ach service is capable over calling any other service.
- Maintain passive APIs.
-  Versioning Strategy is important and so is unit testing.

# Scaling of Microservices
- Microservices are also highly scalable. Putting services all over the world is costly.
- A microservice based architecture allows us to increase instances of the service.
- We can do both horizontal and vertical scaling.
- Traditionally for monoliths, scaling was done for the busiest day.
- Cloud native architectures allow elastic scalability, which is hard for monolithic apps.
- here is latency in making HTTP requests and responses.
- It increases exponentially in lot of calls. Can cause gridlocks.Must look for solutions for this.
**We can use a circuit breaker pattern.**
- When timeouts start occuring, we can trip the circuit and execute default behaviour.
- It is better to do this than to cause full failure.
- Require strong timeout logic
- Scaling important microservices under load is important.

  # Domain Driven Design
  - Investigate the working system and determine the domains.
  - Break the services up. Find granularity by using DDD.
  - Analyze traffic patterns in your code and develop real world use cases.
  - Telemetry is very useful.
  - Reduce cross domain calls wherever appropriate. A customer domain is different from user domain.
  - Latency is a pain point in microservices. Reduce the distribution tax.
  - Strong contracts and well defined boundaries allow for self-discovery.
  - Transactional Boundaries is very difficult.
  - Decomposing monolithic database into smaller databases is very hard.
  - Break down the database into smaller databases and build microservices for each.
  - Migrating Data is significantly harder

  # Moving from Monolith to Microservices based application
  - If we are building microservices for the first time, build the microservices first and ensure they all connect to the same monolithic database.
  - This helps us to analyze the data model and model the data itself.
  - Building data domains is hard but it is important.
  - Good telemetry and tracing pattern is very important. 
  - Traditionally systems built ACID complaint systems. Consistency here ensures all constraints are enforced. Isolation ensures data cant be read until a specific state is reached.Atomicity ensures all operations either succeed as a whole or fail as a whole. Durable operation is guaranteed to be in datastore unless modified.
  - **In microservices based architecture, ensuring ACID is impossible. Therefore, we strive for BASE(basically available instead of ACID. we strive for eventual consistency. We are not guaranteed immediately ACID transactions. ACID is easier as we are guaranteed of its state.**
  -  Do we really need ACID ?
  -  Do uses need immediate access to data?
  -  Banking system however needs ACID say in case of debit and credit to a banking account.
  -  But what about the loans process? There we can use eventual consistency model.
  -  Eventual consistency also allows for rollbacks.

# API Layer
- Aggregated proxy of all of our service offerings.
- Abstracts what services exposes which operation
- Provides a standardized interface.
- API Layer abstracts the client from knowing the ip address or port of the particular microservice.
- Clients will have minimal changes to make if the services are exposed through a proxy layer.
- Zero impact on them unless there is a breaking change

# Reducing Latency in Microservices
- Embrace Asynchronous Communication
- Need event driven microservices--use message queues
- Stream Data Platforms is very powerful. Useful in large distributed systems.
- Here events trigger multiple operations. Examples: Logging, Security. When moving to async operations we need to handle error states correctly.
- Dead letter queues must be monitored.
- Need to check performance of message broker also.

# Why Logging and Tracing is important ?
- Plan for observability early.
- Plan for unified logging strategies early
- Helps in troubleshooting
- We need convergence in your logging behaviour.
- Must make effort to move to log aggregators and make uniformity critical.
- Tracing creates a unique token and involves moving that token through the call stack. Each service uses the trace to make calls to other services. TraceID must be there in all log messages (CorrelatioID).
- We can aggregate log messages based on this TraceID.

# Why Continuous Delivery is important ?
- Helps maintain agility.
- Build a continuous model early in the process
- Chances of success diminish if there is no CI/CD
- A CI/CD builds the code base(dotnet build), executes unit tests, add automated deployment to a non production environment(staging). Here we run integration and security tests.
- When code is in production, we can use blue-green deployments to push the requests to the new code.
- Try to automate as much as possible.
- Goal of microservices is agility.
- Start small, integration of testing frameworks.

# Hybrid Architecture(Partial Microservices based Architecture)
- Utilize a hierarchical service model to implement a partial microservices architecture.
- Here we define rules as to which service type can or cannot consume other service types.
- N Tier Model
- Define few classes of services. (Edge Services, Business Process Service, Data Services, Gateway Service)
- Next build out rules over which service can consume other service.
- Service Based Architecture
- Single Underlying Database
- Gain agility without sharding data
- Must have well defined domains.

# Design Considerations for Microservices
-  CI/CD Pipelines (most important)
-  Logging, Tracing and Telemetry Frameworks(Must be in a common library)
-  Consider Log Aggregators and Search(ELK Stack)
-  Domain Driven Design to define service boundaries
-  Consider what functions these services will perform
-  Do we need ACID Transactions or BASE
- Evaluate Latency and Control(async code or message brokers)
- Most successful teams have standards across the codebase
- Standardization allows us to shift resources
- Design system to be async first
- Every service needs to be an async operation
- Desigining microservices is less about code but more about operations and infrastructure.

# Pros of Microservices
- Well Defined Boundaries
- Allows better scaling
- Reduces waste
- Allows diversity in technology stack

# Cons of Microservices
- Distribution Tax Cost
- Increases complexity of deployment especially with scaling

# Considerations while building microservices
- Business Layer allows us to use Message Brokers
- API Layer should only be used as a proxy layer. Cannot transform service offerings based on client needs
- Two types of Edge Services
  1. **Outbound Edge Service**-->Expose client specific needs to outside world
Not everyone needs a big enough payload. Use an API Gateway. Setup client specific edge services. It makes the transformations easy. Provides clients a consistent interface.
  2. **Inbound or Translation Service** --> Design to abstract us from third party dependencies.
- Consider an email marketing service
  1. Use a third party service for emails and SMTP operations
  2. Build an edge service  which interacts with third party service, this abstracts ourself from vendor implementation. We can replace vendor also
  3. All other services can call our edge service without knowing details of third party service. This can be an async operation.

# Why monitoring of microservices is important
- Monitoring must be done for microservices and ensure lag doesnt have major impacts.
- Platform for continuous monitoring is important(Istio Service Mesh).
- Setup Alerts. Automation of deployments and testing improves agility of team and increases throughput for the team.

 # Final Thoughts
 - Microservices is very hard to deal with for small teams.
 - Start at lowest point and build our data to be broken up
 - Expose Domains with Solid APIs
 - Should breakup databases as well.
 - Encapsulate as many of potential services as possible
 - Keep a scalable and distributable system in mind every step of the way.

# Microservice Design Patterns

## Different types of services
- Data Service: Connects to a data source within the system
- Business Service: Abstraction that builds on data service. Sometimes business domains encapsulate two or more data services
- Translation Service: Abstracts on a third party operation
- Edge Service: Responsible for delivering data to end users

## Platform: Arena for all service operations across data centers
- Microservices do not make a system cloud native and cloud native is not only for microservices
- Cloud Native can run anywhere
- Cloud Native is an architecture style. Doing processing and building systems to facilitate a goal. It helps in externalizing configuration, focuses on scalability, has fast application startups and immediate shutdowns.
- Cloud Native apps are portable and scalable where apps run as a single unit or multiple units. Allows for auto scaling up and down. Microservices break up existing endpoints into independent units of work.
- We need both cloud native and microservices but both can exist independent of each other.
- Use Divide and Conquer, divide problems into smaller problem statement.
- In Microservices, we first build domain based services. In Financial Systems, we need to build decomposition model around the atomic system itself.

# Types of Microservices
- **Domain Based Microservices**
Based on domain driven design
We must decompose at domain level. Focus on serving the data and apply logic to the domain itself.
What if a domain share functionality with another domain
In domain driven design,
1. Start with the domain model
2. Evaluate the actions that need to perform.
3. All of this yields the service definition itself.
4. Consider storage of data
5. Consider Model A with Actions A,B,C,D. All these actions are exposed as an API.
6. Build the datastore to store the data.

-  **Business Process Based Microservices**
- Helps us build a more structured microservices architecture.
- Business Process Domains provide higher level of abstraction built around specific business    functionality.
-  Helps to encapsulate various domains.
-  Business Process services have no data access of its own.
-  But please dont build out all of business processes into a single domain package.
-  This is like layering a monolith.
    1. Identity the process to expose.
    2. Identify the data domains you will need to consume
    3. Define the APIs(contract).
    4. Encapsulate the business process code into its own module.

## What if you need atomic transactions in microservices where eventual consistency is not enough.
- Generate ACID complaint transactions across more than one data domain.
- Provide cross support services that supports rollbacks and callbacks.

## Atomic Design Considerations
1. Do services need to be atomic?
2. Domains must be in shared database.
3. Get the transactions definse, including rollback conditions
4. Implement the service as normal with fast fail and rollback.
5. If we can avoid atomic based services, please do.Eventual consistency is the best


# Design Patterns
## Strangler Pattern: Breaks down monoliths rather than writing new services.
- We start with a monolith and shard your microservices piece by piece into new microservice endpoints.
- We can start at data store itself and break it down.
- We can carving functionality out of monolith and replacing it with a  microservice based artifact.
- WE first define 2 business processes within the monolith.
- Then define 3 distinct data access areas.
- We need to build a new database and build our data domain. Then we move our client to move to this new domain.
- We strangle the monolith completely from being consumed from clients and then we deprecate the monolith.

## SIDECAR PATTERN: Allows offloading many operational security functions into separate functional components. These components deploy alongside the main component.
- Deployed as a module associated with every applicable microservice in our system.
- Removing repetitive code.
- Logging, monitoring, networking services are offloaded to a separate module.(remember Istio Service Mesh)
- Start with the process itself.
- Write your code.
- Schedule your sidecar deployment. This is done with help of service definition.
- Sidecars are useful in containerized deployments.
- We can add logging,monitoring and security sidecars to our services.
- Sidecars are generic and we can apply it anywhere.
- Sidecar reduces duplicate code

## Gateway Pattern
- Ingress Pattern for clients communicating with microservices
- Designed to provide a buffer between underlying services and client needs
- It can customize the payloads from the services as per client needs(remember find and replace in AzureAPIM)
- All security and authorization logic can be put in a single ingress point.(validate-jwt)
- Decorate payloads, customize headers
- Perform aggregations(get data from multiple microservices and combine data) but donot apply business logic..do only simple aggregations
- Rate Limiting
- Gateway Patterns provide insulation as the public contract remains the same, but underlying implementations may change.

**Building gateway pattern**
- Define Contracts
- Expose APIs in gateway component
- Use strict version control
- Implement Gateway

## Process Aggregator Pattern
- Simple way to develop complex process
- When multiple domains need to be called together we use a business process service.
- We see a frequent need to call 2 or more business processes at same time.
- This pattern provides a single API call, which calls several business process services. This API call aggregates the response from multiple business process services
- API call may introduce its own processing logic
- However can cause long blocking calls
- We need to use asynchronous calls
**To build the aggregator**
- Determine the business processes
- Define the processing rules
- Define a new model for aggregator
- Implement API based on that model (use Standard REST verbs)
- Wire the service together and implement the internal processing.

## Edge Pattern
- Subset of gateway pattern
- Basic problem it solves is that API Gateway scaling becomes a concern as the different type of clients and their needs grow.
- Client needs specialized business logic which only applies to that specific.
- Edge patterns are client specific gateways. They docus on isolating calls for specific clients.
- More flexible for new clients, just make a new service for each client
- However maintenance is a concern, how many edge services will you maintain.
- Add OAuth 2.0 to maintain security.

 **Build the Edge Pattern**
 - Identify the needs and constraints of the client
 - Build Contracts
 - Implement API and contracts
 - Maintain passivity as long as client is needed.

**SCALING IS EASIER IN EDGE PATTERN AS WE KNOW WHEN THE CLIENT NEEDS GROW AND WHEN THEY COME DOWN**

# Data Patterns in Microservices

## SINGLE SERVICE SINGLE DATABASE PATTERN
- Problem it solves is that scalability needs between database and service are related. If load on a service increases, it will also increase load on underlying database
- Each data domain gets its own dedicated datastore.
- As we scale the service, we need to scale the database also

## SHARED DATABASE PATTERN
- 2 or more services use the same database.
- Data distribution should be handled by database.
- Structure data to isolate it and prepare for eventual separation
- Each service that consumes the service should have unique credentials.
- Data domain should connect to a single schema.
- Useful for startups, as we can focus on new features. Also useful if you want to break the database up at a later point in time.

## CQRS Pattern
- Those who get it well can make their lives easy
- Data access patterns diverge from traditional CRUD to more complex multi-model patterns
- Query interfaces may transform and aggregate the schema to represent the model.
- We make Task build UI operations
- Eventual Consistency is the target. We cannot guarantee that the data you write is the data you read.

## ASYNCHRONOUS EVENTING PATTERN
- Sometimes long running transactions or complex workflows cannot fit into a single blocking API call
- A service API triggers an event
- Event can cascade asynchronously
- This is powerful in a distributed system

## OPERATIONAL PATTERNS

### LOG AGGREGATION PATTERN
- Need to know what is going on with our system
- Logs Provide detailed information about microservices
- Logging must be consistent across all services
- Consistency across services is critical in structure and format.
- Make efforts to make logs consistent.
- Must inject tracing identifier across different service calls

With microservices we must store our logs in a centralized database(ELK Stack)
Must make efforts to do structured logging, easier to parse and search and easier to find out and fix issues

###  METRICS AGGREGATION PATTERN
- More useful than logging
- Less human interaction, just some instrumentation
- See what is going on with the system as a whole
- We can view metrics graphically on Dashboards (Application Insights, Azure Monitor)
- Build high level dashboards.
- Build detailed dashboards for each service.
- Inject events especially deployments into the dashboard.
- Trace alarms on your graphs

### Tracing Patterns
- We need them because in a monolithic system all calls from edge to database are in a single process. We can dump the stacktrace
- However in a microservices based architecture, we cant just do that. So we need to inject tracing identifier. We should track everything to database calls or any dependent service calls with it.
- Must use standards based approach
- Inject Trace ID at entry point of your system
- Every log message should embed the trace ID
- Put it in the Header
- Visualize the call stacks

### External Configurations
- Must write configurations outside of code
- Kubernetes have ways to inject configMap into the app
- Consistent naming is very useful
- Must try to expose configuration as much as possible but protect secrets
- Service written to favor embedded values

### SERVICE DISCOVERY
- Save time in a dynamic runtime
- Problem: What service i need to call?
- As each service comes online it advertises what contracts it exposes
- We can query the central location to find the services we want.
- Client can call Service URI from discovery not from config
- Consul, Eureka
- Netflix has exposed components for service discovery

### CONTINUOUS DELIVERY
- Process by which we deliver new code to production with full automation
- Move code from non production to production using gates
Steps:
1. Trigger code to move after artifact has published
2. Automate integration tests
3. Deploy integration test suites along with the tests(SonarQube)
4. Add Security Testing(third party tools) Penetration Testing framework is run then internal security tests is run. OWASP top 10 and Synk Security Tool
5. User Acceptance Testing
6. Do smoke tests in production
7. CI/CD is a basic requirement of microservices

### DOCUMENTATION
- It is critical. Having solid documentation for APIs is important.
- For Restful services use OpenAPI to document contracts
- C# has built in support for OpenAPI
- Expose documentation as a consistent location (/docs) swagger docs
- Populate central documentation repository
- Include examples of API Consumption (remember Developer Portal, SwaggerJSON)
- Improves reuse of code and ensures code is well discussed

# MICROSERVICES SECURITY

- In a monolith all services communicate in-process whereas in microservices all communication occurs over HTTP. Access to microservices occurs through REST API.
-  For Fault Tolerance, we have Circuit Breaker Pattern in Microservices. E.g Polly

## Security Fundamentals
- Confidentiality -->Ensures information is private
- Integrity -->Ensure information can be trusted
- Availability -->Ensures information can be available to authenticated users when they need it.

### Access Control: : Authentication and Authorization
- Authentication-->Establishes user identity (user or machine). Highly sensitive systems may apply multi-factor authentication like OTP or password
- Authorization-->Identification and enforcement of privileges (May be delegated to 3rd Parties through OAuth)

## Attack Surface: Ports, APIs,DB (all are vunerable)
- In a monolith, the attack surface is limited. Requests entering the system go through a security filter. Security Context is shared across all components. All components share the same trust domain in monolith.
- Distributed systems are made of independent components. Each component or microservice has a port. So we have a wide attack surface and it is dynamic in nature. Access Control is a challenge.
- Building authentication within each service is very challenging. One microservice calling another microservice would have to revalidate each call.
- EAST-WEST TRAFFIC-->Traffic between microservices
- NORTH-SOUTH TRAFFIC -->Traffic entering the system
- Need to ensure confidentiality of traffic between various services. For e.g a DDOS attack on one service would affect only that service. All other services can work normally. This is a security advantage.

## Access Management Patterns
- Token: Digital Keys(GUIDs or detailed info tokens) like JWT Token. Identity Service manages the authentication and issues tokens
- Services call Identity Service with the token to verify its authenticity. It can cause lot of service traffic to the identity service. One solution to reduce this traffic is to route it to a reverse proxy. The reverse proxy will talk to Identity Service and then validate the token and allow access to the microservice, but this also has challenges. However, implicitly trusting the network is also not good.
- Now the token from the Identity Service will pass the identity and privileges to the microservices. These service will check these privileges and provide access accordingly.

## What do Identity Services really provide?
- Identity Services provide 2 main classes of services: Authentication and Token Management.
- Best way is to outsource the identity services.
- Azure AD also supports MFA Authentication like OTP or biometrics. It also provides identity management.Users can also delegate permissions. We have OAuth2.0, JWT, Open ID Connection(OIDC)

## Reverse Proxy or API Gateway
- Single Entry Point of Access.
- These Gateways validate the client token by checking it with Identity Platform.
- We can also enforce access control in gateway.
- We can also do rate limiting where one client has limited access (429 error code). Too many requests received.
- We can also do request tracing and performance monitoring.
- API monetization
- API Gateways come with Developer Portals.It can integrate with Identity Providers. We need to register our subscribers.
- API Gateways can be onprem or on the cloud.

### Microservice Access Scenarios
- Microservices are not accessed by end users.
- All calls come from API Gateway
- Clients can be public clients like SPA(credentials can be accessed by attacker) or confidential clients(server side apps)
- An endpoint can also request access to the microservice.
- Microservices must support machine to machine access scenarios such as Client Credentials flow.

### Types of Tokens
- Reference Token: Reference Token is an opaque string that doesnt contain any meaningful data. It is passed into a request and used to look up for token metadata in a repository in IAM platform.
- Structured Token: Structured token contain the token and the claims which are grouped together in a claim set.
- JWT Tokens has 3 parts(Header,Payload and Signature)
- ACCESS TOKENS: Allow the bearer of token to access an API
- REFRESH TOKEN: Allows new access token to be obtained after original has expired.
- ID Token: JWT Token contain information about authentication event and user identity.

All tokens have a lifecycle. Tokens are difficult to manage.

## OAuth2.0
- We have the following actors:
1. Resource Owners(end user who owns the info in a microservice)
2. Resource Server(server hosting the API)
3.  Authorization Server(issues Access Token to client)
4. Client is an application that access resources on behalf of an owner

Clients call the Authorization Server and uses the Access Token to access the resource server.(Hotel Scenario getting the keycard access to the room)

## Types of GRANTS
- GRANTS are the sequence of steps taken to issue an access token to a client
- It outlines the HTTP calls and parameters exchanged among client, resource owner and auth server
- Client specifies the scope of access request
- Biggest advantage is that the microservice doesnot have to handle authentication
- We have the following GRANT Types:
  1. Authorization Code Flow
  2. Client Credentials Flow
  3. Implicit Flow
  4. Resource Owner credentials

### Authorization Code Flow
- Client and its redirect URI must be registered with authorization server.Registration is done using registration form in developer portal. Once form is completed a clientID and clientsecret are issued

### Client Credentials Flow
- Used to get token for machine to machine scenarios. Client ID and secret and their grant type is passed to auth server. It receives a token.

### Open ID Connect
- It is a thin identity layer that sits on top of OAuth.OIDC Providers are linkedin, microsoft,google etc.Identity Providers can also authenticate users to authenticate applications. Basically its an authorization server that meets OIDC Standards. They expose the following endpoints:
- /authorize
- /token
- /userinfo -->returns claims about authenticated users

OIDC issues an ID Token and Access Token. ID Token also includes claims about the user. Access Token can be passed to userinfo endpoint to receive claims about the end user

#### Steps in a OIDC Flow
 1. Make a request to the identity provider(Google,LinkedIn)
 2. After authentication, a code is returned to the client
 3. Client can exchange that code for ID Token and Access Token
 4. Call the authorization server and pass that code
 5. Get back an access token and ID Token(starts with EYJ..thats a JWT)
 6. ID Token has claims about end user.

This really helps us to establish user context across microservices.

### Considerations to be kept in mind for a Token
- Tokens must be validated
- Token is held by the client between requests and it is passed to resource server with each request.
- Validation depends on type of token. If it is a reference token, send it to auth server to learn more about the state of token or if its valid or not .Auth Server must have clustering and caching in place to handle heavy loads. 
- In JWT.IO we can validate the token with help of a key(symmetric key in our case) and check if the signature is verified corresponding to that key. All JSON Web Tokens correspond to the JOSE specifications.
- Each token has an expiration date. This is set through expiresIn claim in JWT. JWT Token should be short lived.
- For long lived scenarios use a Refresh Token. Every time an access token is obtained using a refresh token, refresh tokens should be rotated.
- When a logout occurs all tokens should be revoked. We also have an endpoint through which a token can be revoked.
- Transport everything over TLS. Tokens should be protected like passwords. Same sort of protection must be for client ID and client secret. Always set an expiration date. Dont include highly sensitive info in payload of token

# Security Between Microservices
- Communication between microservices must be based on mutual TLS (mTLS)
- Use the concept of zero trust. Trust boundary should be the microservice itself
- Digital Certificates should be used. It identifies the identity, the public key and Certificate Authority. Certificates should be exchanged.They should be validated against Certificate Authorities.
-  SSL certificate verifies the identity of the site.Any info that is sent is not in clear text. 
- In microservices we take concept of security to another level by implementing mTLS. Both parties authenticate using a digital certificate issued by a CA before a secure channel is established(remember GRPC)
- In Mutual TLS, each microservice is deployed with a certificate and a private and public key pair that allows them to identify each other.
- Use mTLS to secure communication between API Gateway and Microservice.
- **Biggest challenge of implementing mTLS is management and provisioning of certificates.**
- Most services are hosted inside a container.
- How certificates are handled before they expire. This is done with container orchestrators and service meshes.

### How to secure traffic between microservices(EAST-WEST Traffic)
- Microservices consume other microservices.
- We cannot use the same pattern used to validate access to service 1 also be used to validate to service 2. This is an anti pattern and is like a distributed monolith and causes tight coupling and violates concept of least privilege.
- Service 1 can do client credentials flow to get an access token to talk to Service 2. However this is not ideal, now Service 2 doesnt have knowledge of the original user who issued request to Service 1. (Use Delegating Handler in .NET)
- We can nest one token inside another when calling service 2 from service 1. This is called Token Switch pattern.
- To secure communication between microservices do these steps
  1.  Make sure the client uses a reference token
  2.  When token enters the network, use the IAM to switch the reference token for a structured token with claims of the user.This          can happen at the API Gateway.
  3.  Now microservices can securely communicate without storing any state between them. 

## Logging and Tracing between Microservices
- Logs can show suspicious behaviour.
- Can establish audit trails
- We can use correlation ID with each request
- We can reassemble the event at a later point of time to support a security inquiry.
- Common Logging structure must be included.
- All errors must be logged along with debug information.
- Send all logs to a central host (LogStash)
- Use Elastic Search and Kibana.
- Automated monitoring can generate alerts as well.

# Usage of Service Mesh to secure communication between microservices residing inside a container
- Service Mesh is used to manage the complexity of service to service communication
- Prerequisites:
  1. Microservices must run inside containers with docker
  2. Must be deployed in Container Orchestration System like Kubernetes
- Build a security mesh which is a network of proxies that sit next to container(sidecar proxy). These proxies intercept the traffic entering and exiting the microservice providing an excellent point to apply security tactics like mTLS, Access Policies and Audit Logging.
- With many interconnected microservies, we establish a mesh of proxies(service mesh)
- Mesh is transparent to microservices
- Mesh intercepts the traffic between them
- **Istio by Google** splits into a data plane and control plane.
-  Data plane contains the set of proxies and policy and configuration is managed by control plane.
-  Using pilot the control plane can push it to data plane and enforce authorization or claims in JWT  passed to microservice.
- We can create policy to secure our microservices.
- Istio allows better monitoring and improve observability by using TraceID and RequestID.
-  Istio also has access logging of its own.
-  **Istio provides mTLS out of the box.**
-  It bundles a Certification Authority Citadel into control plane.
-  It can mount certificate header and key inside a kubernetes pod when it is created.It can also automate rotate the certificates inside the pod.
-  Istio Mesh secures our microservices and improves logging.

# Application and Container Security

## Throttling and Rate Limiting
- Controls usage of API Clients. Limits traffic to avoid complete outages.
- Think of it like a speed limit for APIs.
- Create a quota and rate limit for each client. Rate Limit prevents bursts of calls. For APIs that are monetized we can set the quota accordingly.
- We can also limit by user(Remember Limiting by Subscriber ID in Azure APIM).
- We can apply throttling limits for some operations which are resource extensive.
## Container Security
- Microservices are deployed inside containers. Security measures can be applied on container runtime.
- Must remove non essential applications inside a container.
- Configure container to run with least amount of privileges necessary.
- Never run container using the -privileged flag.
- Running container using read only file system and volumes is a great way to reduce attack radius.
- Docker Bench can check for security misconfigurations.
## Container Image Security
- Images are built from Dockerfile.
- Images are stored in container registry where it can be pulled and run as a container.
- Always pull the image from a trusted source.
- It is possible to insert malicious code inside a Dockerfile image
- Must use the image's latest version.
- Providers of images regularly update them.
- Snyk tool can detect security vunerabilities related to an image.
## Secrets Management
- Kubernetes provide better approaches to store secrets. It has built in secrets management store.
- Secrets can be injected as an environment variable or we can write a file to a volume.
- However now we cannot share secrets outside of microservice cluster.
- Best option use Azure Key Vault
## Pipeline Security
- Pipeline is a function which accepts commits from developer as input and outputs a container which contains the software the commit was made against.
- We can introduce security gates inside the pipeline.
- Use Static Code Analysis tools like SonarQube which can alert about security issues.
- SonarQube must run on CI Build.
- Ensure a PULL Request Model in place.
- Snyk can integrate directly into our pull request.
- Run Unit and Integration Tests
- Inject some interactive security testing.
- Use Container Registry Scanner.
- Access to artifact repository must be closely guarded.

# Asynchronous Messaging in Microservices
-  Communication between microservices can be through asynchronous messaging.
-  Most often done through messaging.
-  Main Asynchronous communication is done over:
   1. HTTP over REST
   2. Polling or Push
   3. Messaging Systems
   4. Drop a message and move on
- In a standard model, communication between microservices is over HTTP using RESTFUL patterns.Calls are synchronous in nature.ach      call becomes a blocking call and has to wait for response. Call paths can be deep. Useful in small operations but complicated in      multi service communication.
- While implementing Asynchronous Communication, all communication is over HTTP over REST. Server has to poll to check the status.

## Benefits of Asynchronous Communications
- Offload Strain
- No impact to customer
- Improve user experience
- Improve system health
- Can have retries without affecting performance
- Prevent Gridlocks
- Avoid congestion of network
- Avoid slowness and cascading slowness
- Can have long running processes
- Avoid timeouts
- Reduce Coupling
- Communication is only through contract of the message
- Improves fault tolerance
- No more message blocking or timeouts

In most microservices, response is not needed immediately. We aim for eventual consistency.

## Cons of Asynchronous Messaging
- Complexity by artifact sprawls.
- Disconnected Code Paths
- Makes debugging, troubleshooting more complex
- More complexity in fanouts
- Observability complexity also increases(more important to log correlation IDs)
- Have to inspect logs and logging is very important.
- Correlation of metrics is also a challenge
- Additional components and each new component adds overhead
- Have more containers running which also increases cost
- Sometimes finding source of issue is very complex. Root cause is tough to find.
- Tracing issues through callstack is much harder.

## Message Broker
- It is at the heart of asynchronous communication
- Does Translations
- Routing
- Aggregations
- Errors(Dead Letter Queues)

Common Message Brokers:
1. RabbitMQ
2. JMS
3. Apache Kafka
4. Redis Cache(Not the best)

**PRODUCER**: Creates the message and dispatches to message broker
**CONSUMER**:  System that receives the message but can also be a producer for another message downstream.
**Dead letter Queue**: Here the error messages go.

## Interservice Communication Patterns
- Point to Point (Single Producer and Single Consumer)-->This is send and forget. Responses are also point to point
- Publish Subscribe(Single Producer but more subscribers who listen for messages). Multiple consumers of same message. This is also send and forget.
- Durable Subscriptions(If subscriber is durable, there is a guarantee of atleast once delivery)

### POINT TO POINT MICROSERVICE COMMUNICATION (Most Common)
- No response is needed, client doesnt need to know response, we can produce a message and fire it off
- Useful for audit records which are not mission critical
- No guarantees or whether it is done with proper manner
- Easy to Scale
Examples include:
  1. Admin Tasks in application
  2. Sending Emails

PRODUCER -->MESSAGE BROKER -->CONSUMER
There will be a DLQ in this case also, we need to monitor this queue.

### PUBLISH SUBSCRIBE MICROSERVICE COMMUNICATION
- Examples include
   1. Multiple Responders
   2. Triggers to search multiple search indices
   3. Multiple Tasks

Consumers can decide which messages they can listen for.(Apply filtering)

### DURABLE SUBSCRIBERS MICROSERVICES COMMUNICATION
- Always get the messages
- Subscriber has to acknowledge it
- Powerful in mission-critical operations
- Can allow nothing to be blocked yet each message is guaranteed to be processed.
- Subscribers can unregister also
- Done in Producer Agnostic way
  PRODUCER--->MESSAGE BROKER--->Subscriber 1
                |
                |
               \|/
           Subscriber 2

  ## EVENT DRIVEN MICROSERVICES
  - Series of Steps
  - Triggered from a single event
  - There is a check to ensure all dependent components are working fine
  - Move towards an end goal
  - Each piece has a role to play
 
  ### Choreographed Events
  - There is a call tree
  - Each step does some work and passes work down the chain
  - Cascade down the pipelines(similar to MongoDb integration pipelines)

  ### Orchestrated Events
  - Examples include
      1. Sequential Processing(like user registration or loan approval process)
      2. Command Workflows(Dispatch events to worker process and sends response message, then we can dispatch another message) For             e.g setting up Kubernetes multi-cluster
      3. Response Required (Can create aggregate of responses)
  - Pros
     1. Increased Reliability
     2. Better error handling
     3. Better observability
  - Cons
     1. More expensive
     2. Decreased Performance(Everything has to go through orchestrator)
  - Very common
  - Centralized command and control
  - We can invoke the steps as we need to
  - We poll for results
  - Still based on isolated steps and each step is distinct and each step has a job to do

  Event Producer(Polls the orchestrator for response)
    |
    |
   \|/
Orchestrator -->Message Broker--->Step
                     |
                     |
                    \|/
                    Step

  ### Choreographed Events
  - Initializer of event is the choreographer
  - Use cases include:
      1. When we have different systems in play
      2. When we have silos of teams
      3. Alternative cascades (Each step has results that trigger more than one next step)
  - Benefits
      1. Increased performance
      2. Reduced Cost
  - Cons
      1. Reduced Reliability
      2. No centralized orchestrator(makes logging difficult and traceability)
      3. Reduced observability

  Event Producer(Choreographer-creator of message) -->Message Broker-->Step
                                       |
                                       |
                                      \|/
                                      Step

   
### Hybrid Event Driven System
- Centralized command and control
- Remote choreography
- Work gets done
- Contracts via message broker is the key
- We need to have well documented contracts
- Contracts must be passive to change
- Read the contract and honor it
- If there is a breaking change, send it to DLQ.

### Stream Data Platform
- It handles streams of data comes from structured log messages
- Increased need for disk space
- Activity is handled asynchronously
- Very useful in microservices
- Similar to pub/sub model
- **Producers**: Applications produce logs, producers of the messages, Databases can also produce logs and events, Servers also produce logs, Everything else that produces messages and logs
- **Consumers**: Log Aggregators(LogStash), Correlation and Tracing IDs help the Log aggregators
- **Analytics Engine**: Long term storage vehicles like Data Lakes, Eventing Engines(trigger downstream events)
- **Log Aggregation**: Message Broker can aggregate logs from different systems, Keep all logs in original format, Readability of log messages, Consumption Engine

**ELK Stack** is the best one for this
- Can handle real time log aggregation
- We can visualize the stream of events
- Trigger Alerts
- Create Alerts
- Useful in debugging
- Log visualization can also help in refactoring

Stream Data Platforms read from all environments and not just production
#### System Analytics
- Stream Data Platform is the best place to do analytics(gRPC)
- Allows data to be analyzed quickly (Apache Spark)
- Quicker Responses and dashboards can be built quickly
- Analytics should be built within microservices but the company must invest in it
- Value isnt immediate
- Return on investment takes time
- Need to do event detection and analysis to stop malicious attacks
- It is also useful to identity patterns

Producer                                            Consumer
Producer        Persistent Message Broker(Kafka)    Consumer
Producer                                            Consumer
Producer                                            Consumer

### Data Flows in Microservices
- Can range from distributed data and eventual consistency to CQRS based data rights to improve throughput.
- Data is slow(Reading,writing of data are all slow processes)
- Data is critical to operation of any system
- Need to store state
- Data grows daily
- We need more data to be effective(Data is the new Oil)
- ACID is painful in a microservices world
- BAS: Basically Available (Eventual Consistency) (S)Data is soft and malleable
- Apache Cassandra achieves eventual consistency
- We need BAS in planet scale databases
- Cons of Eventual Consistency:
   1. Latent Reads
   2. Communication Faults
   3. Catastrophic Failure
- Must allow fallback reads and load balance globally.
- Must build in fault tolerance in microservices (Circuit Breaker Pattern)

### Why CQRS is useful in microservices
- Data Services have a specific function
- Operate on a single data domain
- Optimized for efficiencies
- Writes are expensive
- Reads are not immediate or cheap

CQRS is a model that describes written data in a different model than reading the data.
It segregates writes from reads. It is event driven but it is not a replacement for CRUD Operations
UPDATE SERVICE-->UPDATE DATABASE    ---> 
                                          Message broker
READ SERVICE---->READ DATABASE      --->

READ DATABASE IS NOT ALWAYS CONSISTENT WITH UPDATE DATABASE

#### Data Migration
- Use asynchronous messaging
- Move data from a big database to a more focused database
- Core part of microservices migration
- We have to transform data also
- Writes are critical and need to be captured
- Reads do more than just read(may transform data)
- Syncing of data is hard but migration time also must be optimized
- Consider an original service and original database. The original service sends a write to original database. It also activates a trigger that calls a Producer Service. The Producer Service puts a message inside a message broker that a database write action came in. The Consumer service will read that message and then call the original database to get the data. The Consumer Service will talk to the New Service and transform the data.The New Service will then write data to the New Database
- A Crawler can also be used to check if the migration has completed between original database and new database.
- We have to understand why we need to synchronize data
- We have different databases
- Need to keep databases in sync
- We have different systems for e.g analytics and original database(ETL)
- Lot of overhead to keep data in sync and if it keeps in sync

SOURCE --->MESSAGE BROKER --->DESTINATION
         WATCHER(Ensure the DBs are in sync)










  



































