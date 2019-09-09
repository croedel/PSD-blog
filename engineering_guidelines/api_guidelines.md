---
breadcrumb: API Guidelines
---

# API Guidelines

## RESTful APIs with JSON payload

We prefer RESTful APIs with JSON payloads. Distributed SOAs following the REST style have a loose coupling between client and server implementations and are tolerant to certain changes.

We apply the RESTful web service principles to all kind of application (micro-) service components, independently from whether they provide functionality via the internet or intranet.

We use the [OpenAPI](https://github.com/OAI/OpenAPI-Specification/) specification as standard to define API specification files.

Our APIs should obey [Postel's Law -— a.k.a. "the Robustness Principle"](https://en.wikipedia.org/wiki/Robustness_principle):

> Be conservative in what you send, be liberal in what you accept.

## Language

Write APIs in U.S. English

## Backwards compatibility > versioning

APIs are contracts between service providers and service consumers that shall not be broken via unilateral decisions. Therefore all APIs shall be backwards compatible. Breaking changes shall be avoided as far as possible.

APIs must be evolved without breaking any consumers!

We strongly encourage using compatible API extensions and discourage versioning.

## Prefer Compatible Extensions

API designers should apply the following rules to evolve RESTful APIs for services in a backward-compatible way:

* Add only optional, never mandatory fields.
* Never change the semantic of fields (e.g. changing the semantic from customer-number to customer-id, as both are different unique customer keys)
* Input fields may have (complex) constraints being validated via server-side business logic. Never change the validation logic to be more restrictive and make sure that all constraints are clearly defined in description.
* Enum ranges can be reduced when used as input parameters, only if the server is ready to accept and handle old range values too. Enum range can be reduced when used as output parameters.
* Enum ranges cannot not be extended when used for output parameters — clients may not be prepared to handle it. However, enum ranges can be extended when used for input parameters.
* Use x-extensible-enum, if range is used for output parameters and likely to be extended with growing functionality. It defines an open list of explicit values and clients must be agnostic to new values.
* Support redirection in case an URL has to change (301 Moved Permanently).

## Prepare Clients To Not Crash On Compatible API Extensions

Service clients should apply the robustness principle:

* Be conservative with API requests and data passed as input, e.g. avoid to exploit definition deficits like passing megabytes for strings with unspecified maximum length.
* Be tolerant in processing and reading data of API responses, more specifically...

Service clients must be prepared for compatible API extensions of service providers:

* Be tolerant with unknown fields in the payload (see also Fowler’s "TolerantReader" post), i.e. ignore new fields but do not eliminate them from payload if needed for subsequent PUT requests.
* Be prepared that x-extensible-enum return parameter may deliver new values; either be agnostic or provide default behavior for unknown values.
* Be prepared to handle HTTP status codes not explicitly specified in endpoint definitions. Note also, that status codes are extensible. Default handling is how you would treat the corresponding x00 code (see RFC7231 Section 6).
* Follow the redirect when the server returns HTTP status 301 Moved Permanently.

## Naming
### Path segments

Use lowercase separate words with hyphens for Path Segments.
e.g. /shipment-orders/{shipment-order-id}

### Query parameters

Use snake_case (never camelCase) for Query Parameters.
Examples: customer_number, order_id, billing_address

### URL-friendly resource identifiers

Use URL-friendly resource identifiers in order to simplify encoding of resource IDs to URLs: 
[a-zA-Z0-9:._-]*

### Avoid verbs

Avoid verbs — think about nouns.

In RESTful APIs, a resource is identified by a URI and can be modified through simple CRUD. So, consider to model the API based on your resources using standard HTTP methods as operation indicators.

### Intuitively understandable URLs

Identify resources and sub-resources via path segments. In order to improve the consumer experience, you should aim for intuitively understandable URLs, where each sub-path is a valid reference to a resource or a set of resources.

For example, if `/subscriptions/{id}/native-payments/{id}` is a valid path of your API, then `/subscriptions/{id}/native-payments`, `/subscriptions/{id}` and `/subscriptions` must be valid as well in principle.
Data formats

Use following standard data formats:

| Data type | Format | Example |
|-----------|--------|----------|
| Country	| ISO 3166-1-alpha2 (https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) | DE, AT, CH, GB
| Language | ISO 639-1 (https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) | de, en
| Currency | ISO 4217 (https://en.wikipedia.org/wiki/ISO_4217)	| EUR, USD
| Date	| RFC 3339 (https://tools.ietf.org/html/rfc3339	date-fullyear - date-month - date-mday | e.g.:  2015-05-28
| Time | RFC 3339 (https://tools.ietf.org/html/rfc3339	full-date T full-time  | e.g. 2015-05-28T14:07:17Z

## Security
### SSL Usage

Always use SSL and make sure the caller of your service is authenticated and authorized. SSL actually means "HTTPS everywhere, not HTTP."

## HTTP Methods

Be compliant with the standardized HTTP method semantics summarized as follows:

### GET

GET requests are used to read either a single or a collection resource.

* GET requests for individual resources will usually generate a 404 if the resource does not exist
* GET requests for collection resources may return either 200 (if the collection is empty) or 404 (if the collection is missing)
* GET requests must NOT have a request body payload

### GET with Body

APIs sometimes face the problem, that they have to provide extensive structured request information with GET, that may conflicts with the size limits of clients, load-balancers, and servers. As we require APIs to be standard conform (body in GET must be ignored on server side), API designers have to check the following two options:

* GET with URL encoded query parameters: when it is possible to encode the request information in query parameters, respecting the usual size limits of clients, gateways, and servers, this should be the first choice. The request information can either be provided distributed to multiple query parameters or a single structured URL encoded string.
* POST with body content: when a GET with URL encoded query parameters is not possible, a POST with body content must be used. In this case the endpoint must be documented with the hint GET with body to transport the GET semantic of this call.

### PUT

PUT requests are used to update (in rare cases to create) entire resources - single or collection resources. The semantic is best described as "please put the enclosed representation at the resource mentioned by the URL, replacing any existing resource.".

* PUT requests are usually applied to single resources, and not to collection resources, as this would imply replacing the entire collection
* PUT requests are usually robust against non-existence of resources by implicitly creating before updating
* on successful PUT requests, the server will replace the entire resource addressed by the URL with the representation passed in the payload (subsequent reads will deliver the same payload)
* successful PUT requests will usually generate 200 or 204 (if the resource was updated - with or without actual content returned), and 201 (if the resource was created)

Important: It is best practice to prefer POST over PUT for creation of (at least top-level) resources. This leaves the resource ID under control of the service and allows to concentrate on the update semantic using PUT as follows.

### POST

POST requests are idiomatically used to create single resources on a collection resource endpoint, but other semantics on single resources endpoint are equally possible. The semantic for collection endpoints is best described as "please add the enclosed representation to the collection resource identified by the URL".

* on a successful POST request, the server will create one or multiple new resources and provide their URI/URLs in the response
* successful POST requests will usually generate 200 (if resources have been updated), 201 (if resources have been created), and 202 (if the request was accepted but has not been finished yet)

The semantic for single resource endpoints is best described as "please execute the given well specified request on the resource identified by the URL".

Generally: POST should be used for scenarios that cannot be covered by the other methods sufficiently. In such cases, make sure to document the fact that POST is used as a workaround (see GET with Body).
Note: Resource IDs with respect to POST requests are created and maintained by server and returned with response payload.

### PATCH

PATCH requests are used to update parts of single resources, i.e. where only a specific subset of resource fields should be replaced. The semantic is best described as "please change the resource identified by the URL according to my change request". The semantic of the change request is not defined in the HTTP standard and must be described in the API specification by using suitable media types.

* PATCH requests are usually applied to single resources as patching entire collection is challenging
* PATCH requests are usually not robust against non-existence of resource instances
* on successful PATCH requests, the server will update parts of the resource addressed by the URL as defined by the change request in the payload
* successful PATCH requests will usually generate 200 or 204 (if resources have been updated with or without updated content returned)

### DELETE

DELETE requests are used to delete resources. The semantic is best described as "please delete the resource identified by the URL".

* DELETE requests are usually applied to single resources, not on collection resources, as this would imply deleting the entire collection
* successful DELETE requests will usually generate 200 (if the deleted resource is returned) or 204 (if no content is returned)
* failed DELETE requests will usually generate 404 (if the resource cannot be found) or 410 (if the resource was already deleted before)

Important: After deleting a resource with DELETE, a GET request on the resource is expected to either return 404 (not found) or 410 (gone) depending on how the resource is represented after deletion. Under no circumstances the resource must be accessible after this operation on its endpoint.

### HEAD

HEAD requests are used to retrieve the header information of single resources and resource collections.

* HEAD has exactly the same semantics as GET, but returns headers only, no body.

Hint: This is particular useful to efficiently lookup whether large resources or collection resources have been updated in conjunction with the ETag-header.

## Use standard HTTP status codes

You must only use standardized HTTP status codes consistently with their intended semantics. You must not invent new HTTP status codes.

RFC standards define ~60 different HTTP status codes with specific semantics (mainly [RFC7231](https://tools.ietf.org/html/rfc7231#section-6) and [RFC-6585](https://tools.ietf.org/html/rfc6585)) — and there are upcoming new ones, e.g. [draft legally-restricted-status](https://tools.ietf.org/html/draft-tbray-http-legally-restricted-status-05). See overview on all error codes on [Wikipedia](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes) or via [https://httpstatuses.com/]()) also inculding 'unofficial codes', e.g. used by popular web servers like Nginx.

Below we list the most commonly used and best understood HTTP status codes, consistent with their semantic in the RFCs. APIs should only use these to prevent misconceptions that arise from less commonly used HTTP status codes.

| Status | Message | Meaning | Notice |
|--------|---------|---------|--------|
| 200 | OK |Call was successfull	
| 201 | Created |Call was successfull and object got created	
| 204 | No Content |Call was successfull but there is no content for requested resource |	Use this to indicate that the API endpoints does exist but the requested resource does not exist
| 207 | Multi-Status |The response body contains multiple status informations for different parts of a batch/bulk request	
| 301 |Moved permanently | Service is moved permanently	
| 302	|Found (Moved temporarily) |	Service is moved temporarily	
| 304	| Not Modified	| Response has not changed since last request	
| 400	| Bad Request |	Client made a bad request. Client side request was invalid and needs to be corrected / retried.	
| 401 | Unauthorized |	Client is not authorized to access service	
| 403	| Forbidden	| Client is not allowed to access this service	
| 404 | Not Found | Client requested a service that does not exists | Use this to indicate that the API endpoints does not exists! Do _not_ use this to indicate that the requested _resource_ does not exist, see 204
| 405	| Method Not Allowed	| the method is not supported	
| 406	| Not Acceptable |	resource can only generate content not acceptable according to the Accept headers sent in the request	
| 408	| Request timeout | the server times out waiting for the resource	
| 409	| Conflict	| request cannot be completed due to conflict, e.g. when two clients try to create the same resource or if there are concurrent, conflicting updates	
| 410	| Gone	| resource does not exist any longer, e.g. when accessing a resource that has intentionally been deleted	
| 412	| Precondition Failed |	returned for conditional requests, e.g. If-Match if the condition failed. Used for optimistic locking.	
| 423	| Locked |	Pessimistic locking, e.g. processing states	
| 428	| Precondition Required	 | server requires the request to be conditional (e.g. to make sure that the "lost update problem" is avoided).	
| 429	| Too many requests	| the client does not consider rate limiting and sent too many requests.
| 500	| Internal Server Error	 | An error has occured server-side and can not be fixed by redoing the request
| 501	| Not Implemented |	server cannot fulfill the request (usually implies future availability, e.g. new feature)	
| 503	| Service Unavailable| 	service is (temporarily) not available (e.g. if a required component or downstream service is not available) — client retry may be sensible. If possible, the service should indicate how long the client should wait by setting the 'Retry-After' header.	

## Response body

### Return JSON

All endpoints should return a valid JSON document. Exceptions are only valid for external dependencies (given data structure of external services) or to meet other external requirements.
Return JSON response object as top-level data

In a response body, you must return a JSON object (not an array) named `response` as a top level data structure to support future extensibility and compatibility with existing clients. This allows you to easily extend your response and e.g. add pagination, deprecation notices or debug infos later without breaking backwards compatibility

```
{
    "response": {
        // Your data
    }
}
```

Return JSON error object as top-level data

In order to define the functional status of the API to requesting clients and developers in case of an error all APIs must provide a JSON error array.

All possible error codes shall be documented appropriately!
{
    "errors": [
        {
            "code": "$ERROR_CODE",
            "msg": "$ERROR_MESSAGE",
            "stack": "$STACK"
        }
    ]
}

* code: Error code must be a String, e.g. "ValueNotAllowed" and should be descriptive.
* msg: (optional) Descriptive error message.
* stack: (optional) Error stack trace.
* Additional fields may be attached as long as they are documented.

### Pagination

Access to lists of data items must support pagination to protect the service against overload as well as for best client side iteration and batch processing experience. This holds true for all lists that are (potentially) larger than just a few hundred entries.

There are two well known page iteration techniques:

* Offset/Limit-based (https://developer.infoconnect.com/paging-results) pagination: numeric offset identifies the first page entry
* Cursor/Limit-based (https://dev.twitter.com/overview/api/cursoring) — aka key-based — pagination: a unique key element identifies the first page entry (see also [Facebook’s guide](https://developers.facebook.com/docs/graph-api/using-graph-api/v2.4#paging))

The technical conception of pagination should also consider user experience related issues. As mentioned in this [article](https://www.smashingmagazine.com/2016/03/pagination-infinite-scrolling-load-more-buttons/), jumping to a specific page is far less used than navigation via `next`/`prev` page links. This favours cursor-based over offset-based pagination.

### Customer facing APIs

Customer facing APIs require special attention. They will be integrated into  customer products with potentially long lasting life cycles (e.g. SmartTV apps, SetTop boxes, ...) and are focused on stability.

All our customer facing APIs shall use our central API Gateway https://api.p7s1.io/

### Deprecation

Sometimes it is necessary to phase out an API endpoint (or version), for instance, if a field is no longer supported in the result or a whole business functionality behind an endpoint has to be shut down. There are many other reasons as well. As long as these endpoints are still used by consumers these are breaking changes and not allowed. Deprecation rules have to be applied to make sure that necessary consumer changes are aligned and deprecated endpoints are not used before API changes are deployed.

#### Obtain Approval of Clients

Before shutting down an API (or version of an API) the producer must make sure, that all clients have given their consent to shut down the endpoint.

#### External Partners Must Agree on Deprecation Timespan

If the API is consumed by any external partner, the producer must define a reasonable timespan that the API will be maintained after the producer has announced deprecation.

#### Reflect Deprecation in API Definition

If a method on a path, a whole path or even a whole API endpoint (multiple paths) should be deprecated, the producers must set deprecated=true on each method / path element that will be deprecated (OpenAPI 2.0 only allows you to define deprecation on this level).

#### Monitor Usage of Deprecated APIs

Owners of APIs used in production must monitor usage of deprecated APIs until the API can be shut down in order to align deprecation and avoid uncontrolled breaking effects.

#### Not Start Using Deprecated APIs

Clients must not start using deprecated parts of an API.

