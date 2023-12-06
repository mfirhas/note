# Domain-Driven Design

from book: *Implementing Domain-Driven Design* by Vaughn Vernon.


# Chapter 1

Ubiquitous Language: 
- *“It is a team pattern used to capture the concepts and terms of a specific core business domain in the software model itself”*
- *“The Ubiquitous Language is a shared language developed by the team—a team composed of both domain experts and software developers.”*
- *“more centered on how the business itself thinks and operates”*

Bounded Context: 
- Explicit boundaries around Ubiquitous Language.
- There is one Ubiquitous Language per bounded context.

Context Map: Interaction between bounded context.

When to use DDD:
- Complex business operations/usecases
- Many domains and business operations/usecases
- Even it's not complex in the beginning, it has potential to grow in complexity.
- Frequent changes
- You don't understand the domain yet, meaning there'll be a lot of researches/modifications/changes of requirements.

When to NOT use DDD:
- Pure CRUD only
- Few business operations/usecases

Goal of DDD: Able to express ubiquitous languages, bounded context and their mapping in the team composed of technical and non-technical people, not only see it from technical perspective, but also from business perspective.

## Domain Model
Part of Ubiquitous Language which resides inside bounded context.

*“It’s a software model of the very specific business domain you are working in. Often it’s implemented as an object model, where those objects have both data and behavior with literal and accurate business meaning.”*

- Rich of business rules and logics, not only data/attributes.
- Tend to be small and focused.

## Anemic Domain Model
Domain model which doesn't contain additional business rules/logics other than getters/setters.


# Chapter 2: Domains, Subdomains, and Bounded Context

## Domain
Domains contain ubiquitous languages, and business rules specific to the smallest possible context. Each domains reside inside bounded context. Domains can have subdomain, where smaller domain with its own bounded context.

Domain has subdomains called Core Domains and Supporting Domains.

Core Domains is the more specialized domain which has smaller context but central to the business rules/logics.

Supporting Domains are domain to compliment the systems widely, usually when you make adapter to certain business rules/logics.

Whole business domain contains 1 to many bounded contexts. Each Bounded Context contains 1 to many domains(subdomains). Bounded context separate related domains/subdomains. Each domains/subdomains can communicate to other domains/subdomains in same bounded context or to other bounded context(context mapping).

Bounded context is to give context to the domain, some domains might have same names, even same functionalities/operations, but they have different context. 
E.g. Account in term of banking is different than Account in term of social media.

The business rules and logics inside domains are defined by Domain Experts, they are people having expertise in the particular domain, doesn't have to be the developers, can be anyone with required knowledge in the domain.

Subdomain Types:
- **Core subdomain**: core to business rules and logics, the most rarely changed, many other domains depend on this domain.
- **Supporting subdomain**: domain extending the rules and logics, more frequent changes than core domains.
- **Generic subdomain**: not too related to the business rules and logics, but required.

Domain dan subdomain terdiri dari:
- **Entity**: 
- **Value Object**: 

Bounded context bisa terdiri dari:
- Domains/Subdomains(entity and/or value object)
- Services
- Events
- Modules
- Aggregates
- Context Maps
- Shared Kernel

Domain Model consists of 1 or more bounded contexts.

The size of bounded context should not more nor less, but exact as to what is needed.

Separate architectural(or infrastructural?) from ubiquitous languages inside the domain's bounded context, architectural/infrastructural should have their own bounded context, probably inside generic domain.

Domain modeling should not be dictated by technical terms/components, instead be dictated by business requirements/rules/logics. Ubiquitous languages should come from domain experts instead of technical experts, unless technical experts play double role as domain experts as well. We should map those business requirements into domain models utilizing terms existed in particular programming languages as programming languages have different ways of representing DDD components.


# Chapter 3: Context Maps
Relations and Integrations between bounded context.

Types of Relations between bounded contexts:
- **Partnership**: Synchronous integration, both ends must coordinate and schedule the release well in order to work well.
- **Shared Kernel**: Only share part of the codes/models. Shared items cannot be change without consultations with other teams used it.
- **Customer-Supplier Development**: Upstream-Downstream relationship where upstream needs to address downstream needs in order to succeed.
- **Conformist**: When downstream follows what upstream already defined.
- **Anticorruption Layer(ACL)**: Downstream creates additional layer to translate upstream into downstream's own model. E.g. serde? validations?
- **Open Host Service(OHS)**: Provide protocol to be accessed(e.g. http, grpc, message queue)
- **Published Language(PL)**: Common "language" to communicate, combined with Opeh Host Service. Message exchange format, e.g. JSON, XML, Protobuf, etc.
- **Separate Ways**: No connections and specialized solutions within small scope. E.g. having own oauth implementation.
- **Big Ball of Mud**: Where large domain models mixed and offer inconsistent connections. Proceed with cautious.


# Chapter 4: Architecture

## The Layers Architecture(Buschmann):
- User Interface Layer
- Application Layer
- Domain Layer
- Infrastructure Layer

## Hexagonal and Port/Adapters Pattern
- Providing port and adapter for each input and output
- Separation between client and implementations detail.

## Service-Oriented Architecture
1. Service Contract: document or message format defined as contract. E.g. JSON format, xml format, etc.
2. Service Loose Coupling: Service minimize dependency and only have an awareness of each other.
3. Service Abstraction: Publish only what defined in contracts, hide implementation details.
4. Service Reusability: Can use reused by other services.
5. Service Autonomy: Has their own resource independent of other's.
6. Service Statelessness: Service store no states, instead states are handled by clients.
7. Service Discoverability
8. Service Composability

Service-Oriented Architecture Manifesto.

There's no specific rules defining how domains and their size inside each services in SOA, it depends on business rules handled by that specific service.

## REST
- Constraints: [REST's Constraints](https://codewords.recurse.com/issues/five/what-restful-actually-means)
- Represents a resource on server side with identifier(URI).
- Stateless: each request is independent of previous or next requests. There is no session in each request, the session might be stored somewhere else.
- Fixed methods/interfaces
- REST API should represent use cases, not domain models.
- Loose coupling

## Command-Query Responsibility Segregation (TODO: need more dedicated research)
- Command: write-only interfaces, modify system states, return no values.
- Query: read-only interfaces, doesn't modify system states , return values.
- Commands can be synchronous or asynchronous.

## Event-Driven Architecture
- Services communicate in asynchronous ways through messaging sending/receiving events.

## Pipes and Filters
Event-Driven Architecture is achieved by piping messages through **Pipes/Channels**, these messages got sent or accepted by **Port**, then port will pass the messages into **filter processor/processors** and send messages into another port to be sent into another pipes/channels.

Processors are loosely coupled to each other, the orders can be re-order, and interchangable. 
Filters/processors may read/write from/to 1/multiple pipes. Filters/processors can be made parallel.

Channels or the messages queues/brokers can be used: amqp(e.g. rabbitmq), or kafka.
Each amqp and kafka have different ports, but the messages can be processed the same.

## Long-Running Processes, aka Sagas (TODO: need more dedicated research)
*“...Event-Driven, distributed, parallel processing pattern...”*
Executive and Tracker

## Event Sourcing
Storing and tracking changes of domain model.
Data can be gathered and made by combining multiple changes made into storage.

Event Sourcing typically only has 1 get query, no other complex query should happen. Combine with CQRS for querying mechanism.

Issue: how to handle keep-growing changes with millions event billions of changes made?

Solution:
- Snapshots: background process of creating in-memory state where events from event store loaded into memory, calculated/serialized and store-back into event store, then next calculation/serializing use the calculated/serialized data in event store with newer events. This method require synchroniation between app, in-memory state and event store.

Example of event sourcing: Journaling/Bookkeeping process of accounting may use event sourcing to store users debit/credit activities.

## Data Fabric and Grid-Based Distributed Computing (TODO: need more dedicated research)
To handle big data.

## Data Replication (TODO: need more dedicated research)

## Continuous Queries (TODO: need more dedicated research)

## Distributed Processing (TODO: need more dedicated research)


# Chapter 5: Entities
*“When an object is distinguished by its identity, rather than its attributes, make this primary to its definition in the model. Keep the class definition simple and focused on life cycle continuity and identity. Define a means of distinguishing each object regardless of its form or history. . . . The model must define what it means to be the same thing.”*

Entity:
- Representation of business object that has attributes and operations related to them. 
- Store data and its changes through the stages of program.
- Identified by specific identity.
  - Identity can be represented in one of its attribute.
  - Identity must be unique that really differentiate each entity.
  - E.g. RDBMS's primary keys, uuid, etc.
- Equality by its identity(Partial Equivalence).
- Each entity can have more than 1 identity(surrogate identities).
- Surrogate identity should not exposed to users, since it's not part of domain model, but part of persistence.
- Domain identity and surrogate identity should be separated
- Entity should not be anemic. Anemic model could cause by designing too focus on database tables, instead of focusing on domain model.
- Entity should be rich with operations related to the domain it handles.
- Example of entities: class/object representing tables from rdbms, object representing complex object that's easily changed/modified through program lifetime, e.g. Application wide context, and its dependencies.

Entity validation:
- design by contract: setting pre-condition, post-condition, invariant.
- Defensive programming: checking and guarding against invalid states.
- Validating whole entity after validating its attributes.

Kind of identity:
- User Generated Identity.
- Application Generated Identity(e.g. UUID)
- Persistent Generated Identity(e.g. auto-increment primary key)
- Bounded Context assigned identity(local to bounded context)


# Chapter 6: Value Objects
- Like entity, the difference is that value object is identified by the whole data it contains.
- Value object tends to be part of entity to represent some of entity's attributes/data.  E.g. Time, Address, Money.
- Equality by whole data(Full Equivalence(reflexivity)), e.g. Time is considered same if all of its component is same, like hours, minutes, seconds and timezone.
- *“...strive to model using Value Objects instead of Entities wherever possible...”*
- Immutable, you don't change only parts of it, the only thing you do is either replace whole of it, or modify it through sepecified operations that have no side effects.
- Value objects can be easily replaced and handed off, without worry of side effects or misuse.
- Value objects merupakan component penyusun entity.
- Value objects can contain 1 or more data inside, represented wholly as single value.
- Use factory to create value objects.
- Values objects have operations related to them, but should be side-effects free.
- Values objects should avoid containing entities since entities are highly changeable making value object mutable.
- Passing entities into value objects methods will break immutability and inconsistency in API as it's not sure how it affects the entity.
- You can implement value objects immutability by: 
  - create them at constructions, or 
  - injecting them into entities construction, or
  - making setters private, or 
  - you can utilize language's type system such as Java final keyword, or Rust immutability(default).
- Value objects are NOT to be confused with *primitive types*, they're different. Primitive Types stand on their own with no functionalities at all.
- While primitive types only occupy stack memory, value objects can be in stack or heap depend on the values it contains.
- There are 2 kinds of Value Objects:
  - Full Value Object (in functional programming it's called *Product Type*): acts as a whole. E.g. string, time, money, date of birth, etc.
  - Partial Value Object/Standard Types (in functional programming it's called *Sum Type* or *Tagged Union* or *Discriminated Union*): there are multiple types in 1 value object and only one of them valid at one time. E.g. Currency(USD, SGD, IDR, YEN, there's no money with dual/triple currency), User types(admin, public users, internal users, etc)
- Full Value Object may contains Partial Value Object, or the other way around, or a combinations of it.
- Don't design value objects with JavaBean Specs in mind, because it allows you using setters which violate immutability of value objects.
- Here are atleast methods/operations each value objects should have:
  - *Equality*:
    - Method that define equality of value objects.
    - In Java it's `equals()` method derived, 
    - In Rust it's trait `Eq` for standard/built-in types or `PartialEq` for other types.
  - *Hashable*: 
    - Method to hash the object to create unique code/hash that differentiate it with others, even for tiny change.
    - In Java it's `hashCode()` method derived,
    - In Rust it's trait `Hash` derived(using derive macro).
  - *Stringable*:
    - Method that convert object into string to represent it to users or for debugging.
    - In Java it's `toString()` method derived,
    - In Rust it's trait `Display` or `Debug`(for debugging only) or `ToString`.
- Example of value objects: String in some programming languages, types related with time, money, currency, etc.

*Opinion: Looking at Rust's standard types, almost all of its primitive and built-in types contain operations related to the value contained, making all of them value objects. While some languages really have primitive types where they really stand alone do nothing.*


## Chapter 7: Services
*“A Service in the domain is a stateless operation that fulfills a domain-specific task”*.

What service is NOT:
- Service in DDD is NOT service in SOA terms.
- Service in DDD is NOT doing IO into/from other(e.g. http, grpc, etc)
- The one that storing IO specifics implementation is called **Application**

What service is:
- Service in DDD contains business logics.
- In Clean Architecture by Uncle Bob, this is called *Use Case*
- Service is stateless.
- Service tidak berurusan dengan implementasi dari database, web, http, dan IO lainnya.
- Service contains more complex logics than entity and value objects.
- Service designed using ubiquitous languages.
- Service should be written **after** entities and value objects defined clearly, otherwise the entities could ended up anemic if focus on service first.
- Service contains logics leveraged from entities and value objects, e.g. logic specific to certain functionalities of particular components of an entities or value objects, or service could be aggregation logics of multiple entities or value objects.
- In OOP, services don't always have to be segregated by interface class/type.
- If you want to define interface(OOP) and implementation details, separate them. Interfaces inside domain model, and implementations inside infrastructure.

## Chapter 8: Domain Events
Something that is published and subscribed in order to achieve decoupling that provide highly scalable and peak-performing cooperating services, whether in the same local bounded-context, or foreign bounded-context(e.g. distributed system).

Facilitate Eventual Consistency.

Events can be stored before forward to subscriber. If events are not stored then 2-phase commit needed.

Evented system can be made to streaming things that used to be catched later in order to avoid synchronized batched process(querying many data at once per time).

Synchronized batched process is when you gather all data that stored previously, and do the processing all at once, this require specific time needed(e.g. midnight) and require big cost of processing time the bigger the data.

We can just publish all those data becoming events into subscriber to be handled right-away.

Evented system consisted of 3 component: **Publisher**, **Channel**, and **Subscriber**.

Events are handled asynchronously. 

If transaction needed, can be done in subscriber in application context.

Event is immutable, unlike entity, but it has identities just like entity, it has multiple identities based on event's properties and unique identity for that particular event. This means more than one event can be sent but still have uniqueness to them from the unique id they have.

Adding unique identity is not mandatory if comparison between events are not needed, but still recommended if events are sent outside local bounded-context.

Events are handled by a system called message queue. Event's source acts as published can be from any bounded context or aggregate, the events passed into channel the message queue, and forwarded into subscriber. Channel might has its own storage to store events published from publisher before forwarding into subscriber through queue/channel. This kind of system usually called message broker(channel + storage). Events sent through mq/mb commonly called messages.

Events belong to domain model, not infrastructure.

Publisher send events from aggregate, throught publisher implementation at infrastructure part 

Subscriber receive events from application, to aggregate.

Only modify 1 aggregate instance per transaction(publish-subscribe).

Autonomous and Independence: avoid synchronized api like http, rpc.

Forwarding methods:
- Polling(Pull Model): REST based approach, server send the data into event store, and client poll the data.
- Queue(Push Model): Using messaging system to queue events from publisher to subscriber. E.g. AMQP, Kafka.


## Chapter 9: Modules
Modules are group of related and cohesive domain objects to separate it from other low-coupled set of domain objects in another modules.

- Name modules using ubiquitous languages.
- Module is about how to segregates domain and infrastructure components into accessible and testable APIs.
- Avoid cyclic dependencies between peer modules(Unidirectional).
- Domain objects inside module are cohesive, while inter-modules are low-coupled.
- Module encompasses many things inside(value objects, entities, events, etc), 
- Top-level module naming depends on programming languages convention/style.
- Next level is bounded-context.
- Do **not** name modules using models name from domains, because modules only represents API segregations, not representing specific objects/types.
- The smaller modules segments, the more cohesive items inside it.
- Modules are not bounded context.
- Naming modules are important as it will impacts cohesiveness and API ergonomic and testability.


## Chapter 10: Aggregates

- Cluster of entities and value objects related to each others to form a single object called root aggregate.
- Multiple sames entities and value objects can form new unique aggregates each with identity.
- Define boundaries for invariants -> transactional -> atomic, form consistency boundaries, inside bounded context.
- 1 aggregate has its own invariant rules guarded within single transaction, independent of other aggregates.
- Bounded context only modify 1 aggregate in single transaction. There should not be more than 1 aggregate in single transactions.
- User request should be mapped 1-to-1 when dealing aggregate modification.
- Aggregates size should be small, not bloated.
- Big size aggregates will hurt invariants protections, hurting maintenance, breaking consistency between model's objects.
- Big size aggregates will also hurt performance since many basics objects require big load, instead simple ones.
- Each aggregates represented by its own entity called Root Entity, along with multiple entities and value objects.
- When you are dealing with multiple aggregates that need to be synchronized, prefer **Eventual Consistency** instead putting them all in **Transactional Consistency**.
- Aggregates can be designed with entities only, or value objects only, or combinations of both.
- Aggregate references other aggregate by ID, not by whole aggregate definition.
- Aggregates referencing each other must **not** be handled in same transactions, employ Eventual Consistency instead.
- Aggregates holding an ID to other aggregates have potential doing eventual consistency operations.
- Besides using events for navigating between aggregates, one can use repository or domain service, or application service before invoking aggregates behaviour
- Use theta joins or CQRS for front end/UI for representing whole data(not limited by reference ID only), because FE should get the whole data without having to dispatch separate requests.
- Use message-based event driven for handling multiple aggregates modifications accross boundaries.
- A aggregate published, when failed, need to be retried until consistency achieved.
- Reasons to break cross context eventual consistency:
  - If you are doing batch processing the multiple same aggregates with same types, it's okay to break the rule and process them all at once in one transaction, before delivering the event to different aggregates in another bounded context. Whether they're sent one-by-one, or in batch, it doesn't matter anymore since their state already recorded from publisher bounded context. So you don't have to process and deliver the event one-by-one repetitively for the same aggregates.
  - If tech stacks you used don't support messaging system. Some languages have no internal concurrency and asynchronous support to apply observer patterns, and subscribing methods(e.g. PHP?)
  - Global transactions: e.g. using 2-phase commits protocol handling multiple aggregates in one transactions, it's not wrong, just coming from legacy or old implementations. The problem with global transactions is performance and scalability.
  - Query performance.
- *"Transactions across distributed systems are not atomic. The various systems bring multiple Aggregates into a consistent state **eventually**."*
- *Law of Demeter*:
  - When exposing services to clients, expose through public APIs, don't expose innards of the object implementations/services.
  - An object only invoke method on:
    - itself
    - any parameters passed into
    - any objects it instantiated
    - self contained objects it can directly access(e.g. fields, dependencies injected)
- *Tell, Don't Ask*:
  - Client invoke public API in Server to make server do something, instead of server giving things and let client do the rest.
- Optimistic Concurrency: Avoid multiple clients mutate single aggregate in one transaction, local to the aggregate.
- Don't inject domain services or repository into aggregates, instead they go to application services. Aggregates should have their own operations independent from those injections, instead do the domain services and/or repository operations before doing aggregates operations.
- Values objects < Entities < Aggregates < Context


## Chapter 11: Factories
- Used to create/instantiate new object(value objects, entity, aggregates, domain services, etc)
- Used to manage invariant of objects with predefined parameters, so you don't have to create objects manually by setters, because it can breaks invariants.
- Ease the clients whenever they want to create objects, by just calling factory objects/method.
- Factories handle details of objects creations, their invariants, their pre/post conditions, client must not worry about these.
- Factory on services are used for integratiing bounded context.
- Factory can also be used for transating objects from/to domain models/contexts


## Chapter 12: Repositories
- Acts as interfaces directly to storage where you store Aggregates.
- 2 kinds of repositories:
  - Collection-oriented repository: 
    - Mimics **Set** data structure.
    - Tracks changes to data being read/write.
    - E.g: Relational Database.
  - Persistence-oriented repository:
    - Mimics **Map** data structures.
    - Doesn't track changes to data being read/write.
    - E.g: NoSQL Database
- Managing transactions should be the responsibility of **Application Layer**.
- Implement Facade pattern to handle & manage transactions before and after invoking domain layer's operations.
- Repository vs DAO:
  - Use Repository in DDD context.
  - Repository focus on domain models.
  - While DAO focus on data in the database(e.g. in term of object -> tables)
  - Domain models should not be ruled by tables in database, use repositroy to map this operations.
  - DAO create mapping 1-1 with tables in database and use said object as representation of tables in database.
  - Repository can be said as bigger scope of DAO, as repository might employ DAO operations inside. All depends on implementations.
- Testing repository: 
  - Test the operations connected to the production database in used.
  - Test the operations using in-memory implementations(mocked).