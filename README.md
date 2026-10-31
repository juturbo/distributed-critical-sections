# Distributed Critical Sections with a Message-Oriented Middleware

A centralized mutual exclusion system for distributed environments using a Message-Oriented Middleware (RabbitMQ). Processes acquire and release locks without explicit knowledge of one another.

## Architecture and Features

* **Centralized Coordinator:** A server manages lock states. Clients interact solely through the MOM via a shared public queue for requests and temporary private queues for grants.
* **Concurrency and Fairness:** The server processes requests sequentially using a single-thread executor, preventing race conditions and ensuring FIFO fairness.
* **Hierarchical Locks:** Resources use path-based targets. Conflicts are evaluated on identical, ancestor, or descendant paths.
* **Fault Tolerance:** Locks have a 15-second lease duration. An eviction timer clears expired locks to prevent deadlocks if a client crashes.
* **Synchronous Semantics:** Clients bridge asynchronous messaging with blocking lock acquisition using `CompletableFuture` and concurrent maps.

## Execution Instructions

* **Start RabbitMQ:**
   Launch the message broker using Docker:
   ```bash
   docker run -it --rm --name rabbitmq \
     -p 5672:5672 -p 15672:15672 rabbitmq:4-management

* **Start the Server:**
Execute the RabbitMQLockServer application to initialize the centralized lock manager and listen for incoming requests.

* **Run the Client:**
Instantiate the DistributedLockManager interface in your distributed processes to request and release locks on target resources.