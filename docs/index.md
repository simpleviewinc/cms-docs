# Simpleview CMS Rest API Documentation

The REST API (v2) can be used to query your website's backend database. This document is intended as a basic introduction, and an overview of some of the available enpoints, but is not complete or exhaustive.

- [General Information](#general-information)
- [Generating an Access Token](#generating-an-access-token)
- [Request Format](#request-format)
- [Request/Response Options](#request-response-options)
- [Listings Model](#listings-model)
  - [Listings Fields](#listings-fields)
  - [Example Listing Queries](#example-listing-queries)
  - [Example Listings Docs](#example-listings-docs)
  - [Listing Meta Fields](#listing-meta-fields)
  - [Example Listing Meta data](#example-listing-meta-data)
- [Events Model](#events-model)
  - [Events Fields](#events-fields)
  - [Example Event Data](#example-event-data)
  - [Example Event Queries](#example-event-queries)
  - [Event Meta Fields](#event-meta-fields)
  - [Example event meta data](#example-event-meta-data)
- [Offers Model](#offers-model)
  - [Offer Fields](#offer-fields)
  - [Example Offer Data](#example-offer-data)
  - [Offer Meta Fields](#offer-meta-fields)
  - [Example offers meta data](#example-offers-meta-data)
- [Nav Model](#nav-model)
  - [Nav Item Fields](#nav-item-fields)
  - [Example Nav Endpoint URLs](#example-nav-endpoint-urls)
  - [Example Nav Item Data](#example-nav-item-data)
- [Blog Post Model](#blog-post-model)
  - [Blog Post Fields](#blog-post-fields)
  - [Example Blog Post Endpoint URLs](#example-blog-post-endpoint-urls)
  - [Example Blog Post Data](#example-blog-post-data)
  

## General Information:

- CORS is not enabled, so the API is not accessible via client-side JavaScript.
- An access token is required for each request. Details are provided below.
- Queries use mongodb extended JSON, ([MongoDB Extended JSON documentation](http://docs.mongodb.org/manual/reference/mongodb-extended-json/)), allowing you to query on date, objectid, and even regex using the syntax. This means that you are able to query on any field. 
- Result size for any endpoint is capped at 200kb. Requests performed that would exceed this limit will fail. You can specify which fields will be included in the response using the `options.fields` which can often reduce the result size significantly.
- Requests are limited to 1000ms in query duration, and will fail if they take any longer. If you are hitting this limit, you are likely using a poorly optimized query. Reach out for assistance on this as needed, as optimizing depends on the query and is  beyond the scope of this document.
- URLs are in the form of `https://www.[your domain].com/includes/rest_v2/[model]/[method]/?json=[query]&token=[access token]`. Method is either `find`, `aggregate`, or `count`. Models are documented below.
