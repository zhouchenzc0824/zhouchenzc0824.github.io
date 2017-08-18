---
layout: post
title: "How to design web services"
date: 2017-6-3 021:10:11.000000000 -06:00
tags: [WebServices]
---

>It is ubiquitous that all the modern applications no matter what architecture is, Microservices or Monolithic, are all using web services for data transfer and communication between endpoints due to convenience and lightweight-ness.

When it comes to web services, a lot people will say REST and SOAP are two most common types of web services that are used in nowadays. But what I want to say is that these two concepts REST and SOAP are not comparable. 

First of all, the thing we need to know is SOAP is a protocol and REST is an architectural style. In this case, REST which is focused on resources should be compared against SOA and PRC which are messages-focused and operations-focused respectively.
Software folks are in favor of REST than SOAP since most of people said rest is easier to design and consume. SOAP is using XML format for all messages while REST is using smaller format JSON, the design philosophy of REST APIs is closer to other web technologies. Just name some.

Now introduce ```Richardson Maturity Modle``` a model developed by Leonard Richardson that will turn the REST API design principal into 4 levels.

```Level 0``` 
- use HTTP as transport system for remote interactions without using any other web mechanisms.(use protocol for remote interactions)
 
```Level 1``` 
- tackles the question of handling complexity by using divide and conquer, breaking a large service endpoint down into multiple resources.(be able to distinguish different resources)
 
```Level 2```
- introduces a standard set of verbs so that we handle similar situations in the same way, removing unnecessary variation. (use HTTP verbs, but use other protocol verbs can still be Restful)
 
```Level 3```
- introduces discoverability, providing a way of making a protocol more self-documenting with number of hypermedia controls for different things to do next. (HATEOAS: clients interact with application entirely through hypermedia provided by application servers)

So the tips for designing a fine grained RESTFUL APIs:

- Use API versioning: add a version as prefix to URLs
```GET www.services.com/v1/posts```
In this case add v1 as prefix

- Cross-origin resource sharing(CORS)
When placing the API into a different server, some fonts, XMLHttpRequest or fetch may not be for the different domain requests.

- Always use plurals as URIs.

- Avoid unnecessary query-string in URI which is usually for filtering.

- Good idea to respond an updated object with request ```PUT```, ```PATCH```, and ```POST``` instead of null.

- Envelop data to avoid potential vulnerability and potential hacks.

- Always return a non-empty object since some language evaluate empty object as true like JavaScript.

- Always use HTTP status and Error responses.

It is not saying that you must have to follow all these steps to design you REST APIs, but these are the recommended way to follow based on previous experience from many software engineers.


![happy](/assets/gp/happy.gif)












