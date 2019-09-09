---
breadcrumb: Engineering Principles
---

# Engineering Principles

## Philosophy

As software engineers we strive to build services and applications that are:

* resilient
* extensible
* maintainable
* built-in quality
* scalable
* efficient

Doing so, we never forget why we do that:

> Development shall enable our business and is not an end in itself
> We strive to deliver customer value

These properties lead to architectural principles that guide the choices we have to make.

## Core Principles

When we plan and engineer software, we let us guide by following engineering principles:

### We care about the Customer and the Product

We understand Customer needs and are proud of our products. We work closely with business in shipping the best products. We are not simple order takers.

### World class engineering capability

We strive to hire the best talent for the job, adopt the best technologies suited for our work. We are continuously looking to improve our ways of working.

### Build the right thing

We aim to deliver value in small increments and ship early and often. We let the costumer speak and run experiments in order to measure impact and fail fast. Not the HIPPO (Highest Paid Person) but Data is King in decision making.

### We aim for speed that can be sustained

We win by being fast and efficient at what we do. We know that long-term velocity is what counts and we continously pay-off short term tradeoffs.

### We choose simple over smart

„keep it simple, stupid“. We should strive for simplicity and avoid unnecessary complexity as most systems work best if they are kept simple. Readable code is better than a one-liner.

### Smart usage of resources

The best engineering happens under constraints and hence good engineering is being diligent on how we spend our time and money. Waste is not cool!

## Architecture Principles

### Stateless

When possible, be stateless. If you can’t, persist state outside the address space of the application, for example in a database.

### Immutable

Strive for immutability whenever possible. An object is immutable if its state cannot be modified. Immutable things are automatically thread-safe, without requiring synchronization. Overall, immutability tends to result in fewer bugs and makes it easier to prove a program correct.

### Idempotent

Whenever possible and reasonable, make service endpoints [idempotent](https://en.wikipedia.org/wiki/Idempotence#Computer_science_meaning), so that an operation produces the same result even when it’s executed multiple times. This allows clients to safely retry operations in case of timeouts due to service processing or network failures.

### Loosely-coupled systems

We prefer loosely coupled services. They are more resilient when it comes to remote dependency failures. We aim to develop autonomous isolated (micro-)services that can be independently deployed and that are centered around defined business capabilities.

### Asynchronous communication

Synchronous calls to remote systems can lead to threads in waiting state until the call times out. This can completely paralyze a system, as more and more threads move into that state until the system can no longer react to new requests. Further synchronous calls are blocking and prevent the thread from doing anything else.

We reduce the impact of remote failures by communicating asynchronously via events, where possible. Microservices publish streams of events to an event broker or queue, which interested microservices consume asynchronously. Communication with other systems becomes non-blocking: even when some functionality is affected temporarily, the system continues working.

### Service degradation

In synchronous calls, we react to remote dependency failures by degrading a service until the remote dependency is working again. (Degradation's concrete behaviour depends mainly on the service. For some service ths might e.g. be implemented as returning a default value.) This means that your system must be aware of remote dependency health. It needs to detect failure and also notice when an external dependency becomes healthy again.

In asynchronous processing scenarios, you usually have more flexibility in processing service delivery time. Hence, you can live with temporary remote failures and delay processing with retries. However, service degradation is also an option here, if otherwise processing delays due to remote failures are not acceptable.

### Microservices

Build services based on domain model and around business entities with state and behavior - for example “orders”, “payments” or “prices”. In REST terms, these are “resources”.

A service should be big enough to offer a valid business capability, but small enough to be handled by a team that can be fed by two pizzas (Amazon’s Two-Pizza Team rule) - from two to 12 people. In practice, a Two-Pizza Team may be able to own and run a large number of small services, or a smaller number of larger services.

All things considered, we prefer smaller services written in expressive programming languages with minimal code whenever possible.

Please consult the Architecture team when you start planning to build a new service.

## APIs

We prefer RESTful APIs with JSON payloads.

More info on that topic can be found on [API Guidelines](api_guidelines)

### Autonomy

A service should be:

* as autonomous as possible
* start up and be resilient when its dependencies are not available
* handle realistic reasonable exceptions and provide proper defaults if possible (container, self-healing, circuit breaker)
* run in its own process and be independently deployable.
* not share its data storage or code repository with any other service, so that changes do not affect other systems.
* should not share libraries with other services, unless those libraries are open-source. Shared internal dependencies lead to a large-scale complexity over time.
* should not provide a client library. The core API and its data model are expressed as REST and JSON.

### SaaS

Build your services so that it’s possible to offer them as a SaaS solution to third parties. In fact, consider any other system a third party with regards to API structure, resilience and service level. This is easier to do than it was a few years ago: AWS pushes us this way, the Internet model scales, and our security model is geared toward allowing our services to be on the open Internet.

We want to offers services in ways we never imagined or expected. This is part of being a platform.

### KISS / YAGNI

Keep it simple and stupid! Just build what is required and avoid overengineering. Development shall enable our business and is not an end in itself! This is also known as "You aren't gonna need it" (YAGNI).

### Data driven decisions

We log appropriate usage data for each service in order to enable and promote data driven decisions.

### Security

* Never transmit personal or confidential data unencrypted.
* New resources should be access-controlled by default (i.e. never create public S3 buckets).
* Never store secrets in source code (use AWS KMS/SSM instead).
* Do not use IP-address-based policies for those sites, prefer true authentication.
* Assume no shell access to production Docker containers.
* Use PIA if you need to secure any administrative dashboards or other non-production URLs.

### Caching awareness

To ensure efficient and responsive delivery of services, always consider the caching characteristics of the resources you expose. Where possible and appropriate use one or more of HTTP and application-level caching techniques:

* Provide "Cache-Control" HTTP headers for all routes. Use very high TTLs for static assets and meaningful TTLs for dynamic content.
* Include static assets using version attributes that are part of the cache key (example: "/assets/style.v2589641.css").
* Use conservative CDN settings.
* Avoid manual invalidation of cache items, prefer asset versioning.
