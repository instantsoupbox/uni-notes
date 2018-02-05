# Exercise 03 - Building Microservices

## Monolithic Architecture
* **Three layer/tier architecture**
    * Database
    * Client-side UI
    * Server-side application
* 3-L architecture is packed into a single monolith type architecture and deployed on the cloud.
* Advantages:
    1. Easy development
    2. Easy testing
    3. Simple deployment
    4. Easy horizontal scaling by running multiple copies behind a load balancer
* Problems:
    1. Growth in size and complexity
    2. Large and complex codebase
    3. Slow start time
    4. Expensive re-deployment of whole application due to downtime increase
    5. Scaling difficulty in case of conflicting resource requirements of different modules.
    6. Instances of the application are identical, therefore bugs will impact the availability of the entire application
    7. Immunity to adopting new technology due to affect on the entire application.

## Microservice Architecture
* Split monolithic application into small, interconnected services with its own database or storage system. 
* Benefits:
    1. Easy understanding and maintainance
    2. Independent service development
    3. Independent service deployment
    4. Barierless adoption of new technology
    5. Easy, independent scaling of each service
* Drawbacks:
    1. Additional complexity of development
    2. Additional complexity of deployment

### API-Gateway
* Challenge: Which of the microservice endpoints should the client query?
* Solution: API-Gateway
* It is the server that is the **single-entry point** into the microservice system.
* It **encapsulates** the internal system architecture, provides API tailored to each client.

### Service Discovery
* Client-Side Discovery: Client's request is made over the **Service Registry**. It knows the locations of all service instances.
* Server-Side Discovery: Client's request is made over a router/**load balancer**. It queries a service registry and then forwards the request to an available service instance.

## Compose
* A tool for defining and running multi-container Docker applications.
* It's used to create and manage microservices architecture and specifies the needed services and from where they're built.

