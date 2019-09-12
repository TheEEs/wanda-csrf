[![Build Status](https://travis-ci.org/TheEEs/wanda-csrf.svg?branch=master)](https://travis-ci.org/TheEEs/wanda-csrf)
[![GitHub release](https://img.shields.io/github/release/TheEEs/wanda-csrf.svg)](https://github.com/TheEEs/wanda-csrf/releases)
# wanda-csrf

Adds CSRF protection to your [Wanda](https://github.com/TheEEs/wanda) application.

Requires a session middleware to be initialized first.

## Installation


Add this to your application's `shard.yml`:

```yaml
dependencies:
  wanda-csrf:
    github: TheEEs/wanda-csrf
```


## Usage

Basic Use
```crystal
require "wanda-csrf"

add_handler CSRF.new
```

You can also change the name of the form field, header name, the methods which don't need csrf,error message and routes which you don't want csrf to apply.
All of these are optional
```crystal
require "wanda-csrf"

add_handler CSRF.new(
  header: "X_CSRF_TOKEN",
  allowed_methods: ["GET", "HEAD", "OPTIONS", "TRACE"],
  allowed_routes: ["/api/somecallback"],
  parameter_name: "_csrf", 
  error: "CSRF Error" 
)
```

If you need to have some logic within your error response, you can also pass it a proc (a pointer to a function)


```crystal
require "wanda-csrf"

add_handler CSRF.new(
  header: "X_CSRF_TOKEN",
  allowed_methods: ["GET", "HEAD", "OPTIONS", "TRACE"],
  allowed_routes: ["/api/somecallback"],
  parameter_name: "_csrf", 
  error: ->myerrorhandler(HTTP::Server::Context)
)

def myerrorhandler(env)
  if env.request.headers["Content-Type"]? == "application/json"
    {"error" => "csrf error"}.to_json
  else
    "<html><head><title>Error</title><body><h1>You cannot post to this route without a valid csrf token</h1></body></html>"
  end
end
```
