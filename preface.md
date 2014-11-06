# The BetterNow public API

This is the home for the schema and documentation of the BetterNow
public API.

Please feel free to open a new issue if you have questions about
the documentation or the API in general.

The API is evolving rapidly, and we need your help. Let us know what you need
from our API by opening an issue on this repository, and have a look at our
[guidelines for contributing](CONTRIBUTING.md) if you want to help building out
the schema.

The design of our API is heavily informed by [Heroku's HTTP API Design
Guide](https://github.com/interagent/http-api-design) and their [Platform
API](https://devcenter.heroku.com/articles/platform-api-reference)

<!-- toc -->

## Authentication

Contact [support@betternow.org](mailto:support@betternow.org) to receive an API
key.

The API key must be Base64 encoded and passed as a `Bearer` token in the
`Authorization` header. E.g., if my API key was
`938a145dd2674b83f05de469a4efcdc52828960f`, the authorization header would be
`Authorization: Bearer OTM4YTE0NWRkMjY3NGI4M2YwNWRlNDY5YTRlZmNkYzUyODI4OTYwZg==`

## Caching

All responses include an `ETag` (or Entity Tag) header, identifying the specific
version of a returned resource. You may use this value to check for changes to a
resource by repeating the request and passing the `ETag` value in the
`If-None-Match` header. If the resource has not changed, a `304 Not Modified` status
will be returned with an empty body. If the resource has changed, the request
will proceed normally.

## Clients

Clients must address requests to `api.betternow.org` using HTTPS and specify the
`Accept: application/vnd.betternow+json; version=1` Accept header. Non-browser
clients should specify a User-Agent header to facilitate tracking and debugging.

## CORS

BetterNow will configure cross-origin resource sharing (CORS) for your domain
upon request to [support@betternow.org](mailto:support@betternow.org).

## Rate Limits

We continually monitor the health of our API and reserve the right to enforce
rate limits. Rate-limited requests will receive a response with a `429 Too Many
Requests` status.

## Pagination via Ranges

List requests will return a `Content-Range` header indicating the range of values
returned. Large lists may require additional requests to retrieve. If a list
response has been truncated you will receive a `206 Partial Content` status and
the `Next-Range` will be set header set. To retrieve the next range, repeat the request with
the `Range` header set to the value of the previous request’s `Next-Range` header.

## JSON Schema

The machine-readable version of this README is [schema.json](schema.json). You
can use tools like [committee](https://github.com/interagent/committee) with the
schema to test and stub a local version of the api when you're developing your
client. I'm sure there are other tools for other languages - feel free to submit
a PR to add links for them.

## "Summary" vs. "Full" representations

When resources are embedded in other resources (e.g. a list of projects embedded
in an organisation), they will often be represented as "summarized" versions of
the embedded resource. Summary representations will always include an `url`
element that can be dereferenced to provide the full representation.

