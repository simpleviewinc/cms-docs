## Request Format

Each request needs to include 2 URL parameters, `token` and `json`. The `json` parameter represents the main request payload, follows [mongolayer syntax](https://github.com/simpleviewinc/mongolayer), and will generally include a `filter` (or pipeline if using the aggregate endpoint) and `options`.

**Example request URL:**

This is the most basic of requests, getting a single listing by it's `recid` (unique record ID) and returning its `title` field.

```
https://[client_identifier].simpleviewinc.comincludes/rest_v2/plugins_listings_listings/find/?json=%7B%22filter%22%3A%7B%22recid%22%3A%2041750%7D,%20%22options%22%3A%7B%22limit%22%3A%201,%20%22fields%22%3A%7B%22title%22%3A1%7D%7D%7D&token=f19942375ec3b67599c82197385c8e19
```

Obviously these URLs aren't very readable once they're encoded, so here's the `json` parameter from the above request, prior to URL encoding and reformatted as readable JSON data:

```json
{
  "filter" : {
    "recid": 41750
  },
  "options" : {
	"limit" : 1,
    "fields" : {
      "title" : true
    }
  }
}
```

Example Response:

```json
{
  "docs": [
    {
      "_id": "62c604e4df86489a35c2e48d",
      "title": "Magpies Gourmet Pizza, Inc.",
      "id": "62c604e4df86489a35c2e48d",
      "isListing": true,
      "hasTripAdvisor": false,
      "hasYelp": false
    }
  ]
}
```

There are a couple things worth noting here. A successful API response will always be a JSON formatted string. The `find` endpoints always return a `docs` field, which contains an array of the matching docs (records). You can see in the response that the `title` field is included, as indicated in the `options.fields` section of the request. But there are also some additional fields. These are "virtual fields" created when the response docs are cast as objects according to the data model. In some cases, where you're requesting a large amount of data, it may be desirable or necessary (due to response size restrictions) to avoid having these virtual fields included in the response. You can do so by specifying `options.castDocs` as `false`.

Here's the same request, with the exception of telling the API not to cast with the data model, and thus not including virtual fields.

Full URL:
```
https://[client_identifier].simpleviewinc.com/includes/rest_v2/plugins_listings_listings/find/?json={%22filter%22:{%22recid%22:999},%20%22options%22:{%22castDocs%22:false,%22fields%22:{%22title%22:true}}}&token=6db0fd820e20e7295515f5657dd75e6f
```

JSON data before URL encoding:

```json
{
  "filter" : {
    "recid": 41750
  },
  "options" : {
    "limit" : 1,
    "castDocs" : false,
    "fields" : {
      "title" : true
    }
  }
}
```

API Response:
```json
{
  "docs": [
    {
      "title": "Magpies Gourmet Pizza, Inc."
    }
  ]
}
```

So by including `"castDocs" : false`, we *only* get the field(s) specified in `options.fields`.
