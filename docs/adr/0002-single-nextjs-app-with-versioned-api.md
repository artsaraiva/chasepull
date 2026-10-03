# One Next.js app with a versioned API, not a separate backend

The original brief preferred a separate backend/API layer so that a future native app could share it. We build a single Next.js application instead. The REST API lives under `/api/v1` as route handlers, and it is the contract both the web app and future native clients use. Web pages call the same typed query functions the route handlers wrap. Background jobs are scripts in the same codebase, run on a schedule.

This keeps one deployment, one type system and one test harness while the product is small. The native app needs a stable contract, not a separate deployment, and `/api/v1` provides that contract. We will split out a standalone service only when there is a measured reason, such as load, a runtime the platform can't host, or a team boundary.
