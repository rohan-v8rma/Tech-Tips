# INDEX

- [INDEX](#index)
- [API Endpoint](#api-endpoint)
- [Routes of API](#routes-of-api)
- [API payload](#api-payload)
- [RESTful APIs](#restful-apis)
  - [Example of a RESTful API](#example-of-a-restful-api)
  - [Uses of REST APIs](#uses-of-rest-apis)
- [API request: HTTP response codes](#api-request-http-response-codes)
- [JSON Data](#json-data)
  - [Use a consistent naming convention](#use-a-consistent-naming-convention)
  - [Keep it simple](#keep-it-simple)
  - [Use arrays for lists](#use-arrays-for-lists)
  - [Use objects for nested data](#use-objects-for-nested-data)
  - [Use a single root element](#use-a-single-root-element)
  - [Use meaningful keys](#use-meaningful-keys)
- [`cURL` command explained](#curl-command-explained)

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

---

# API request: HTTP response codes

HTTP status codes are generated by the server that's hosting the site when it responds to a request made by a client, for example a browser or a crawler. 

Every HTTP status code has a different meaning, but often the outcome of the request is the same. For example, there are multiple status codes that signal redirection, but their outcome is the same.

Resp


# JSON Data


When structuring JSON data, it's important to keep in mind that JSON is a hierarchical format, meaning that it has a tree-like structure with nested elements. Here are a few guidelines for structuring JSON data:

## Use a consistent naming convention

Choose a naming convention and stick to it throughout the JSON data. This will make it easier to understand and navigate the data.

## Keep it simple

Avoid complex nesting and use only the necessary elements to make the data easy to read and understand.

## Use arrays for lists

Use arrays to represent lists of items, and use objects to represent individual items within those lists.

## Use objects for nested data

Use objects to represent nested data, such as an object containing multiple properties.

## Use a single root element

Use a single root element to contain all the data, making it easy to identify and access the data.

## Use meaningful keys

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
