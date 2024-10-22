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



















