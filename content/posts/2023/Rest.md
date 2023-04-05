---
title: "REST API Design Rulebook"
date: 2023-04-05T18:25:45+05:45
draft: false
---

The Richardson Maturity Model outlines three levels of REST API maturity that make it RESTful. Level 1 is characterized by the use of URI resources and HTTP methods. Level 2 adds the use of HTTP response codes and HATEOAS. Level 3 adds additional hypermedia controls to further enable discoverability and self-description of the API.

### **CRUD functions**

| HTTP Verb | CRUD           |
| --------- | -------------- |
| POST      | Create         |
| GET       | Read           |
| PUT       | Update/Replace |
| PATCH     | Update/Modify  |
| DELETE    | Delete         |

**_Roy Fielding_**, co-found of HTTP-server project recognized that the web’s scalability was goverened by a set of key constraints.

Client-Server: Separation of concerns is a core theme of client-server constraints.

Uniform-Interface:Interaction between the web’s components-meaning its clients, servers, and network-based intermediaries- depened on the uniformity of their interfaces.

What should be uniform then?

- **identification of resources** : should have a unique identifier
- **Manipulation of resources through representation**: representation itself is the way to interact with the resource. through JSON, HTML
- **self descriptive messages**: to update something, you need its id and what to update. whatever is required, it should be present
- **HATEOAS**: (Hypermedia as the engine of application state) Hypermedia are links and navigational element allowing client to discover and interact with the realted resource. HATEOAS is a key concept that allows for dynamic, self-describing APIs that can evolve over time without breaking clients.

Layered System:Other things between client and servers, such as reverse proxies with nginx, cdns etc

Cache: storing contents somewhere so instead of server sending data, it is the caching server that sends the data.

Stateless :no state of client should be memorized by server.

Code on demand: remote functions(firebase functions) ….

### Identifier Design With URL

RFC 3986 defines the URI Syntax as

```nix
URI = scheme "://" authority "/" path [ "?" query ] [ "#" fragment ]
```

- Forward slash separator (/) must be used to indicate a hierarchical relationship
  `api.canvas.restapi.org/shapes/polygons/quadrilaterals/squares`
- Trailing forward slash(/) shouldn’t be included in urls. Because it adds no semantic value and may cause confusion.

  `api.canvas.net/shapes/ 👎`

  `api.canvas.net/shapes 👍`

- Hyphen should be used to improve the readability of URIs
  `api.canvas.net/blog/ashish-thapa/entry/my-post-is-awesome`
- Don’t use underscore(\_) in urls. because visual cue for hypermedia is underline.
  {{< u >}} bad_underscore_might_obstruct_to_think_url_might_have_spaces.{{</ u >}}
- use lowercase letters for URI paths.
- File extension shouldn’t be included in URLS, instead use `content-type` header.

**URI path** is for resource modelling. Example of well modelled path

```
api.canvas.restapi.org/shapes/polygons/quadrilaterals/squares
```

## Resource Archetypes

**Resource Archetypes** are the design patterns for handling resource modelling. Archetype is the original pattern or model of which all other things are representation or copies.

**Document:** basis of database record . For example: **_Document_** for **Country** collection are: Nepal, Czetch Republic, Germany etc. Documents are generally fixed and don’t change, unlike stores

**Collection :** basis of directory of resources. For example: User can be collection, Country can be collection, Songs can be collection.

**Store:** Model that manages a client resource repository. `/user/{user_id}/blogs` gets the blog of the user_id.

**Controller:** when HTTP verbs like PUT, GET, POST can’t say what action or resource will do, you use controller,

`GET /user/{user}/songs/{playlist_id}`

---

this looks like fetching playlist songs

`GET /user/{user}/songs/{playlist_id}/play` looks like user is trying to play a song

Here play is the controller

---

- For document naming , use **singular nouns**. `/leagues/**seattle**`
- for collection naming, use **plural nous.** `/**leagues**/seattle`
- use verb phrase for controller names. such as _play, register, gather_ etc
- CRUD Function name should not be used in **URIs** such as `GET /deleteUser?id=123` because you already have http verbs for that. better would be `DELETE /user`

### **URI Query Design**

Query Part is always the transparent part of the API.

Cache must not vary their behaviour based on the presence or absence of query in the given URL.

- Query part of url can be used to filter collection or stores
- For Pagination use URL query

### **Interaction Design with HTTP**

**GET**: must be used to retreive a represenation of a resource.

**HEAD**: must be used to retieve response headers. send the same response as GET but only headers.

`curl --head api.something.com/greeting`

**PUT**: for inserting and updating a stored resource

**POST**: is semantically open-ended. it allows a method to take any action, regadless of its repeatability or side effects. that is why post method is considered unsafe . Because it can do anything. might end up doing same thing twice on resubmission.

**DELETE** : to delete a record*.*

**Response status Code**

**201**: resource was successfully created

**202**: async action was started

**204**: for when response body is empty

**301**: moved permanently

**400**: bad request for non specific failure

**401**: unauthorized

**403**: forbidden

**404**: not found

**Metadata design**

_Content-type_ : should be all http response, which suggests the type of data found in message body

_content-length_: it is required. Length of the content

**last-modified:** it is required. timestamp that happened to alter the representational state of resource

**eTag:** for caches to be more efficient and save bandwidth, as a webserver doesn’t need to resend a full response if the content was not changed.

**if-match :** this makes the request conditional. If resource matches one of the ETag Values then only it will return the requested resources.

**cache-control:** reduce client perceived latency, to increase reliabilty, and to reduce load on API servers. it takes **max-age** value.

`cache-control: max-age=60, must-revalidate`

`cache-control: no-cache` , if no caching is required. and then `expires: 0`

**Media type**

**`type "/" subtype *( ";" parameter )`**

type might be application, audio, image, message, model, multipart, text or video

There are vendor specific media types `application/vnd.ms-excel`
