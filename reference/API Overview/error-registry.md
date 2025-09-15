---
title: Error Registry
deprecated: false
hidden: false
metadata:
  robots: index
---
## NotFoundProblem

<a name="not-found" />

This problem occurs when the requested resource could not be found.

Your client application tried to access a resource that does not exist (or could not be found). Please review how your users initiated such a request.

* **Type:** [https://developers.kudosity.com/errors#not-found](https://developers.kudosity.com/errors#not-found)
* **Title:** Not Found
* **Detail:** The requested resource was not found
* **Status:** 404

**Example:**

```json
{
  "detail": "The requested resource was not found",
  "status": 404,
  "title": "Not Found",
  "type": "https://developers.kudosity.com/errors#not-found"
}
```

***

## InternalServerErrorProblem

<a name="server-error" />

This problem occurs when the server encounters an unexpected condition that prevents it from fulfilling the request.

Your client application did everything correct. Unfortunately our API encountered a condition that resulted in this problem.

* **Type:** [https://developers.kudosity.com/errors#server-error](https://developers.kudosity.com/errors#server-error)
* **Title:** Server Error
* **Detail:** The server encountered an unexpected error
* **Status:** 500

**Example:**

```json
{
  "detail": "The server encountered an unexpected error",
  "status": 500,
  "title": "Server Error",
  "type": "https://developers.kudosity.com/errors#server-error"
}
```

***

## UnauthorizedProblem

<a name="unauthorized" />

HTTP 401 Unauthorized - API authentication failed.

The request was rejected because the API key is missing, invalid, expired, or lacks sufficient permissions for this resource. Check your Authorization header and API key configuration.

* **Type:** [https://developers.kudosity.com/errors#unauthorized](https://developers.kudosity.com/errors#unauthorized)
* **Title:** API Authentication Failed
* **Detail:** Missing or invalid API key in Authorization header. Verify your API key is correct and has the required permissions
* **Status:** 401

**Example:**

```json
{
  "detail": "Missing or invalid API key in Authorization header. Verify your API key is correct and has the required permissions",
  "status": 401,
  "title": "API Authentication Failed",
  "type": "https://developers.kudosity.com/errors#unauthorized"
}
```

***

## InputValidationProblem

<a name="input-validation" />

This problem occurs when the request is deemed unprocessable.

Your client issued a request that failed validation. Certain validation libraries return multi-errors and cannot be easily parsed into discreet types. This problem type, afforded the provider with the ability to surface all validation errors and negate the need for a trial and error workflow on your side.

* **Type:** [https://developers.kudosity.com/errors#input-validation](https://developers.kudosity.com/errors#input-validation)

* **Title:** Invalid Input Parameters

* **Detail:** The request contained invalid, or malformed parameters.

* **Status:** 422

* **Fields:**
  * **issues**

**Example:**

```json
{
  "detail": "The request contained invalid, or malformed parameters.",
  "issues": [
    {
      "code": 2200,
      "field": "account_id",
      "message": "The account_id supplied was invalid."
    }
  ],
  "status": 422,
  "title": "Invalid Input Parameters",
  "type": "https://developers.kudosity.com/errors#input-validation"
}
```

***

# Kudosity Error Registry

## InternalServerErrorProblem

This problem occurs when the server encounters an unexpected condition that prevents it from fulfilling the request.

Your client application did everything correct. Unfortunately our API encountered a condition that resulted in this problem.

**Type:** [https://developers.kudosity.com/errors#server-error](https://developers.kudosity.com/errors#server-error)
**Title:** Server Error
**Detail:** The server encountered an unexpected error
**Status:** 500

***

### Example

```json
{
  "detail": "The server encountered an unexpected error",
  "status": 500,
  "title": "Server Error",
  "type": "https://developers.kudosity.com/errors#server-error"
}
```

***

## InputValidationProblem

This problem occurs when the request is deemed unprocessable.

Your client issued a request that failed validation. Certain validation libraries return multi-errors and cannot be easily parsed into discreet types. This problem type affords the provider the ability to surface all validation errors and negate the need for a trial-and-error workflow on your side.

**Type:** [https://developers.kudosity.com/errors#input-validation](https://developers.kudosity.com/errors#input-validation)
**Title:** Invalid Input Parameters
**Detail:** The request contained invalid, or malformed parameters.
**Status:** 422

**Fields:**

* **issues**

***

### Example

```json
{
  "detail": "The request contained invalid, or malformed parameters.",
  "issues": [
    {
      "code": 2200,
      "field": "account_id",
      "message": "The account_id supplied was invalid."
    }
  ],
  "status": 422,
  "title": "Invalid Input Parameters",
  "type": "https://developers.kudosity.com/errors#input-validation"
}
```

***

## NotFoundProblem

This problem occurs when the requested resource could not be found.

Your client application tried to access a resource that does not exist (or could not be found). Please review how your users initiated such a request.

**Type:** [https://developers.kudosity.com/errors#not-found](https://developers.kudosity.com/errors#not-found)
**Title:** Not Found
**Detail:** The requested resource was not found
**Status:** 404

***

### Example

```json
{
  "detail": "The requested resource was not found",
  "status": 404,
  "title": "Not Found",
  "type": "https://developers.kudosity.com/errors#not-found"
}
```

# NotFoundProblem

This problem occurs when the requested resource could not be found.

Your client application tried to access a resource that does not exist (or could not be found). Please review how your users initiated such a request.

**Type:** [https://developers.kudosity.com/errors#notfoundproblem](https://developers.kudosity.com/errors#notfoundproblem)
**Title:** Not Found
**Detail:** The requested resource was not found
**Status:** 404

***

### Example

```json
{
  "detail": "The requested resource was not found",
  "status": 404,
  "title": "Not Found",
  "type": "https://developers.kudosity.com/errors#notfoundproblem"
}
```