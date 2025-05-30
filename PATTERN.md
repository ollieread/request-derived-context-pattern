# Request-derived Context

> [!WARNING]
> This pattern is still in development and may change in the future.

## Name

Request-derived Context.

## Also Known As

- Tenant Context
- User Context

## Intent

The **Request-derived Context** pattern defines a consistent approach and formal structure for resolving contextual
information, such as a tenant or user, from the current request
and making it available for the lifetime of the said request.
While this is a common architectural concern implemented by most modern web frameworks, it lacks formalisation,
meaning we lack any consistent terminology or structure to describe it.
This pattern separates the concerns of extraction, resolution and accessibility, improving clarity and reuse.

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
