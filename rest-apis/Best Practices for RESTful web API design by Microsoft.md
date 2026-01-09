# Best Practices for RESTful web API design by [Microsoft](https://learn.microsoft.com/en-us/azure/architecture/best-practices/api-design)

## Table of Contents

- [Core Concepts Behind RESTful APIs](#core-concepts-behind-restful-apis)
- [Resource URI Naming Conventions](#resource-uri-naming-conventions)
- [HTTP request methods](#http-request-methods)
- [Implement asynchronous methods](#implement-asynchronous-methods)
- [Implement HATEOAS](#implement-hateoas)
- [Enable distributed tracing and trace context in APIs](#enable-distributed-tracing-and-trace-context-in-apis)

A RESTful Web API is a Web API that employs Representational State Transfer architectural principals and conventions. It is nothing more then well accepted principles and values that form platform independent and loosely coupled solution.

# Core Concepts Behind RESTful APIs

## URI (Uniform Resource Identifier)

The URI is a unique representation of a resource. RESTful APIs URIs are meant to point to resource rather than to the action (we use HTTP methods for it). Therefore URI is a pointer to a specific resource (object, data, service, etc.)

For example:

```jsx
https://api.contoso.com/orders/1  // Points to order with id 1
https://api.contoso.com/companies/1/departments // Points to all departments of company 1
```

## Resource Representation

The resources that are represented by the URI are being encoded and transported through HTTP protocol using specific format such as JSON or XML. Clients suppose to make a request to URI to get specific resource.

## **Uniform Interface**

Uniform interface in terms of the URIs structure and used HTTP methods.

## **Stateless Request Model**

Meaning that backend doesn’t store any client session on its end. Each request is being authenticated, served and forgotten.

## Hypermedia Links

It is considered a good practice to include related-to-resource links with the response, so that users can navigate from one resource to another faster.

# **Resource URI Naming Conventions**

When you design a RESTful web API, it's important that you use the correct naming and relationship conventions for resources:

## **Use nouns for resource names**

Use plural form of the resource in an URI, for example, `/orders`  or `/tasks` 

## **Consider the relationships between different types of resources and how you might expose these associations**

For example, `/customers/1/orders`  or `customers/1/orders/99` . But be cautions here because in general it is no considered a good practice to have very nested URIs. Usually a good limit will be 2 levels of nesting path as shown above. Otherwise think about making the resource as a first-class resource wit its own path in addition with hyperlinks that will enable user to easily navigate from one resource to another. 
From my own experience, I tend to use nested URIs in cases when there is a strong parent-child relationship between resources and child resource won’t make sense without parent resource or when the parent and child resources are in aggregate relationship, which makes it natural to have as a nested path. Additionally, with deeply nested URI the endpoint looses its flexibility, for example, consider that we only have `/customer/1/orders/99/products` , the URI explicitly identifies that this is a URI only for customer 1’s order 99’s products, while there surely going to be a use case when we want to list all products. For that we would need `/products`  endpoints collection.

## **Avoid creating APIs that mirror the internal structure of a database**

The purpose of REST is to model business entities and the operations that an application can perform on those entities. A client shouldn't be exposed to the internal implementation. For example, if your data is stored in a relational database, the web API doesn't need to expose each table as a collection of resources.

# **HTTP request methods**

Below table gives an example RESTful API design for customer endpoints. Interestingly it uses PUT HTTP method for bulk updates for customer

    

| **Resource** | **POST** | **GET** | **PUT** | **DELETE** |
| --- | --- | --- | --- | --- |
| /customers | Create a new customer | Retrieve all customers | Bulk update of customers | Remove all customers |
| /customers/1 | Error | Retrieve the details for customer 1 | Update the details of customer 1 if it exists | Remove customer 1 |
| /customers/1/orders | Create a new order for customer 1 | Retrieve all orders for customer 1 | Bulk update of orders for customer 1 | Remove all orders for customer 1 |

<aside>
🖊️

PUT requests must be `*idempotent*`, which means that submitting the same request multiple times always results in the same resource being modified with the same values. If a client resends a PUT request, the results should remain unchanged. In contrast, POST and PATCH requests aren't guaranteed to be idempotent.

</aside>

<aside>
🖊️

It is considered a well-defined practice to use `null`  as a field value in PATCH request to remove field from the resource.

</aside>

# **Implement asynchronous methods**

In some case the APIs may take time to satisfy the request. An sometimes it is not suitable to wait for the response of the request. In such cases we implement the endpoint in a way to implement the request asynchronously and immediately return HTTP status 202 (Accepted) to identify that the request is accepted.

<aside>
🖊️

The **`Location`** header is used in HTTP responses to tell the client where to go next. It provides a URL that the client should redirect to or use to locate a newly created resource.

</aside>

# **Implement HATEOAS**

One of the primary reasons behind the RESTful APIs architecture is the ability to navigate and access resources without prior knowledge of URI schema. Each GET request’s response should not only include the resource but also the links to directly related resources in a specific format that describes the operations available on each of these resources. This principle is known as HATEOAS, or Hypertext as the Engine of Application State. 

For example:

```jsx
{
  "orderID":3,
  "productID":2,
  "quantity":4,
  "orderValue":16.60,
  "links":[
    {
      "rel":"customer",
      "href":"https://api.contoso.com/customers/3",
      "action":"GET",
      "types":["text/xml","application/json"]
    },
    {
      "rel":"customer",
      "href":"https://api.contoso.com/customers/3",
      "action":"PUT",
      "types":["application/x-www-form-urlencoded"]
    },
    {
      "rel":"customer",
      "href":"https://api.contoso.com/customers/3",
      "action":"DELETE",
      "types":[]
    },
    {
      "rel":"self",
      "href":"https://api.contoso.com/orders/3",
      "action":"GET",
      "types":["text/xml","application/json"]
    },
    {
      "rel":"self",
      "href":"https://api.contoso.com/orders/3",
      "action":"PUT",
      "types":["application/x-www-form-urlencoded"]
    },
    {
      "rel":"self",
      "href":"https://api.contoso.com/orders/3",
      "action":"DELETE",
      "types":[]
    }]
}
```

# **Enable distributed tracing and trace context in APIs**

As distributed systems and microservice architectures become the standard, the complexity of modern architectures increases. Using headers, such as `Correlation-ID`, `X-Request-ID`, or `X-Trace-ID`, to propagate trace context in API requests is a best practice to achieve end-to-end visibility. This approach enables tracking of requests as they flow from the client to back-end services. It facilitates rapid identification of failures, monitors latency, and maps API dependencies across services.
