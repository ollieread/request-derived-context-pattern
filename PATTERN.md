# Request Scoped Context

## Also Known As

- **Request Context** – A general term used in many frameworks to refer to data that is available throughout a single
  request, though often conflated with the container used to access it.
- **Request Context Binding** – Describes the act of resolving context (e.g. tenant, user) from the request and making
  it available via binding (e.g. DI container, request attributes).
- **Scoped Context** – Refers to data that is resolved and accessible only within the current request scope; common in
  DI lifetimes or framework-managed lifecycles.
- **Tenant/User/Locale Context** – Informal shorthand for specific resolved contexts (e.g. “current tenant”) without
  naming the resolution mechanism.

## Intent

A pattern for resolving and accessing data that is specific to the context of the current request, such as a tenant,
a user, or a locale.
This pattern is a formalisation of a well-known and frequently implemented architectural concern in web applications,
where each request will have a unique context that the application requires to function.
The most common implementation of this is seen within the authentication and authorisation layers of an application,
where the user is resolved based on data from the request, such as a bearer token or session cookie.

## Context

This pattern is applicable to any application or system that uses an HTTP-based request/response model, where each
request is processed independently and in isolation.
This includes but is not limited to web applications, APIs, and microservices.
As long as the following criteria are met, this pattern is applicable, regardless of the specific technology stack:

- Requests are processed independently and in isolation.
- The application requires context-specific data to function correctly.
- The context-specific data is resolved based on the request.

## Problem

Modern web applications often rely on contextual data to function correctly, such as the current user or tenant.
Since HTTP messages are stateless, we have to pass data along in the request, which can be used to resolve the context.
For example, a request may include a cookie, which contains an encrypted or encoded session id.
Once received, the server can decode the cookie to retrieve the session, which will contain a unique identifier for the
current user.

Despite this being widely understood and implemented, this concern is rarely formalised, meaning that we lack
consistent terminology, which can lead to confusion and miscommunication.
This means that we have multiple common-place features that rely on this pattern, at a fundamental level, but we
treat them as separate concerns.

## Forces

- **Eager vs Lazy Resolution**
  Some contexts, such as the current tenant or the locale, need to be resolved early in the request lifecycle, while
  others, such as the current user, can be deferred until needed, if needed at all.
- **Multiple Potential Sources**
  A request may contain multiple sources of context, in different formats, in different places, such as any
  component of the host, headers, cookies, or even the body.
- **Direct vs Indirect Identification**
  Some context sources may directly identify their context, such as a tenant ID in a header, or a cookie containing
  a user ID. Other sources will be indirect,
  such as a session ID in a cookie, or a JWT bearer token, each of which requires processing to extract a direct
  identifier.
- **Avoidance of Global State**
  When working in environments with long-lived processes, which will handle multiple requests within their lifetime, 
  global state should be avoided.
  If you're working with a per-request lifecycle, as is common with languages like PHP, or old Python and Ruby 
  applications, this is less of a concern.
- **Request Isolation**
  The context for any given request must be isolated from others, regardless of the underlying execution model.
  This is essentially the same as the previous point, and only exists to highlight the isolation of the context.
