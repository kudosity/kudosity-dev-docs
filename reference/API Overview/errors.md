---
title: Error Registry
deprecated: false
hidden: false
metadata:
  robots: index
---
When a request fails, the API returns a [Problem Details](https://www.rfc-editor.org/rfc/rfc9457) object in the `error` field of the response body. The object's `type` links to the entry for that error on this page.

Every Problem Details object has these fields:

* `type`: a link to the error's entry on this page.
* `title`: a short summary of the error type.
* `detail`: an explanation of this occurrence of the error.
* `status`: the HTTP status code.

Some error types add fields, such as `issues` for input validation.

| Status | Error | Anchor |
| --- | --- | --- |
| 400 | Input validation | `#input-validation` |
| 401 | Unauthorized | `#unauthorized` |
| 402 | Payment required | `#payment-required` |
| 403 | Forbidden | `#forbidden` |
| 404 | Not found | `#not-found` |
| 409 | Conflict | `#conflict` |
| 413 | Request entity too large | `#request-entity-too-large` |
| 422 | Unprocessable entity | `#unprocessable-entity` |
| 422 | Idempotency key reused | `#idempotency-key-reused` |
| 429 | Too many requests | `#too-many-requests` |
| 500 | Server error | `#server-error` |
| 501 | Not implemented | `#not-implemented` |
| 503 | Service unavailable | `#service-unavailable` |

***

## Input validation (400)

<a name="input-validation" />

The request failed validation. The `issues` array lists every problem with the request, so you can fix them all before you retry. Each issue gives the `name` of the parameter or field and a `message`. Some endpoints also return a `code`.

* **Type:** `https://developers.kudosity.com/reference/errors#input-validation`
* **Title:** Invalid Request
* **Status:** 400

**Example:**

```json
{
  "type": "https://developers.kudosity.com/reference/errors#input-validation",
  "title": "Invalid Request",
  "status": 400,
  "detail": "Request validation failed",
  "issues": [
    {
      "name": "status",
      "message": "status must be one of: PENDING, APPROVED, REJECTED, PAUSED, DISABLED, FLAGGED, LOCKED, ARCHIVED, IN_APPEAL, LIMIT_EXCEEDED"
    }
  ]
}
```

***

## Unauthorized (401)

<a name="unauthorized" />

The API key is missing, invalid or expired, or it doesn't have permission for this resource. Check the `Authorization` header and your API key.

* **Type:** `https://developers.kudosity.com/reference/errors#unauthorized`
* **Title:** API Authentication Failed
* **Status:** 401

**Example:**

```json
{
  "type": "https://developers.kudosity.com/reference/errors#unauthorized",
  "title": "API Authentication Failed",
  "status": 401,
  "detail": "Missing or invalid API key in Authorization header. Verify your API key is correct and has the required permissions"
}
```

***

## Payment required (402)

<a name="payment-required" />

Your account balance isn't enough to complete the request. Top up your balance, then retry.

* **Type:** `https://developers.kudosity.com/reference/errors#payment-required`
* **Title:** Payment Required
* **Status:** 402

**Example:**

```json
{
  "type": "https://developers.kudosity.com/reference/errors#payment-required",
  "title": "Payment Required",
  "status": 402,
  "detail": "Your account balance is insufficient to complete this request"
}
```

***

## Forbidden (403)

<a name="forbidden" />

Your request is authenticated, but your account doesn't have permission to access this resource.

* **Type:** `https://developers.kudosity.com/reference/errors#forbidden`
* **Title:** Forbidden
* **Status:** 403

**Example:**

```json
{
  "type": "https://developers.kudosity.com/reference/errors#forbidden",
  "title": "Forbidden",
  "status": 403,
  "detail": "Access to this resource is forbidden"
}
```

***

## Not found (404)

<a name="not-found" />

The resource doesn't exist, or your account can't access it. Check the ID in the request path.

* **Type:** `https://developers.kudosity.com/reference/errors#not-found`
* **Title:** Not Found
* **Status:** 404

**Example:**

```json
{
  "type": "https://developers.kudosity.com/reference/errors#not-found",
  "title": "Not Found",
  "status": 404,
  "detail": "The requested resource was not found"
}
```

***

## Conflict (409)

<a name="conflict" />

The request conflicts with the current state of the resource, or with an operation already in progress. Check the resource's current state before you retry.

* **Type:** `https://developers.kudosity.com/reference/errors#conflict`
* **Title:** Conflict
* **Status:** 409

**Example:**

```json
{
  "type": "https://developers.kudosity.com/reference/errors#conflict",
  "title": "Conflict",
  "status": 409,
  "detail": "The request conflicts with the current state of the resource"
}
```

***

## Request entity too large (413)

<a name="request-entity-too-large" />

The request body is larger than the maximum size the endpoint accepts.

* **Type:** `https://developers.kudosity.com/reference/errors#request-entity-too-large`
* **Title:** Request Entity Too Large
* **Status:** 413

**Example:**

```json
{
  "type": "https://developers.kudosity.com/reference/errors#request-entity-too-large",
  "title": "Request Entity Too Large",
  "status": 413,
  "detail": "The request payload exceeds the maximum allowed size"
}
```

***

## Unprocessable entity (422)

<a name="unprocessable-entity" />

The request is well formed, but the operation isn't available for this resource. For example, you can't renew a personal mobile number, because it has no lease. Don't retry the request.

* **Type:** `https://developers.kudosity.com/reference/errors#unprocessable-entity`
* **Title:** Unprocessable Entity
* **Status:** 422

***

## Idempotency key reused (422)

<a name="idempotency-key-reused" />

The `Idempotency-Key` was already used for a different request. Resend the original request with that key, or send this request with a new key.

* **Type:** `https://developers.kudosity.com/reference/errors#idempotency-key-reused`
* **Title:** Unprocessable Entity
* **Status:** 422

**Example:**

```json
{
  "type": "https://developers.kudosity.com/reference/errors#idempotency-key-reused",
  "title": "Unprocessable Entity",
  "status": 422,
  "detail": "This Idempotency-Key was used for a different request"
}
```

***

## Too many requests (429)

<a name="too-many-requests" />

You sent too many requests in a short time. Wait, then retry. For limits, see [Rate limiting](https://developers.kudosity.com/reference/rate-limiting).

* **Type:** `https://developers.kudosity.com/reference/errors#too-many-requests`
* **Title:** Too Many Requests
* **Status:** 429

**Example:**

```json
{
  "type": "https://developers.kudosity.com/reference/errors#too-many-requests",
  "title": "Too Many Requests",
  "status": 429,
  "detail": "Rate limit exceeded"
}
```

***

## Server error (500)

<a name="server-error" />

The API encountered an unexpected error. This error isn't caused by your request. Retry later, and contact support if the error continues.

* **Type:** `https://developers.kudosity.com/reference/errors#server-error`
* **Title:** Internal Server Error
* **Status:** 500

**Example:**

```json
{
  "type": "https://developers.kudosity.com/reference/errors#server-error",
  "title": "Internal Server Error",
  "status": 500,
  "detail": "The server encountered an unexpected error"
}
```

***

## Not implemented (501)

<a name="not-implemented" />

The API understood the request, but this operation isn't supported for this resource. Retrying won't help.

* **Type:** `https://developers.kudosity.com/reference/errors#not-implemented`
* **Title:** Not Implemented
* **Status:** 501

**Example:**

```json
{
  "type": "https://developers.kudosity.com/reference/errors#not-implemented",
  "title": "Not Implemented",
  "status": 501,
  "detail": "This operation is not supported for this resource"
}
```

***

## Service unavailable (503)

<a name="service-unavailable" />

A service the API depends on is temporarily unavailable. You can safely retry a `GET` request later. Before you retry a `POST`, `PATCH` or `DELETE` request, check the resource's current state, because the original request may already have been applied.

* **Type:** `https://developers.kudosity.com/reference/errors#service-unavailable`
* **Title:** Service Unavailable
* **Status:** 503

**Example:**

```json
{
  "type": "https://developers.kudosity.com/reference/errors#service-unavailable",
  "title": "Service Unavailable",
  "status": 503,
  "detail": "A service this request depends on is temporarily unavailable"
}
```
