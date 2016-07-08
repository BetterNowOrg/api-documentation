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

<!-- START doctoc -->
<!-- END doctoc -->

## Authentication

Contact [support@betternow.org](mailto:support@betternow.org) to receive an API
key.

The API key must be Base64 encoded and passed as a `Bearer` token in the
`Authorization` header. E.g., if my API key was
`938a145dd2674b83f05de469a4efcdc52828960f`, the authorization header would be
`Authorization: Bearer OTM4YTE0NWRkMjY3NGI4M2YwNWRlNDY5YTRlZmNkYzUyODI4OTYwZg==`

API keys will be scoped to specific top-level resources. Authenticated requests
for other resources will return a `403 Forbidden` response.

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

The API fully supports cross-origin resource sharing (CORS) to enable
browser-based clients.

## Rate Limits

We continually monitor the health of our API and reserve the right to enforce
rate limits. Rate-limited requests will receive a response with a `429 Too Many
Requests` status.

## Pagination via Ranges

List requests will return a `Content-Range` header indicating the range of values
returned. Large lists may require additional requests to retrieve. If a list
response has been truncated you will receive a `206 Partial Content` status and
the `Next-Range` header will be set.

To retrieve the next range, repeat the request with the `Range` header set to
the value of the previous request’s `Next-Range` header and the `Range-Unit:
items` header, e.g:

### Initial request to paginated resource

```
curl -n -sS -i -H 'Accept: application/vnd.betternow+json; version=1' \
  https://api.betternow.org/fundraisers

HTTP/1.1 206 Partial Content
#... ommitted headers
Accept-Ranges: items
Content-Range: 0-49/7167
Link: <https://api.betternow.org/fundraisers>; rel="next"; items="50-7216", <https://api.betternow.org/fundraisers>; rel="last"; items="7150-14316"
Next-Range: 50-7216
Range-Unit: items
Status: 206 Partial Content
```

### Subsequent request to paginated resource
```
curl -n -sS -i -H 'Accept: application/vnd.betternow+json; version=1' \
  -H 'Range-Unit: items' \
  -H 'Range: 50-7216' \
  https://api.betternow.org/fundraisers
```

The `rel=next` relation in the `Link` header may also be used.

If the list is empty, a `204 No Content` status with the correct range headers
and an empty request body will be returned.

## JSON Schema

The machine-readable version of this README is [schema.json](schema.json). You
can use tools like [committee](https://github.com/interagent/committee) with the
schema to test and stub a local version of the api when you're developing your
client. I'm sure there are other tools for other languages - feel free to submit
a PR to add links for them.

## "Sub-resources"

When resources are related to other resources (e.g. a list of projects embedded
in an organisation), they will be represented as a reference to an url that can
be dereferenced to return a list of the embedded resources. Clients should
prefer following the url included in the parent resource rather then
constructing their own urls.

## Example Usage
```
cat > ~/.netrc
machine api.betternow.org
  login your_name
  password <YOUR API KEY>


curl -n -sS -i -H 'Accept: application/vnd.betternow+json; version=1' \
  https://api.betternow.org/fundraisers/alot
```

```html
<!doctype html>
<html>
  <head>
    <meta charset="utf-8">
    <title>CORS test page</title>
    <script>
      var Base64={_keyStr:"ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/=",encode:function(e){var t="";var n,r,i,s,o,u,a;var f=0;e=Base64._utf8_encode(e);while(f<e.length){n=e.charCodeAt(f++);r=e.charCodeAt(f++);i=e.charCodeAt(f++);s=n>>2;o=(n&3)<<4|r>>4;u=(r&15)<<2|i>>6;a=i&63;if(isNaN(r)){u=a=64}else if(isNaN(i)){a=64}t=t+this._keyStr.charAt(s)+this._keyStr.charAt(o)+this._keyStr.charAt(u)+this._keyStr.charAt(a)}return t},decode:function(e){var t="";var n,r,i;var s,o,u,a;var f=0;e=e.replace(/[^A-Za-z0-9\+\/\=]/g,"");while(f<e.length){s=this._keyStr.indexOf(e.charAt(f++));o=this._keyStr.indexOf(e.charAt(f++));u=this._keyStr.indexOf(e.charAt(f++));a=this._keyStr.indexOf(e.charAt(f++));n=s<<2|o>>4;r=(o&15)<<4|u>>2;i=(u&3)<<6|a;t=t+String.fromCharCode(n);if(u!=64){t=t+String.fromCharCode(r)}if(a!=64){t=t+String.fromCharCode(i)}}t=Base64._utf8_decode(t);return t},_utf8_encode:function(e){e=e.replace(/\r\n/g,"\n");var t="";for(var n=0;n<e.length;n++){var r=e.charCodeAt(n);if(r<128){t+=String.fromCharCode(r)}else if(r>127&&r<2048){t+=String.fromCharCode(r>>6|192);t+=String.fromCharCode(r&63|128)}else{t+=String.fromCharCode(r>>12|224);t+=String.fromCharCode(r>>6&63|128);t+=String.fromCharCode(r&63|128)}}return t},_utf8_decode:function(e){var t="";var n=0;var r=c1=c2=0;while(n<e.length){r=e.charCodeAt(n);if(r<128){t+=String.fromCharCode(r);n++}else if(r>191&&r<224){c2=e.charCodeAt(n+1);t+=String.fromCharCode((r&31)<<6|c2&63);n+=2}else{c2=e.charCodeAt(n+1);c3=e.charCodeAt(n+2);t+=String.fromCharCode((r&15)<<12|(c2&63)<<6|c3&63);n+=3}}return t}}

      var publishableKey = '<YOUR API KEY>';

      var xhr = new XMLHttpRequest();
      xhr.open("get", "https://api.betternow.org/fundraisers/alot", true);
      xhr.setRequestHeader('Authorization', 'Bearer ' + Base64.encode(publishableKey + ':'))
      xhr.setRequestHeader('Accept', 'application/vnd.betternow+json; version=1');
      xhr.onload = function() {
        alert(this.responseText);
      }
      xhr.send();
    </script>
  </head>
  <body>
    <p>If you don't get an alert something is wrong...</p>
  </body>
</html>
```

