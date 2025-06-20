
microservices tutorial available in codedecode channel:
```
what are microservices?
why microservices?
pros & cons of microservices arhitecture?
when to use microservices?
how does microservice architecture work?
what are main features of microservices?
how do microservices communicate with each other?
what is the difference between monolithic, SOA & Microservices Architecture?
```

conversion from monolithic to microservices
```
how to design microservices?

which dbs does microservices-based architecture prefer?

imp design patterns used in microservice architecture

what are bounded contexts

how to test microservices?

how to secure microservices?(auth)

how to monitor your microservices - tools available?

how to deploy microservices?
```

```
why containers a good infrastructure for microservices?

how to deploy microservices in docker?

how to deploy microservices in aws?
```




```
What are microservices?

Microservices is an architecture where the application is exposed as loosely coupled services that
can be independently developed, deployed, and maintained. Each service exposed is referred to as
Microservice. Each service performs a unique function.

Speciality of this architecture is that polyglot architecture is supported. For example, if a team is
working on one of the microservice using Java, Spring Boot, and MySQL, another team can work
on another microservice using Python, Node JS, and NoSQL.

Different microservices can use a different version of the same programming language.
Different microservices can use different programming languages.
Different microservices can use different architectures as well.
```

```
Why Microservices?

In the case of monolith applications, there are several problems like

1· Same code base for presentation, business layer, and data access layer. Application is deployed as
a single unit.
2· Complex to maintain and scalability is an issue.

Microservice solves the above problems.
Microservices are ideal when a monolith or a legacy application needs to be modernized.

1. For new software development, if the key business drivers are to reduce time to market, scalable better
software, lower costs, faster development, or cloud-native development, microservices are ideal.

2. Each service is independent and gives the flexibility to choose the programming language, database,
and/or architecture.

3. Distinct services can be developed, deployed & maintained independently
```


```
what are pros & cons of microservice architecture?


Pros of Microservice Architecture

1. Freedom to use different technologies

2. Each microservices focuses on single capability

3. Supports individual deployable units

4. Allow frequent software releases

5. Ensures security of each service

6. Multiple services are parallelly developed and deployed



Cons of Microservice Architecture

1. Management of a large number of services is difficult.

2. Communication between microservices is complex.

3. Increased efforts for configuration and other operations

4. Difficult to maintain transaction safety and data boundaries

5. Due to the decentralized nature of microservices, more
microservices will mean more resources hence high Investment

6. Debugging of problems is harder unless the right instrumentation
is followed during design and development.

7. Microservices will need a large team size with the right mix of
experience in design, development, automation, deployments,
tools, and testing.
```


```
When to use microservices?

1· Reduce time to market,
2. Scalable better software,
3. Lower costs,
4· Faster development,
5. Cloud-native development
6· It makes sense to adopt a microservices architecture, if the team size is big enough as each service will require its team to develop, deploy and manage.
7. Timeframe and skills of team members are a constraint.
8· If fast results are required,
9· choose microservices architecture only if the team also has experience in microservices.
10· Do not use this architecture for simple application which can be managed by monolithic
application .
11· So you use ask yourself first do we really need this microservice architecture to decouple the
services as it comes with a cost
```

```
monolith -> service Oriented Architecture -> microservices Architecture
```


```
Draw a microservice architecture mermaid flowchart.
Watch microservices Interview1 & 2 videos again.
```

Microservices Communication
```
Ways to communicate between Microservices

1. We have seen Synchronous communications through -
Rest APIs
GraphQI
Feign using Eureka discoveries
GRPC ( 10 times faster than REST APIs ) - developed by Google as substitute of REST
with many more features.

2· A synchronous call means that a service waits for the response after performing a request.

3· Today we will look at ways to do asynchronous communication in java. This communication usually involves some kind of messaging system like
Active Mqs
Rabbit MQs
Kafka
```


```
What is Async communication

1. In Async communication, To initiate such type of communication, a Microservice who wants to send some data to another Microservice publishes a message to a separate component known as a message broker. It is responsible for handling the message sent by the producer service and it will
guarantee message delivery.

2. After the message is received by the broker, it's now its job to pass the message to the target service. If the recipient is down at the moment, the broker might be configured to retry as long as necessary for successful delivery.

3. These messages can be persisted if required or stored only in memory. In the latter case, they will be lost when the broker is restarted and they are not yet sent to the consumer.

4· Since the broker is responsible for delivering the message, it's no longer necessary for both services to be up for successful communication. Thus async messaging mitigates the biggest problem of synchronous communication - (tight coupling).

5. A relevant point here is that there, the sender doesn't need to wait for the response. It might be sent back from the receiver later as another asynchronous message.

6. The intended service receives the message in its own time. The sending service is not locked to the broker. It simply fires and forgets.
```


```
What if the message broker is down?

1. A message broker is a vital part of the asynchronous architecture and hence must be fault tolerant

2. This can be achieved by setting up additional standby replicas that can do failover. Still, even with auxiliary replicas, failures of the messaging system might happen from time to time.

3. If it's essential to ensure the message arrives at its destination, a broker might be configured to
work in at-least-once mode. After the message reaches the consumer, it needs to send back ACK to the broker. If no acknowledgement gets to the broker, it will retry the delivery after some time.
```


```
Types of Async Communication
1. Commonly, there are two optins in message-based communication:
	1. Point to point
	2. Publisher-Subscriber
```


```
What is PTP Async communication
1. PTP - A queue will be used for this type of messaging-based communication.

2. The service that produces the message, which is called as producer (sender), will send the message to a queue in one message broker and the service that has an interest in that message, which is called a consumer (receiver), will consume the message from that queue and carry out further processes for that message

3. One message sent by a producer can be consumed by only one receiver and the message will be deleted after consumed.

4. If the receiver or an interested service is down, the message will remain persistent in that queue until the receiver is up and consumes the message.

5· For this reason, messaging-based communication is one of the best choices to make our microservices resilient.

6. A popuiar choice for the queueing system is RabbitMQ, ActiveMQ
```

```
What is Publisher-Subscriber Async communication

1. In publisher-subscriber messaging-based communication, the topic in the message broker will be used to store the message sent by the publisher and then subscribers that subscribe to that topic will consume that message

2. Unlike point to point pattern, the message will be ready to consume for all subscribers and the topic can have one or more subscribers. The message remains persistent in a topic until we delete it.

3. In messaging-based communication, the services that consume messages, either from queue or topic, must know the common message structure that is produced or published by producer or
publisher.

4· examples are Kafka, Amazon SNS etc
```


```
What is Event Based Async communication

1. Unlike messaging-based communication, in event-based communication, especially in event-driven pattern , the services that consume the message do not need to know the details of the message.

2. In event-driven pattern, the services just push the event to the topic in the message broker and then the services that subscribe to that topic will react for each occurrence event in that topic Each event in the topic will be related to a specific business logic execution.
```

Exercise: Generate a code base for each example & complete the tasks.
