---
breadcrumb: Engineering Principles
---
# Motivation

Operating their services in production professionally is an essential succes factor for every company. The best functionality will not help if the systems are unstable, insecure or unavailable.

When you follow a traditional waterfall model with quaterly releases, long end-to-end QA phases with dedicated QA teams, separate operations teams, etc. this might not be a primary goal for the development teams. But following a DevOps / NoOps model with agile methods, continous delivery and canary testing, it gets absolutely essential.

Having this in mind, this document describes a set of technical health metrics we consider as being essential. Every team should implement and monitor them regularly. Never the less, having more metrics can be beneficial - this shall only create a baseline we expect all teams to fullfill.

# Metrics

## Synthetic monitoring / API healthcheck

All API's shall provide a healthcheck URL.
The healthcheck shall check the core service functionality. This covers e.g.

* Connection to the service's database
* Connection to external services
* Core service functionality / business logic

The healthcheck should perform a typical, representive request to the service. The healthcheck shall return http status code "200 OK" if everything proofs OK. 
It should return a status code and additional information (e.g. timing infos, details, ...) as JSON data

The healthcheck URL shall be integrated into icinga2, which calls it periodically.
icinga2 shall be configured to derive following metrics

* monthly service availability (%)
* service latency graph + average (or median) monthly latency

For both metrics, there shall be set up an alerting. This means, icinga2 shall send an alert to Opsgenie in following cases:

* The healthcheck fails >=2 times subsequentially
* The latency exceeds a configured theshold

## Application performance monitoring (APM)

All services shall collect performance data on how it performs in production. This can be done by using dynatrace and/or AWS CloudWatch.

Following metrics should be monitored at least:
	
* Incoming no. of requests per minute
* Average (or median) response time / latency over time
* No. of errors over time, split up by error code
* No. of running instances over time (especially if the service supports auto scaling)

For those metrics, appropriate alertings shall be set up. You could e.g. start with alerts on:

* Incoming requests == 0 or > some limit
* Average response time > some limit
* No. of errors > some limit
* No. of running instances == 0 or close to maximum

## Test coverage

You should measure the test coverage of all your services, which are actively worked on. So no need to do this for legacy components.

It should be measured at least monthly. Ideally you integrate it into you CI pipeline as e.g. described in [https://docs.gitlab.com/ee/user/project/pipelines/settings.html#test-coverage-parsing]()

## Security and vulnerability

All teams should consider security and minimizing vulnerability as a core targets when they develop software. Since we basically provide web based software components, we consider OWASP Top 10 as most relevant for us.

The OWASP Top 10 is a powerful awareness document for web application security.
It represents a broad consensus about the most critical security risks to web applications

Team should strive for automated vulnerability testing their applications using tools like e.g. npm-audit or w3af. 
Target is to have no CRITICAL oder HIGH issues.
You might also want to have a look on this paper on the CI integration: https://about.gitlab.com/blog/2019/06/20/announcing-gitlab-devsecops/

## Alerting

Each team shall be set up as a group in Opsgenie.
Alerts from synthetic monitoring or application performance monitoring shall be send to this Opsgenie group.

The teams shall define a reasonable plan on how to react on alerts which arrive in Opsgenie. Each team needs to make sure that their processes fulfill the riquired SLA's. How this might look like, highly depends on the availability class of the service and the agreed service level. (e.g. on-call duty, supporter of the day, ...)

## Releases announcement

All service releases shall be announced to the slack channel XXXXX. Ideally this is integrated into the CI/CD pipeline.
The message shall contain:

* Version number
* Gitlab tag
* ....
