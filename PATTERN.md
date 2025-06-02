# Request-derived Context

> [!WARNING]
> This pattern is still in development and may change in the future.

## Name

Request-derived Context.

## Also Known As

- Tenant Context
- User Context

## Intent

The **Request-derived Context** pattern defines a consistent and structured approach to resolving contextual information
from a HTTP request, such as a tenant or user, and making it available for the lifetime of the request.
It defines a clear separation of concerns for extracting, resolving, and accessing the contextual data, creating a clean
and maintainable architecture that will function regardless of what exactly the context is.

## Context

This pattern is applicable to any application or system that meets the following criteria:

- Uses a HTTP-based request/response model.
- Processes requests independently and in isolation.
- Requires context-specific data to function correctly.
- Resolves context-specific data based on the request.

This includes, but is not limited to, web applications, HTTP APIs, RPC-based services, GraphQL services, microservices,
and serverless functions.
As long as it's receiving HTTP requests and needs context, the pattern is applicable.

This pattern is similar to the **Request Context** concern found in many frameworks, with the key difference being
that the context is derived from the request itself.
All request-derived context is request context, but not all request context is request-derived context.

## Problem

Modern web applications often rely on contextual information, and because of how the HTTP request/response model works,
there's no guarantee that the context will be relevant for more than the current request.
The context could be anything, such as the identity of the user making the request, the tenant the request is on
behalf of, or the region or language the request is being made in.
It could also possibly be required across multiple requests, with each request possibly manipulating the context.
This is a problem because it requires state, and HTTP is a stateless protocol.

We've long since developed methods of adding state on top of HTTP, such as cookies, or more appropriately, sessions.
However, for a web application to know the state that is relevant to the current request, it needs context, bringing us
full circle.
While a HTTP request does not carry state or contain context, it does carry information that can be used to derive it,
as well as allowing for arbitrary data to be passed along with the request, which can be used in the same way.

For the context to be useful, the relevant data needs to be extracted from the request, resolved to whatever its final
form is, and made available for the duration of the request.
Without any form of defined structure, the logic that handles this is typically scattered throughout the codebase, with
the same concept being implemented in multiple places, leading to code duplication and a lack of consistency.

While this behaviour is common across modern web frameworks, it is rarely identified or treated as a formal 
architectural concern.
This means we lack terminology to describe it, making it difficult to draw parallels between different components
within an application that ostensibly requires the same functionality, whether entirely or in part.

## Force

## Solution

## Structure

## Dynamics

## Implementation

## Examples

## Consequences

## Known Uses

## Related Patterns

## Pattern Type

Architectural Pattern
