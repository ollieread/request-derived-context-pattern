# Request-derived Context Pattern

> [!WARNING]
> This pattern is still in development and may change in the future.

This is a formalisation of a well-known architectural concern in web applications, where context is scoped to the 
current request but also derived from it.

---

## Overview

The **Request-derived Context** pattern defines a consistent approach to extracting, resolving, and providing access to
contextual information that is derived from the current request.
Things like **user context** or **tenant context** are common examples of this pattern in action.

This repository documents the pattern and its component concepts, including real-world examples and implementations, 
and sub-patterns.

---

## The Pattern

See [`PATTERN.md`](PATTERN.md) for the full breakdown of the pattern.

---

## License

This pattern and all content in this repository are licensed under the [Creative Commons Attribution 4.0 International License](https://creativecommons.org/licenses/by/4.0/).

---

## Contributing / Feedback

If you’ve used or implemented this pattern, feel free to:

- Share an example
- Suggest improvements to the documentation
- Open an issue to discuss the nuances of your use case
