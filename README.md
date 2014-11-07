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

* [Authentication](#authentication)
* [Caching](#caching)
* [Clients](#clients)
* [CORS](#cors)
* [Rate Limits](#rate-limits)
* [Pagination via Ranges](#pagination-via-ranges)
* [JSON Schema](#json-schema)
* ["Summary" vs. "Full" representations](#summary-vs-full-representations)
* [Event](#event)
* [Fundraising Page](#fundraising-page)
* [Organisation](#organisation)
* [Project](#project)
* [Team](#team)

<!-- toc stop -->


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




## Event
An Event is something that takes place at a particular time and/or place. It could be a sporting event like the Copenhagen Marathon 2013, or a holiday like Christmas 2014

### Attributes
| Name | Type | Description | Example |
| ------- | ------- | ------- | ------- |
| **choose_project_to_fundraise_for_url** | *uri* | The url on BetterNow for people who want to fundraise in connection with an event | `"https://www.betternow.org/dk/fundraisers/new?event_id=1234567"` |
| **cover_media:image:url** | *uri* | The url for the image. On the BetterNow site, the video takes precedence if both exist. | `"https://cnd.example.net/image.jpg"` |
| **cover_media:video:url** | *uri* | The url for the video. Currently only YouTube and Vimeo are supported. Could be blank. | `"https://youtu.be/12345"` |
| **created_at** | *date-time* | when event was created | `"2012-01-01T12:00:00Z"` |
| **description** | *string* | Text describing the Event added by the event organiser. Contains HTML. | `"<p>This is really, <b>REALLY</b> great</p> <br><br>"` |
| **end_date** | *date-time* | The date when the Event ends. May be blank in the case of a single day event. | `"2012-01-01T12:00:00Z"` |
| **html_url** | *uri* | The url to the Event page on BetterNow | `"https://www.betternow.org/dk/events/copenhagen-marathon-2013"` |
| **id** | *string* | unique identifier of event | `1234567` |
| **location:city** | *string* | The name of a city | `"København"` |
| **name** | *string* | the name of the Event | `"Copenhagen Marathon 2013"` |
| **logo_url** | *uri* | The logo for the Event | `"https://cdn.example.net/logo.png"` |
| **updated_at** | *date-time* | when event was updated | `"2012-01-01T12:00:00Z"` |
| **start_date** | *date-time* | The date when the Event starts | `"2012-01-01T12:00:00Z"` |
| **url** | *uri* |  | `"https://api.betternow.org/events/1234567"` |
### Event Info
Info for existing event.

```
GET /events/{event_id}
```


#### Curl Example
```bash
$ curl -n -X GET https://api.betternow.org/events/$EVENT_ID

```


#### Response Example
```
HTTP/1.1 200 OK
```
```json
{
  "choose_project_to_fundraise_for_url": "https://www.betternow.org/dk/fundraisers/new?event_id=1234567",
  "cover_media": {
    "image": {
      "url": "https://cnd.example.net/image.jpg"
    },
    "video": {
      "url": "https://youtu.be/12345"
    }
  },
  "created_at": "2012-01-01T12:00:00Z",
  "description": "<p>This is really, <b>REALLY</b> great</p> <br><br>",
  "end_date": "2012-01-01T12:00:00Z",
  "html_url": "https://www.betternow.org/dk/events/copenhagen-marathon-2013",
  "id": 1234567,
  "location": {
    "city": "København"
  },
  "name": "Copenhagen Marathon 2013",
  "logo_url": "https://cdn.example.net/logo.png",
  "updated_at": "2012-01-01T12:00:00Z",
  "start_date": "2012-01-01T12:00:00Z",
  "url": "https://api.betternow.org/events/1234567"
}
```

### Event List
List existing events.

```
GET /events
```


#### Curl Example
```bash
$ curl -n -X GET https://api.betternow.org/events

```


#### Response Example
```
HTTP/1.1 200 OK
```
```json
[
  {
    "choose_project_to_fundraise_for_url": "https://www.betternow.org/dk/fundraisers/new?event_id=1234567",
    "cover_media": {
      "image": {
        "url": "https://cnd.example.net/image.jpg"
      },
      "video": {
        "url": "https://youtu.be/12345"
      }
    },
    "created_at": "2012-01-01T12:00:00Z",
    "description": "<p>This is really, <b>REALLY</b> great</p> <br><br>",
    "end_date": "2012-01-01T12:00:00Z",
    "html_url": "https://www.betternow.org/dk/events/copenhagen-marathon-2013",
    "id": 1234567,
    "location": {
      "city": "København"
    },
    "name": "Copenhagen Marathon 2013",
    "logo_url": "https://cdn.example.net/logo.png",
    "updated_at": "2012-01-01T12:00:00Z",
    "start_date": "2012-01-01T12:00:00Z",
    "url": "https://api.betternow.org/events/1234567"
  }
]
```

### Event List Projects
List all Projects associated with an Event

```
GET /events/{event_id}/projects
```


#### Curl Example
```bash
$ curl -n -X GET https://api.betternow.org/events/$EVENT_ID/projects

```


#### Response Example
```
HTTP/1.1 200 OK
```
```json
[
  {
    "created_at": "2012-01-01T12:00:00Z",
    "description": "We need your money for this <b>GREAT</b> project",
    "donations": {
      "count": 123,
      "total_donated": {
        "cents": 1234500,
        "currency": "EUR"
      },
      "url": "https://api.betternow.org/projects/1234567/donations"
    },
    "donate_url": "https://www.betternow.org/dk/fundraisers/helpnow-indsamling21/donations/new",
    "html_url": "https://www.betternow.org/dk/helpnow-projekt",
    "id": 1234567,
    "fundraisers": [
      {
        "id": 1234567,
        "headline": "Firstname Lastname's Fundraiser for HelpNow",
        "url": "https://api.betternow.org/fundraisers/1234567",
        "html_url": "https://www.betternow.org/dk/firstname-lastnames-fundraiser-for-helpnow"
      }
    ],
    "name": "HelpNows generelle arbejde",
    "new_fundraiser_url": "https://www.betternow.org/dk/projects/helpnow-projekt/fundraisers/new",
    "updated_at": "2012-01-01T12:00:00Z",
    "url": "https://api.betternow.org/projects/1234567"
  }
]
```

### Event List Fundraisers
List all Fundraisers associated with an Event

```
GET /events/{event_id}/fundraisers
```


#### Curl Example
```bash
$ curl -n -X GET https://api.betternow.org/events/$EVENT_ID/fundraisers

```


#### Response Example
```
HTTP/1.1 200 OK
```
```json
[
  {
    "activity_score": 987654321,
    "cover_media": {
      "image": {
        "url": "https://cnd.example.net/image.jpg"
      },
      "video": {
        "url": "https://youtu.be/12345"
      }
    },
    "created_at": "2012-01-01T12:00:00Z",
    "description": "<p>This is really, <b>REALLY</b> great</p> <br><br>",
    "donate_url": "https://www.betternow.org/dk/fundraisers/firstname-lastnames-fundraiser/donations/new",
    "donations": {
      "count": 123,
      "total_donated": {
        "cents": 1234500,
        "currency": "EUR"
      },
      "url": "https://api.betternow.org/fundraisers/1234567/donations"
    },
    "end_date": "2012-01-01T12:00:00Z",
    "goal": {
      "cents": 1234500,
      "currency": "EUR"
    },
    "headline": "Firstname Lastname's Fundraiser for HelpNow",
    "html_url": "https://www.betternow.org/dk/firstname-lastnames-fundraiser-for-helpnow",
    "id": 1234567,
    "owner": {
      "avatar_url": "https://cdn.example.net/avatar.jpg",
      "first_name": "Firstname",
      "last_name": "Lastname"
    },
    "recipient": {
      "id": 1234567,
      "name": "HelpNow",
      "url": "https://api.betternow.org/organisations/1234567",
      "html_url": "https://www.betternow.org/dk/helpnow"
    },
    "slug": "firstname-lastnames-fundraiser-for-helpnow",
    "updated_at": "2012-01-01T12:00:00Z",
    "url": "https://api.betternow.org/fundraisers/1234567"
  }
]
```

### Event List Teams
List all Teams associated with an Event

```
GET /events/{event_id}/teams
```


#### Curl Example
```bash
$ curl -n -X GET https://api.betternow.org/events/$EVENT_ID/teams

```


#### Response Example
```
HTTP/1.1 200 OK
```
```json
[
  {
    "captain": {
      "avatar_url": "https://cdn.example.net/avatar.jpg",
      "first_name": "Firstname",
      "last_name": "Lastname"
    },
    "cover_media": {
      "image": {
        "url": "https://cnd.example.net/image.jpg"
      },
      "video": {
        "url": "https://youtu.be/12345"
      }
    },
    "created_at": "2012-01-01T12:00:00Z",
    "description": "<p>This is really, <b>REALLY</b> great</p> <br><br>",
    "donations": {
      "count": 123,
      "total_donated": {
        "cents": 1234500,
        "currency": "EUR"
      },
      "url": "https://api.betternow.org/teams/1234567/donations"
    },
    "fundraisers": [
      {
        "id": 1234567,
        "headline": "Firstname Lastname's Fundraiser for HelpNow",
        "url": "https://api.betternow.org/fundraisers/1234567",
        "html_url": "https://www.betternow.org/dk/firstname-lastnames-fundraiser-for-helpnow"
      }
    ],
    "html_url": "https://www.betternow.org/dk/teams/team-novo",
    "id": 1234567,
    "logo_url": "https://cdn.example.net/logo.png",
    "name": "Team NOVO",
    "slug": "team-novo",
    "updated_at": "2012-01-01T12:00:00Z",
    "url": "https://api.betternow.org/team/1234567"
  }
]
```


## Fundraising Page
Detailed information about a single Fundraising Page on BetterNow.org

### Attributes
| Name | Type | Description | Example |
| ------- | ------- | ------- | ------- |
| **activity_score** | *integer* | A number that can be used for sorting lists of fundraisers. More recently active fundraisers should have a higher activity score than fundraisers who have raised more money long ago. | `987654321` |
| **cover_media:image:url** | *uri* | The url for the image. On the BetterNow site, the video takes precedence if both exist. | `"https://cnd.example.net/image.jpg"` |
| **cover_media:video:url** | *uri* | The url for the video. Currently only YouTube and Vimeo are supported. Could be blank. | `"https://youtu.be/12345"` |
| **created_at** | *date-time* | when resource was created | `"2012-01-01T12:00:00Z"` |
| **description** | *string* | The text written by the fundraiser owner. Contains HTML. | `"<p>This is really, <b>REALLY</b> great</p> <br><br>"` |
| **donate_url** | *uri* | The current url to donate via the fundraising page on BetterNow. This can, and does, change. Requests to old urls will be redirect to the current url. | `"https://www.betternow.org/dk/fundraisers/firstname-lastnames-fundraiser/donations/new"` |
| **donations:count** | *integer* | The count of all donations made to this fundraiser | `123` |
| **donations:total_donated:cents** | *integer* | Numeric amount in cents | `1234500` |
| **donations:total_donated:currency** | *string* | 3 character currency code, as specified in ISO 4217<br/> **pattern:** <code>^([A-Z]{3})$</code> | `"EUR"` |
| **donations:url** | *uri* | The url to retrieve details on all donations made to this fundraiser | `"https://api.betternow.org/fundraisers/1234567/donations"` |
| **end_date** | *date-time* | The end date for a fundraiser. | `"2012-01-01T12:00:00Z"` |
| **goal:cents** | *integer* | Numeric amount in cents | `1234500` |
| **goal:currency** | *string* | 3 character currency code, as specified in ISO 4217<br/> **pattern:** <code>^([A-Z]{3})$</code> | `"EUR"` |
| **headline** | *string* | The headline for this fundraising page | `"Firstname Lastname's Fundraiser for HelpNow"` |
| **html_url** | *uri* | The current url to view the fundraising page on BetterNow. This can, and does, change. Requests to old urls will be redirect to the current url. | `"https://www.betternow.org/dk/firstname-lastnames-fundraiser-for-helpnow"` |
| **id** | *string* | The unique identifier of the fundraising page | `1234567` |
| **owner:avatar_url** | *uri* | The URL for the avatar image for the donor | `"https://cdn.example.net/avatar.jpg"` |
| **owner:first_name** | *string* | The first name of the user | `"Firstname"` |
| **owner:last_name** | *string* | The last name of the user | `"Lastname"` |
| **recipient:id** | *string* | Unique identifier of organisation | `1234567` |
| **recipient:name** | *string* | The name of the Organisation | `"HelpNow"` |
| **recipient:url** | *uri* |  | `"https://api.betternow.org/organisations/1234567"` |
| **recipient:html_url** | *uri* | The current url to view the organisation page on BetterNow. This can, and does, change. Requests to old urls will be redirect to the current url. | `"https://www.betternow.org/dk/helpnow"` |
| **slug** | *string* | The current url path component to identify the fundraiser. This can, and does, change.<br/> **pattern:** <code>^([a-z0-9-]{2,})$</code> | `"firstname-lastnames-fundraiser-for-helpnow"` |
| **updated_at** | *date-time* | when resource was updated | `"2012-01-01T12:00:00Z"` |
| **url** | *uri* |  | `"https://api.betternow.org/fundraisers/1234567"` |
### Fundraising Page Info
Info for existing fundraiser.

```
GET /fundraisers/{fundraiser_id}
```


#### Curl Example
```bash
$ curl -n -X GET https://api.betternow.org/fundraisers/$FUNDRAISER_ID

```


#### Response Example
```
HTTP/1.1 200 OK
```
```json
{
  "activity_score": 987654321,
  "cover_media": {
    "image": {
      "url": "https://cnd.example.net/image.jpg"
    },
    "video": {
      "url": "https://youtu.be/12345"
    }
  },
  "created_at": "2012-01-01T12:00:00Z",
  "description": "<p>This is really, <b>REALLY</b> great</p> <br><br>",
  "donate_url": "https://www.betternow.org/dk/fundraisers/firstname-lastnames-fundraiser/donations/new",
  "donations": {
    "count": 123,
    "total_donated": {
      "cents": 1234500,
      "currency": "EUR"
    },
    "url": "https://api.betternow.org/fundraisers/1234567/donations"
  },
  "end_date": "2012-01-01T12:00:00Z",
  "goal": {
    "cents": 1234500,
    "currency": "EUR"
  },
  "headline": "Firstname Lastname's Fundraiser for HelpNow",
  "html_url": "https://www.betternow.org/dk/firstname-lastnames-fundraiser-for-helpnow",
  "id": 1234567,
  "owner": {
    "avatar_url": "https://cdn.example.net/avatar.jpg",
    "first_name": "Firstname",
    "last_name": "Lastname"
  },
  "recipient": {
    "id": 1234567,
    "name": "HelpNow",
    "url": "https://api.betternow.org/organisations/1234567",
    "html_url": "https://www.betternow.org/dk/helpnow"
  },
  "slug": "firstname-lastnames-fundraiser-for-helpnow",
  "updated_at": "2012-01-01T12:00:00Z",
  "url": "https://api.betternow.org/fundraisers/1234567"
}
```

### Fundraising Page List
List existing fundraisers.

```
GET /fundraisers
```


#### Curl Example
```bash
$ curl -n -X GET https://api.betternow.org/fundraisers

```


#### Response Example
```
HTTP/1.1 200 OK
```
```json
[
  {
    "activity_score": 987654321,
    "cover_media": {
      "image": {
        "url": "https://cnd.example.net/image.jpg"
      },
      "video": {
        "url": "https://youtu.be/12345"
      }
    },
    "created_at": "2012-01-01T12:00:00Z",
    "description": "<p>This is really, <b>REALLY</b> great</p> <br><br>",
    "donate_url": "https://www.betternow.org/dk/fundraisers/firstname-lastnames-fundraiser/donations/new",
    "donations": {
      "count": 123,
      "total_donated": {
        "cents": 1234500,
        "currency": "EUR"
      },
      "url": "https://api.betternow.org/fundraisers/1234567/donations"
    },
    "end_date": "2012-01-01T12:00:00Z",
    "goal": {
      "cents": 1234500,
      "currency": "EUR"
    },
    "headline": "Firstname Lastname's Fundraiser for HelpNow",
    "html_url": "https://www.betternow.org/dk/firstname-lastnames-fundraiser-for-helpnow",
    "id": 1234567,
    "owner": {
      "avatar_url": "https://cdn.example.net/avatar.jpg",
      "first_name": "Firstname",
      "last_name": "Lastname"
    },
    "recipient": {
      "id": 1234567,
      "name": "HelpNow",
      "url": "https://api.betternow.org/organisations/1234567",
      "html_url": "https://www.betternow.org/dk/helpnow"
    },
    "slug": "firstname-lastnames-fundraiser-for-helpnow",
    "updated_at": "2012-01-01T12:00:00Z",
    "url": "https://api.betternow.org/fundraisers/1234567"
  }
]
```

### Fundraising Page List Donations
List the donations for existing fundraiser. Donations will always be returned in reverse-chronological order (newest first).

```
GET /fundraisers/{fundraiser_id}/donations
```


#### Curl Example
```bash
$ curl -n -X GET https://api.betternow.org/fundraisers/$FUNDRAISER_ID/donations

```


#### Response Example
```
HTTP/1.1 200 OK
```
```json
[
  {
    "amount": {
      "cents": 12345,
      "currency": "EUR"
    },
    "comment": "Wow, what a great idea!",
    "created_at": "2012-01-01T12:00:00Z",
    "id": 1234567,
    "name": "Joes Truck Stop",
    "updated_at": "2012-01-01T12:00:00Z"
  }
]
```


## Organisation
An Organisation can receive Donations on BetterNow

### Attributes
| Name | Type | Description | Example |
| ------- | ------- | ------- | ------- |
| **cover_media:image:url** | *uri* | The url for the image. On the BetterNow site, the video takes precedence if both exist. | `"https://cnd.example.net/image.jpg"` |
| **cover_media:video:url** | *uri* | The url for the video. Currently only YouTube and Vimeo are supported. Could be blank. | `"https://youtu.be/12345"` |
| **created_at** | *date-time* | When organisation was created | `"2012-01-01T12:00:00Z"` |
| **description** | *string* | The text written by the Organisation's administrators. Contains HTML | `"HelpNow is a dummy organisation created by BetterNow - to help us doing tutorial videos and screenshots. It is not a real organisation, and you cannot donate here.<br><br><br>"` |
| **donations:count** | *integer* | The count of all donations made to this project | `123` |
| **donations:total_donated:cents** | *integer* | Numeric amount in cents | `1234500` |
| **donations:total_donated:currency** | *string* | 3 character currency code, as specified in ISO 4217<br/> **pattern:** <code>^([A-Z]{3})$</code> | `"EUR"` |
| **donations:url** | *uri* | The url to retrieve details on all donations made to this organisation | `"https://api.betternow.org/organisations/1234567/donations"` |
| **donate_url** | *uri* | The current url to donate directly to the organisation on BetterNow. This can, and does, change. Requests to old urls will be redirect to the current url. | `"https://www.betternow.org/dk/fundraisers/helpnow-indsamling1/donations/new"` |
| **fundraisers** | *array* | An array of summarized fundraisers | `[{"id"=>1234567, "headline"=>"Firstname Lastname's Fundraiser for HelpNow", "url"=>"https://api.betternow.org/fundraisers/1234567", "html_url"=>"https://www.betternow.org/dk/firstname-lastnames-fundraiser-for-helpnow"}]` |
| **html_url** | *uri* | The current url to view the organisation page on BetterNow. This can, and does, change. Requests to old urls will be redirect to the current url. | `"https://www.betternow.org/dk/helpnow"` |
| **id** | *string* | Unique identifier of organisation | `1234567` |
| **logo_url** | *uri* | The logo for the Organisation | `"https://cdn.example.net/logo.png"` |
| **name** | *string* | The name of the Organisation | `"HelpNow"` |
| **new_fundraiser_url** | *uri* | The current url to create a new Fundraiser for this organisation on BetterNow. This can, and does, change. Requests to old urls will redirect to the current url. | `"https://www.betternow.org/dk/projects/helpnow-projekt/fundraisers/new"` |
| **projects** | *array* |  | `[{"id"=>1234567, "name"=>"HelpNows generelle arbejde", "url"=>"https://api.betternow.org/projects/1234567", "html_url"=>"https://www.betternow.org/dk/helpnow-projekt"}]` |
| **updated_at** | *date-time* | When organisation was updated | `"2012-01-01T12:00:00Z"` |
| **url** | *uri* |  | `"https://api.betternow.org/organisations/1234567"` |
### Organisation Info
Info for existing organisation.

```
GET /organisations/{organisation_id}
```


#### Curl Example
```bash
$ curl -n -X GET https://api.betternow.org/organisations/$ORGANISATION_ID

```


#### Response Example
```
HTTP/1.1 200 OK
```
```json
{
  "cover_media": {
    "image": {
      "url": "https://cnd.example.net/image.jpg"
    },
    "video": {
      "url": "https://youtu.be/12345"
    }
  },
  "created_at": "2012-01-01T12:00:00Z",
  "description": "HelpNow is a dummy organisation created by BetterNow - to help us doing tutorial videos and screenshots. It is not a real organisation, and you cannot donate here.<br><br><br>",
  "donations": {
    "count": 123,
    "total_donated": {
      "cents": 1234500,
      "currency": "EUR"
    },
    "url": "https://api.betternow.org/organisations/1234567/donations"
  },
  "donate_url": "https://www.betternow.org/dk/fundraisers/helpnow-indsamling1/donations/new",
  "fundraisers": [
    {
      "id": 1234567,
      "headline": "Firstname Lastname's Fundraiser for HelpNow",
      "url": "https://api.betternow.org/fundraisers/1234567",
      "html_url": "https://www.betternow.org/dk/firstname-lastnames-fundraiser-for-helpnow"
    }
  ],
  "html_url": "https://www.betternow.org/dk/helpnow",
  "id": 1234567,
  "logo_url": "https://cdn.example.net/logo.png",
  "name": "HelpNow",
  "new_fundraiser_url": "https://www.betternow.org/dk/projects/helpnow-projekt/fundraisers/new",
  "projects": [
    {
      "id": 1234567,
      "name": "HelpNows generelle arbejde",
      "url": "https://api.betternow.org/projects/1234567",
      "html_url": "https://www.betternow.org/dk/helpnow-projekt"
    }
  ],
  "updated_at": "2012-01-01T12:00:00Z",
  "url": "https://api.betternow.org/organisations/1234567"
}
```

### Organisation List Projects
List all Projects for an existing Organisation. Projects will be ordered by activity score, descending.

```
GET /organisations/{organisation_id}/projects
```


#### Curl Example
```bash
$ curl -n -X GET https://api.betternow.org/organisations/$ORGANISATION_ID/projects

```


#### Response Example
```
HTTP/1.1 200 OK
```
```json
[
  {
    "created_at": "2012-01-01T12:00:00Z",
    "description": "We need your money for this <b>GREAT</b> project",
    "donations": {
      "count": 123,
      "total_donated": {
        "cents": 1234500,
        "currency": "EUR"
      },
      "url": "https://api.betternow.org/projects/1234567/donations"
    },
    "donate_url": "https://www.betternow.org/dk/fundraisers/helpnow-indsamling21/donations/new",
    "html_url": "https://www.betternow.org/dk/helpnow-projekt",
    "id": 1234567,
    "fundraisers": [
      {
        "id": 1234567,
        "headline": "Firstname Lastname's Fundraiser for HelpNow",
        "url": "https://api.betternow.org/fundraisers/1234567",
        "html_url": "https://www.betternow.org/dk/firstname-lastnames-fundraiser-for-helpnow"
      }
    ],
    "name": "HelpNows generelle arbejde",
    "new_fundraiser_url": "https://www.betternow.org/dk/projects/helpnow-projekt/fundraisers/new",
    "updated_at": "2012-01-01T12:00:00Z",
    "url": "https://api.betternow.org/projects/1234567"
  }
]
```

### Organisation List Fundraisers
List all Fundraisers for an existing Organisation. Fundraisers will be ordered by activity score, descending.

```
GET /organisations/{organisation_id}/fundraisers
```


#### Curl Example
```bash
$ curl -n -X GET https://api.betternow.org/organisations/$ORGANISATION_ID/fundraisers

```


#### Response Example
```
HTTP/1.1 200 OK
```
```json
[
  {
    "activity_score": 987654321,
    "cover_media": {
      "image": {
        "url": "https://cnd.example.net/image.jpg"
      },
      "video": {
        "url": "https://youtu.be/12345"
      }
    },
    "created_at": "2012-01-01T12:00:00Z",
    "description": "<p>This is really, <b>REALLY</b> great</p> <br><br>",
    "donate_url": "https://www.betternow.org/dk/fundraisers/firstname-lastnames-fundraiser/donations/new",
    "donations": {
      "count": 123,
      "total_donated": {
        "cents": 1234500,
        "currency": "EUR"
      },
      "url": "https://api.betternow.org/fundraisers/1234567/donations"
    },
    "end_date": "2012-01-01T12:00:00Z",
    "goal": {
      "cents": 1234500,
      "currency": "EUR"
    },
    "headline": "Firstname Lastname's Fundraiser for HelpNow",
    "html_url": "https://www.betternow.org/dk/firstname-lastnames-fundraiser-for-helpnow",
    "id": 1234567,
    "owner": {
      "avatar_url": "https://cdn.example.net/avatar.jpg",
      "first_name": "Firstname",
      "last_name": "Lastname"
    },
    "recipient": {
      "id": 1234567,
      "name": "HelpNow",
      "url": "https://api.betternow.org/organisations/1234567",
      "html_url": "https://www.betternow.org/dk/helpnow"
    },
    "slug": "firstname-lastnames-fundraiser-for-helpnow",
    "updated_at": "2012-01-01T12:00:00Z",
    "url": "https://api.betternow.org/fundraisers/1234567"
  }
]
```

### Organisation List Donations
List the donations for an existing Organisation. Donations will always be returned in reverse-chronological order (newest first).

```
GET /organisations/{organisation_id}/donations
```


#### Curl Example
```bash
$ curl -n -X GET https://api.betternow.org/organisations/$ORGANISATION_ID/donations

```


#### Response Example
```
HTTP/1.1 200 OK
```
```json
[
  {
    "amount": {
      "cents": 12345,
      "currency": "EUR"
    },
    "comment": "Wow, what a great idea!",
    "created_at": "2012-01-01T12:00:00Z",
    "id": 1234567,
    "name": "Joes Truck Stop",
    "updated_at": "2012-01-01T12:00:00Z"
  }
]
```


## Project
A Project is a specific cause that Users can Fundraise for. An Organisation typically has several Projects

### Attributes
| Name | Type | Description | Example |
| ------- | ------- | ------- | ------- |
| **created_at** | *date-time* | When project was created | `"2012-01-01T12:00:00Z"` |
| **description** | *string* | The text written by the Project's administrators. Contains HTML | `"We need your money for this <b>GREAT</b> project"` |
| **donations:count** | *integer* | The count of all donations made to this project | `123` |
| **donations:total_donated:cents** | *integer* | Numeric amount in cents | `1234500` |
| **donations:total_donated:currency** | *string* | 3 character currency code, as specified in ISO 4217<br/> **pattern:** <code>^([A-Z]{3})$</code> | `"EUR"` |
| **donations:url** | *uri* | The url to retrieve details on all donations made to this project | `"https://api.betternow.org/projects/1234567/donations"` |
| **donate_url** | *uri* | The current url to donate directly to the project on BetterNow. This can, and does, change. Requests to old urls will be redirect to the current url. | `"https://www.betternow.org/dk/fundraisers/helpnow-indsamling21/donations/new"` |
| **html_url** | *uri* | The current url to view the project page on BetterNow. This can, and does, change. Requests to old urls will be redirect to the current url. | `"https://www.betternow.org/dk/helpnow-projekt"` |
| **id** | *string* | Unique identifier of project | `1234567` |
| **fundraisers** | *array* | An array of summarized fundraisers | `[{"id"=>1234567, "headline"=>"Firstname Lastname's Fundraiser for HelpNow", "url"=>"https://api.betternow.org/fundraisers/1234567", "html_url"=>"https://www.betternow.org/dk/firstname-lastnames-fundraiser-for-helpnow"}]` |
| **name** | *string* | The name of the Project | `"HelpNows generelle arbejde"` |
| **new_fundraiser_url** | *uri* | The current url to create a new Fundraiser for this project on BetterNow. This can, and does, change. Requests to old urls will redirect to the current url. | `"https://www.betternow.org/dk/projects/helpnow-projekt/fundraisers/new"` |
| **updated_at** | *date-time* | When project was updated | `"2012-01-01T12:00:00Z"` |
| **url** | *uri* |  | `"https://api.betternow.org/projects/1234567"` |
### Project Info
Info for existing project.

```
GET /projects/{project_id}
```


#### Curl Example
```bash
$ curl -n -X GET https://api.betternow.org/projects/$PROJECT_ID

```


#### Response Example
```
HTTP/1.1 200 OK
```
```json
{
  "created_at": "2012-01-01T12:00:00Z",
  "description": "We need your money for this <b>GREAT</b> project",
  "donations": {
    "count": 123,
    "total_donated": {
      "cents": 1234500,
      "currency": "EUR"
    },
    "url": "https://api.betternow.org/projects/1234567/donations"
  },
  "donate_url": "https://www.betternow.org/dk/fundraisers/helpnow-indsamling21/donations/new",
  "html_url": "https://www.betternow.org/dk/helpnow-projekt",
  "id": 1234567,
  "fundraisers": [
    {
      "id": 1234567,
      "headline": "Firstname Lastname's Fundraiser for HelpNow",
      "url": "https://api.betternow.org/fundraisers/1234567",
      "html_url": "https://www.betternow.org/dk/firstname-lastnames-fundraiser-for-helpnow"
    }
  ],
  "name": "HelpNows generelle arbejde",
  "new_fundraiser_url": "https://www.betternow.org/dk/projects/helpnow-projekt/fundraisers/new",
  "updated_at": "2012-01-01T12:00:00Z",
  "url": "https://api.betternow.org/projects/1234567"
}
```

### Project List Fundraisers
List all Fundraisers for an existing Project. Fundraisers will be ordered by activity score, descending.

```
GET /projects/{project_id}/fundraisers
```


#### Curl Example
```bash
$ curl -n -X GET https://api.betternow.org/projects/$PROJECT_ID/fundraisers

```


#### Response Example
```
HTTP/1.1 200 OK
```
```json
[
  {
    "activity_score": 987654321,
    "cover_media": {
      "image": {
        "url": "https://cnd.example.net/image.jpg"
      },
      "video": {
        "url": "https://youtu.be/12345"
      }
    },
    "created_at": "2012-01-01T12:00:00Z",
    "description": "<p>This is really, <b>REALLY</b> great</p> <br><br>",
    "donate_url": "https://www.betternow.org/dk/fundraisers/firstname-lastnames-fundraiser/donations/new",
    "donations": {
      "count": 123,
      "total_donated": {
        "cents": 1234500,
        "currency": "EUR"
      },
      "url": "https://api.betternow.org/fundraisers/1234567/donations"
    },
    "end_date": "2012-01-01T12:00:00Z",
    "goal": {
      "cents": 1234500,
      "currency": "EUR"
    },
    "headline": "Firstname Lastname's Fundraiser for HelpNow",
    "html_url": "https://www.betternow.org/dk/firstname-lastnames-fundraiser-for-helpnow",
    "id": 1234567,
    "owner": {
      "avatar_url": "https://cdn.example.net/avatar.jpg",
      "first_name": "Firstname",
      "last_name": "Lastname"
    },
    "recipient": {
      "id": 1234567,
      "name": "HelpNow",
      "url": "https://api.betternow.org/organisations/1234567",
      "html_url": "https://www.betternow.org/dk/helpnow"
    },
    "slug": "firstname-lastnames-fundraiser-for-helpnow",
    "updated_at": "2012-01-01T12:00:00Z",
    "url": "https://api.betternow.org/fundraisers/1234567"
  }
]
```

### Project List Donations
List the donations for existing project. Donations will always be returned in reverse-chronological order (newest first).

```
GET /projects/{project_id}/donations
```


#### Curl Example
```bash
$ curl -n -X GET https://api.betternow.org/projects/$PROJECT_ID/donations

```


#### Response Example
```
HTTP/1.1 200 OK
```
```json
[
  {
    "amount": {
      "cents": 12345,
      "currency": "EUR"
    },
    "comment": "Wow, what a great idea!",
    "created_at": "2012-01-01T12:00:00Z",
    "id": 1234567,
    "name": "Joes Truck Stop",
    "updated_at": "2012-01-01T12:00:00Z"
  }
]
```


## Team
A Team is a collection of Fundraisers, who may or may not be raising money in connection with a single Event.

### Attributes
| Name | Type | Description | Example |
| ------- | ------- | ------- | ------- |
| **captain:avatar_url** | *uri* | The URL for the avatar image for the donor | `"https://cdn.example.net/avatar.jpg"` |
| **captain:first_name** | *string* | The first name of the user | `"Firstname"` |
| **captain:last_name** | *string* | The last name of the user | `"Lastname"` |
| **cover_media:image:url** | *uri* | The url for the image. On the BetterNow site, the video takes precedence if both exist. | `"https://cnd.example.net/image.jpg"` |
| **cover_media:video:url** | *uri* | The url for the video. Currently only YouTube and Vimeo are supported. Could be blank. | `"https://youtu.be/12345"` |
| **created_at** | *date-time* | when team was created | `"2012-01-01T12:00:00Z"` |
| **description** | *string* | Text describing the Team added by the Team Captain. Contains HTML. | `"<p>This is really, <b>REALLY</b> great</p> <br><br>"` |
| **donations:count** | *integer* | The count of all donations made | `123` |
| **donations:total_donated:cents** | *integer* | Numeric amount in cents | `1234500` |
| **donations:total_donated:currency** | *string* | 3 character currency code, as specified in ISO 4217<br/> **pattern:** <code>^([A-Z]{3})$</code> | `"EUR"` |
| **donations:url** | *uri* | The url to retrieve details on all donations made via team members | `"https://api.betternow.org/teams/1234567/donations"` |
| **fundraisers** | *array* | An array of summarized fundraisers, ordered by total donated, descending | `[{"id"=>1234567, "headline"=>"Firstname Lastname's Fundraiser for HelpNow", "url"=>"https://api.betternow.org/fundraisers/1234567", "html_url"=>"https://www.betternow.org/dk/firstname-lastnames-fundraiser-for-helpnow"}]` |
| **html_url** | *uri* | The url to the Team page on BetterNow | `"https://www.betternow.org/dk/teams/team-novo"` |
| **id** | *string* | unique identifier of team | `1234567` |
| **logo_url** | *uri* | The logo for the Team | `"https://cdn.example.net/logo.png"` |
| **name** | *string* | the name of the Team | `"Team NOVO"` |
| **slug** | *string* | The current url path component to identify the team. This can, and does, change.<br/> **pattern:** <code>^([a-z0-9-]{2,})$</code> | `"team-novo"` |
| **updated_at** | *date-time* | when team was updated | `"2012-01-01T12:00:00Z"` |
| **url** | *uri* |  | `"https://api.betternow.org/team/1234567"` |
### Team Info
Info for existing team.

```
GET /teams/{team_id}
```


#### Curl Example
```bash
$ curl -n -X GET https://api.betternow.org/teams/$TEAM_ID

```


#### Response Example
```
HTTP/1.1 200 OK
```
```json
{
  "captain": {
    "avatar_url": "https://cdn.example.net/avatar.jpg",
    "first_name": "Firstname",
    "last_name": "Lastname"
  },
  "cover_media": {
    "image": {
      "url": "https://cnd.example.net/image.jpg"
    },
    "video": {
      "url": "https://youtu.be/12345"
    }
  },
  "created_at": "2012-01-01T12:00:00Z",
  "description": "<p>This is really, <b>REALLY</b> great</p> <br><br>",
  "donations": {
    "count": 123,
    "total_donated": {
      "cents": 1234500,
      "currency": "EUR"
    },
    "url": "https://api.betternow.org/teams/1234567/donations"
  },
  "fundraisers": [
    {
      "id": 1234567,
      "headline": "Firstname Lastname's Fundraiser for HelpNow",
      "url": "https://api.betternow.org/fundraisers/1234567",
      "html_url": "https://www.betternow.org/dk/firstname-lastnames-fundraiser-for-helpnow"
    }
  ],
  "html_url": "https://www.betternow.org/dk/teams/team-novo",
  "id": 1234567,
  "logo_url": "https://cdn.example.net/logo.png",
  "name": "Team NOVO",
  "slug": "team-novo",
  "updated_at": "2012-01-01T12:00:00Z",
  "url": "https://api.betternow.org/team/1234567"
}
```

### Team List
List existing teams.

```
GET /teams
```


#### Curl Example
```bash
$ curl -n -X GET https://api.betternow.org/teams

```


#### Response Example
```
HTTP/1.1 200 OK
```
```json
[
  {
    "captain": {
      "avatar_url": "https://cdn.example.net/avatar.jpg",
      "first_name": "Firstname",
      "last_name": "Lastname"
    },
    "cover_media": {
      "image": {
        "url": "https://cnd.example.net/image.jpg"
      },
      "video": {
        "url": "https://youtu.be/12345"
      }
    },
    "created_at": "2012-01-01T12:00:00Z",
    "description": "<p>This is really, <b>REALLY</b> great</p> <br><br>",
    "donations": {
      "count": 123,
      "total_donated": {
        "cents": 1234500,
        "currency": "EUR"
      },
      "url": "https://api.betternow.org/teams/1234567/donations"
    },
    "fundraisers": [
      {
        "id": 1234567,
        "headline": "Firstname Lastname's Fundraiser for HelpNow",
        "url": "https://api.betternow.org/fundraisers/1234567",
        "html_url": "https://www.betternow.org/dk/firstname-lastnames-fundraiser-for-helpnow"
      }
    ],
    "html_url": "https://www.betternow.org/dk/teams/team-novo",
    "id": 1234567,
    "logo_url": "https://cdn.example.net/logo.png",
    "name": "Team NOVO",
    "slug": "team-novo",
    "updated_at": "2012-01-01T12:00:00Z",
    "url": "https://api.betternow.org/team/1234567"
  }
]
```

### Team List Fundraisers
List all Fundraisers that are members of this Team. Fundraisers will be ordered by the amount of money donated, descending

```
GET /teams/{team_id}/fundraisers
```


#### Curl Example
```bash
$ curl -n -X GET https://api.betternow.org/teams/$TEAM_ID/fundraisers

```


#### Response Example
```
HTTP/1.1 200 OK
```
```json
[
  {
    "activity_score": 987654321,
    "cover_media": {
      "image": {
        "url": "https://cnd.example.net/image.jpg"
      },
      "video": {
        "url": "https://youtu.be/12345"
      }
    },
    "created_at": "2012-01-01T12:00:00Z",
    "description": "<p>This is really, <b>REALLY</b> great</p> <br><br>",
    "donate_url": "https://www.betternow.org/dk/fundraisers/firstname-lastnames-fundraiser/donations/new",
    "donations": {
      "count": 123,
      "total_donated": {
        "cents": 1234500,
        "currency": "EUR"
      },
      "url": "https://api.betternow.org/fundraisers/1234567/donations"
    },
    "end_date": "2012-01-01T12:00:00Z",
    "goal": {
      "cents": 1234500,
      "currency": "EUR"
    },
    "headline": "Firstname Lastname's Fundraiser for HelpNow",
    "html_url": "https://www.betternow.org/dk/firstname-lastnames-fundraiser-for-helpnow",
    "id": 1234567,
    "owner": {
      "avatar_url": "https://cdn.example.net/avatar.jpg",
      "first_name": "Firstname",
      "last_name": "Lastname"
    },
    "recipient": {
      "id": 1234567,
      "name": "HelpNow",
      "url": "https://api.betternow.org/organisations/1234567",
      "html_url": "https://www.betternow.org/dk/helpnow"
    },
    "slug": "firstname-lastnames-fundraiser-for-helpnow",
    "updated_at": "2012-01-01T12:00:00Z",
    "url": "https://api.betternow.org/fundraisers/1234567"
  }
]
```

### Team List Donations
List all donations given via this Team

```
GET /teams/{team_id}/donations
```


#### Curl Example
```bash
$ curl -n -X GET https://api.betternow.org/teams/$TEAM_ID/donations

```


#### Response Example
```
HTTP/1.1 200 OK
```
```json
[
  {
    "amount": {
      "cents": 12345,
      "currency": "EUR"
    },
    "comment": "Wow, what a great idea!",
    "created_at": "2012-01-01T12:00:00Z",
    "id": 1234567,
    "name": "Joes Truck Stop",
    "updated_at": "2012-01-01T12:00:00Z"
  }
]
```

### Team List Projects
List all Projects that team members are fundraising for

```
GET /teams/{team_id}/projects
```


#### Curl Example
```bash
$ curl -n -X GET https://api.betternow.org/teams/$TEAM_ID/projects

```


#### Response Example
```
HTTP/1.1 200 OK
```
```json
[
  {
    "created_at": "2012-01-01T12:00:00Z",
    "description": "We need your money for this <b>GREAT</b> project",
    "donations": {
      "count": 123,
      "total_donated": {
        "cents": 1234500,
        "currency": "EUR"
      },
      "url": "https://api.betternow.org/projects/1234567/donations"
    },
    "donate_url": "https://www.betternow.org/dk/fundraisers/helpnow-indsamling21/donations/new",
    "html_url": "https://www.betternow.org/dk/helpnow-projekt",
    "id": 1234567,
    "fundraisers": [
      {
        "id": 1234567,
        "headline": "Firstname Lastname's Fundraiser for HelpNow",
        "url": "https://api.betternow.org/fundraisers/1234567",
        "html_url": "https://www.betternow.org/dk/firstname-lastnames-fundraiser-for-helpnow"
      }
    ],
    "name": "HelpNows generelle arbejde",
    "new_fundraiser_url": "https://www.betternow.org/dk/projects/helpnow-projekt/fundraisers/new",
    "updated_at": "2012-01-01T12:00:00Z",
    "url": "https://api.betternow.org/projects/1234567"
  }
]
```