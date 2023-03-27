# EDA Notes

> Events: something that happened, doesn't expect a response
>
> - Immutable, or can't be changed
> - Can be communicated or implemented through event notification
>
> Commands: request, or order, usually expects response
>
> - Can be communicated or implemented through message
>
> Contracts/Domain Library to be used by the functions
>
> - It contains all Types and DTOs shared by functions, events and commands
>
> Logs and Observability
>
> - Events are non-linear, and can be hard to track
> - Track errors with context information of the current function call

Event Driven Architecture (EDA) consists of event producers that generate a stream of events and event consumers that listen for the events. Events are delivered in near real-time, so consumers can respond immediately to events as they occur. Producers are decoupled from consumers — a producer doesn’t know which consumers are listening. Consumers are also decoupled from each other, and every consumer sees all of the events. This differs from a Competing Consumers pattern, where consumers pull messages from a queue and a message is processed just once (assuming no errors). In some systems, such as IoT, events must be ingested at very high volumes.

It has three main components:

1. Event Producer
2. Events Broker/Ingestion
3. Events Consumers

A producer creates events that are redirected by a broker into the right consumer. Consumers will react to this event and execute things they need to execute.

<p align="center" width="100%">
    <img width="50%" alt="EDA Components" src="https://yuml.me/a0017654.jpg"> 
</p>

An event-driven architecture can use a publish/subscribe (also called pub/sub) model or an event stream model.

- **Pub/sub**: The messaging infrastructure keeps track of subscriptions. When an event is published, it sends the event to each subscriber. After an event is received, it cannot be replayed, and new subscribers do not see the event.

- **Event streaming**: Events are written to a log. Events are strictly ordered (within a partition) and durable. Clients don’t subscribe to the stream; instead, a client can read from any part of the stream. The client is responsible for advancing its position in the stream. That means a client can join at any time and can replay events.

On the consumer side, there are some common variations:

- **Simple event processing**. An event immediately triggers an action in the consumer. For example, you could use Azure Functions with a Service Bus trigger so that a function executes whenever a message is published to a Service Bus topic.

- **Complex event processing**. A consumer processes a series of events, looking for patterns in the event data using a technology such as Azure Stream Analytics or Apache Storm. For example, you could aggregate readings from an embedded device over a time window and generate a notification if the moving average crosses a certain threshold.

- **Event stream processing**. Use a data streaming platform such as Azure IoT Hub or Apache Kafka as a pipeline to ingest events and feed them to stream processors. The stream processors act to process or transform the stream. There may be multiple stream processors for different subsystems of the application.

The source of the events may be external to the system. In that case, the system must be able to ingest the data at the volume and throughput that is required by the data source.

When to use this architecture:

- Multiple subsystems must process the same events.
- Real-time processing with minimum time lag.
- Complex event processing, such as pattern matching or aggregation over time windows.
- High volume and high velocity of data.

Benefits:

- Producers and consumers are decoupled.
- No point-to-point integrations; it’s easy to add new consumers to the system.
- Consumers can respond to events immediately as they arrive.
- Highly scalable and distributed.
- Subsystems have independent views of the event stream.

Challenges:

- Guaranteed delivery: In some systems, especially in IoT scenarios, it’s crucial to guarantee that events are delivered.
- Processing events in order or exactly once: Each consumer type typically runs in multiple instances for resiliency and scalability. This can create a challenge if the events must be processed in order (within a consumer type), or if the processing logic is not idempotent.

Additional considerations: The amount of data to include in an event can be a significant consideration that affects both performance and cost. Putting all relevant information needed for processing in the event itself can simplify processing code and save additional lookups. Putting minimal information in an event like just a couple of identifiers will reduce transport time and cost but requires processing code to look up any additional information it needs.

For more information related to this with Azure Cloud Services:

- [Azure Architecture Center](https://learn.microsoft.com/en-us/azure/architecture/browse/) provides architecture diagrams and technology descriptions for reference architectures, real-world examples of cloud architectures, and solution ideas for common workloads on Azure.
- [Azure Cloud Services (classic) documentation](https://learn.microsoft.com/en-us/azure/cloud-services/) provides information on how to use Cloud Services to host and run highly available, scalable cloud applications and APIs. It shows how to manage virtual machine hosts and configure, patch, and install software.
- [Customer and Partner Success Stories](https://azure.microsoft.com/en-us/resources/customer-stories/) provides success stories of customers and partners who have used Azure Cloud Services to solve their business challenges.

## References:

- [What is Event Driven Architecture? (EDA - part 1)](https://www.youtube.com/watch?v=DQ5Cbt8DQbM)
- [Event-driven architecture style](https://learn.microsoft.com/en-us/azure/architecture/guide/architecture-styles/event-driven)

## Resources:

- [Bing Chat](https://www.bing.com/new)
- [Azure Architectures](https://learn.microsoft.com/en-us/azure/architecture/browse/)
- [Presentation: The many meanings of Event Driven Architecture](https://www.youtube.com/watch?v=STKCRSUsyP0)
- [Best Practices (Heroku via Dev.to)](https://dev.to/heroku/best-practices-for-event-driven-microservice-architecture-2lh7)
- [Free whitepaper on EDA from Axon](https://lp.axoniq.io/whitepaper-ddd-cqrs-event-sourcing)
- [Software Architecture Patterns](https://www.oreilly.com/library/view/software-architecture-patterns/9781491971437/)
- [Risk Driven Architecture](https://www.amazon.es/Just-Enough-Software-Architecture-Risk-Driven-ebook/dp/B0BZ582ZL8)
