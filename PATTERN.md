# Request-derived Context

> [!WARNING]
> This pattern is still in development and may change in the future.

## Name

Request-derived Context.

## Also Known As

- Tenant Context
- User Context

## Intent

To define a consistent and structured approach to resolving contextual information from an HTTP request, such as a
tenant or user, and making it available for the duration of the requests' lifetime.
It defines a clear separation of concerns for extracting, resolving, and accessing the contextual data, creating a clean
and maintainable architecture that will function regardless of what exactly the context is.

## Context

This pattern is applicable to any application or system that meets the following criteria:

- Uses an HTTP-based request/response model.
- Processes requests independently and in isolation.
- Requires context-specific data to function correctly.
- Resolves context-specific data based on the request.

This includes, but is not limited to, web applications, HTTP APIs, RPC-based services, GraphQL services, microservices,
and serverless functions.
As long as it is receiving HTTP requests and needs context, the pattern is applicable.

This pattern is similar to the **Request Context** concern found in many frameworks, with the key difference being
that the context is derived from the request itself.
All request-derived context is request context, but not all request context is request-derived context.

## Problem

Modern web applications often rely on contextual information, and because of how the HTTP request/response model works,
there is no guarantee that the context will be relevant for more than the current request.
The context could be anything, such as the identity of the user making the request, the tenant on whose behalf the
request is made, or the region or language the request is being made in.
It could also possibly be required across multiple requests, with each request possibly manipulating the context.
This is a problem because it requires state, and HTTP is a stateless protocol.

We have long since developed methods of adding state on top of HTTP, such as cookies, or more appropriately, sessions.
However, for a web application to know the state that is relevant to the current request, it needs context, bringing us
full circle.
While an HTTP request does not carry state or contain context, it does carry information that can be used to derive it.
As well as allowing for arbitrary data to be passed along with the request (e.g. custom headers, query parameters, or
cookies), which can be used in the same way.

For the context to be useful, the relevant data needs to be extracted from the request, resolved to whatever its final
form is, and made available for the duration of the request.
Without any form of defined structure, the logic that handles this is typically scattered throughout the codebase, with
the same concept being implemented in multiple places, leading to code duplication and a lack of consistency.

While this behaviour is common across modern web frameworks, it is rarely identified or treated as a formal
architectural concern.
This means we lack terminology to describe it, making it difficult to draw parallels between different components
within an application that ostensibly requires the same functionality, whether entirely or in part.

## Force

There are several forces at play that make this pattern necessary:

- **Statelessness** <br/>
  Each HTTP request is handled in isolation; therefore, it is without shared memory or persistent context.
  This means the context must be derived from the request itself.
- **Early Context Requirements** <br/>
  Core decisions at both the application and domain level will often depend on context, which means it may be
  resolved early on, so the request can be processed correctly.
  The pattern must allow for both early and late resolution of context, whether eager or lazy.
- **Diverse Context Sources** <br/>
  Contextual information may exist in a wide variety of formats and locations (e.g. headers, paths, cookies),
  sometimes simultaneously.
  The pattern must accommodate this, allowing for multiple extraction strategies.
- **Varying Resolution Complexity** <br/>
  Just like how the context sources can vary, so too can the complexity of resolving the context.
  Some context sources may be direct and can be exchanged for a value without processing (e.g. tenant slug in the
  URL, or locale in a header).
  Others may be indirect and require additional processing (e.g. user ID in a JWT, or tenant ID in a cookie).
  The pattern must also accommodate this, allowing for different resolution strategies.
- **Separation of Concerns** <br/>
  Mixing extraction and resolution logic with the business logic of the application leads to tight coupling, poor
  testability, and reduced clarity.
  Likewise, though to a lesser extent, mixing the resolution logic and the extraction logic can lead to similar issues.

## Solution

The solution is to separate the process into four distinct components, an extractor, a resolver, a store, and a manager.
Each of these components can be composed in whatever way is appropriate, or swapped out for a different implementation,
without impacting the application.
This not only allows for multiple types of context to be handled, but also allows for different implementations of the
same type, such as supporting user authentication via a JWT in a header, or via a session cookie.

### Extractor

The extractor is responsible for extracting a **context source** from the request if it is present.
While implementations may be generic, each instance of an extractor should be specific to a context source.
Take, for example, the class `HeaderExtractor`, which extracts from request headers, but requires the name of the header
to extract from.
If the application needs the tenant ID from a `X-Tenant-ID` header, but also an authentication token from the 
`Authorization` header, then two separate instances would be required, one for each.

While extractors must remain unaware of the context type, they may process the extracted data.
In the above example, the `X-Tenant-ID` header contains a **direct context source**, which can be used as-is to 
resolve the tenant context.
The `Authorization` header, however, contains a JWT, which would be an **indirect context source**, and would need to be
transformed into a **direct context source**, like a user ID.

This is not without nuance, however. 
Whether an extractor should perform a specific type of processing is a decision that should be made on a case-by-case
basis, taking into consideration the separation of concern principle.
Reading a JWT to extract a value, or even extracting a value from a server-side session would make sense, as those are
elements that typically live within the HTTP layer of an application.
Querying a data source, such as a database or an external service, however, would be best left to the resolver.

> [!NOTE]
> Server-side sessions are not always backed by an in-memory data store and may rely on databases,
> file systems, or other external services.
> In those cases, the decision to make that part of the HTTP layer was already made, so it should not be of concern.

### Resolver

Resolvers are responsible for taking a context source and resolving the context in its final form.


## Structure

## Dynamics

## Implementation

## Examples

## Consequences

## Known Uses

## Related Patterns

## Pattern Type

Architectural Pattern
