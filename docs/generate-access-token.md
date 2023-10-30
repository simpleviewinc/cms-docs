## Generating an Access Token

Each request to the API must include a generated token as a URL parameter. An access token is a 32 character string of random letters and digits, which you'll procure from this URL:

```
https://www.[your domain].com/plugins/core/get_simple_token/
```

Example:
```
https://[client_identifier].simpleviewinc.com/plugins/core/get_simple_token/
```

Tokens are valid for approximately 2 days, but it's recommended you grab a new one at least once per day. When a request is made with an expired or incorrect token, the API will respond with "Invalid credentials" and a 403 status code.