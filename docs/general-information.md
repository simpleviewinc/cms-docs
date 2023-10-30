## General Information

- CORS is not enabled, so the API is not accessible via client-side JavaScript.
- An access token is required for each request. Details are provided below.
- Queries use mongodb extended JSON, ([MongoDB Extended JSON documentation](http://docs.mongodb.org/manual/reference/mongodb-extended-json/)), allowing you to query on date, objectid, and even regex using the syntax. This means that you are able to query on any field. 
- Result size for any endpoint is capped at 200kb. Requests performed that would exceed this limit will fail. You can specify which fields will be included in the response using the `options.fields` which can often reduce the result size significantly.
- Requests are limited to 1000ms in query duration, and will fail if they take any longer. If you are hitting this limit, you are likely using a poorly optimized query. Reach out for assistance on this as needed, as optimizing depends on the query and is  beyond the scope of this document.
- URLs are in the form of `https://www.[your domain].com/includes/rest_v2/[model]/[method]/?json=[query]&token=[access token]`. Method is either `find`, `aggregate`, or `count`. Models are documented below.