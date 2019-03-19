## Principles of good RESTful API Design

Good API design is hard! An API represents a contract between you and those who Consume your data. Breaking this contract will result in many angry emails, and a slew of sad users with mobile apps which no longer work. Documentation is half the battle, and it is very difficult to find programmer who also likes to write.

Building an API is one of the most important things you can do to increase the value of your service. By having an API, your service / core application has the potential to become a platform from which other services grow. Look at the current huge tech companies: Facebook, Twitter, Google, GitHub, Amazon, Netflix… None of them would be nearly as big as they are today if they hadn’t opened up their data via API. In fact, an entire industry exists with the sole purpose of consuming data provided by said platforms.  

    The easier your API is to consume, the more people that will consume it.

The principles of this document, if followed closely when designing your API, will ensure that Consumers of your API will be able to understand what is going on, and should drastically reduce the number of confused and/or angry emails you receive. I’ve organized everything into topics, which don’t necessarily need to be read in order.

### Definitions
Here’s a few of the important terms I will use throughout the course of this document:

* **Resource**: A single instance of an object. For example, an animal.
* **Collection**: A collection of homogeneous objects. For example, animals.
* **HTTP**: A protocol for communicating over a network.
* **Consumer**: A client computer application capable of making HTTP requests.
* **Third Party Developer**: A developer not a part of your project but who wishes to consume your data.
* **Server**: An HTTP server/application accessible from a Consumer over a network.
* **Endpoint**: An API URL on a Server which represents either a Resource or an entire Collection.
* **Idempotent**: Side-effect free, can happen multiple times without penalty.
* **URL Segment**: A slash-separated piece of information in the URL.



Data Design and Abstraction

数据设计与抽象

Planning how your API will look begins earlier than you’d think; first you need to decide how your data will be designed and how your core service / application will work. If you’re doing API First Development this should be easy. If you’re attaching an API to an existing project, you may need to provide more abstraction.

Occasionally, a Collection can represent a database table, and a Resource can represent a row within that table. However, this is not the usual case. In fact, your API should abstract away as much of your data and business logic as possible. It is very important that you don’t overwhelm Third-Party Developers with any complex application data, if you do they won’t want to use your API.

There are also many parts of your service which you SHOULD NOT expose via API at all. A common example is that many APIs will not allow third parties to create users.

规划好你的API的外观要先于开发它实际的功能。首先你要知道数据该如何设计和核心服务/应用程序会如何工作。如果你纯粹新开发一个API，这样会比较容易一些。但如果你是往已有的项目中增加API，你可能需要提供更多的抽象。

有时候一个集合可以表达一个数据库表，而一个资源可以表达成里面的一行记录，但是这并不是常态。事实上，你的API应该尽可能通过抽象来分离数据与业务逻辑。这点非常重要，只有这样做你才不会打击到那些拥有复杂业务的第三方开发者，否则他们是不会使用你的API的。

当然你的服务可能很多部分是不应该通过API暴露出去的。比较常见的例子就是很多API是不允许第三方来创建用户的。

Verbs

动词

Surely you know about GET and POST requests. These are the two most commonly requests used when your browser visits different webpages. The term POST is so popular that it has even invaded common language, where people who know nothing about how the Internet works do know they can “post” something on a friends Facebook wall.

There are four and a half very important HTTP verbs that you need to know about. I say “and a half”, because the PATCH verb is very similar to the PUT verb, and two two are often combined by many an API developer. Here are the verbs, and next to them are their associated database call (I’m assuming most people reading this know more about writing to a database than designing an API).

GET (SELECT): Retrieve a specific Resource from the Server, or a listing of Resources.
POST (CREATE): Create a new Resource on the Server.
PUT (UPDATE): Update a Resource on the Server, providing the entire Resource.
PATCH (UPDATE): Update a Resource on the Server, providing only changed attributes.
DELETE (DELETE): Remove a Resource from the Server.
Here are two lesser known HTTP verbs:

HEAD – Retrieve meta data about a Resource, such as a hash of the data or when it was last updated.
OPTIONS – Retrieve information about what the Consumer is allowed to do with the Resource.
A good RESTful API will make use of the four and a half HTTP verbs for allowing third parties to interact with its data, and will never include actions / verbs as URL segments.

Typically, GET requests can be cached (and often are!) Browsers, for example, will cache GET requests (depending on cache headers), and will go as far as prompt the user if they attempt to POST for a second time. A HEAD request is basically a GET without the response body, and can be cached as well.

显然你了解GET和POST请求。当你用浏览器去访问不同页面的时候，这两个是最常见的请求。POST术语如此流行以至于开始侵扰通俗用语。即使是那些不知道互联网如何工作的人们也能“post”一些东西到朋友的Facebook墙上。

这里至少有四个半非常重要的HTTP动词需要你知道。我之所以说“半个”的意思是PATCH这个动词非常类似于PUT，并且它们俩也常常被开发者绑定到同一个API上。

GET (选择)：从服务器上获取一个具体的资源或者一个资源列表。
POST （创建）： 在服务器上创建一个新的资源。
PUT （更新）：以整体的方式更新服务器上的一个资源。
PATCH （更新）：只更新服务器上一个资源的一个属性。
DELETE （删除）：删除服务器上的一个资源。
还有两个不常用的HTTP动词：

HEAD ： 获取一个资源的元数据，如数据的哈希值或最后的更新时间。
OPTIONS：获取客户端能对资源做什么操作的信息。
一个好的RESTful API只允许第三方调用者使用这四个半HTTP动词进行数据交互，并且在URL段里面不出现任何其他的动词。

一般来说，GET请求可以被浏览器缓存（通常也是这样的）。例如，缓存请求头用于第二次用户的POST请求。HEAD请求是基于一个无响应体的GET请求，并且也可以被缓存的。

Versioning

版本化

No matter what you are building, no matter how much planning you do beforehand, your core application is going to change, your data relationships will change, attributes will invariably be added and removed from your Resources. This is just how software development works, and is especially true if your project is alive and used by many people (which is likely the case if you’re building an API).

Remember than an API is a published contract between a Server and a Consumer. If you make changes to the Servers API and these changes break backwards compatibility, you will break things for your Consumer and they will resent you for it. Do it enough, and they will leave. To ensure your application evolves AND you keep your Consumers happy, you need to occasionally introduce new versions of the API while still allowing old versions to be accessible.

As a side note, if you are simply ADDING new features to your API, such as new attributes on a Resource (which are not required and the Resource will function without), or if you are ADDING new Endpoints, you do not need to increment your API version number since these changes do not break backwards compatibility. You will want to update your API Documentation (your Contract), of course.

Over time you can deprecate old versions of the API. To deprecate a feature doesn’t mean to shut if off or diminish the quality of it, but to tell Consumers of your API that the older version will be removed on a specific date and that they should upgrade to a newer version.

A good RESTful API will keep track of the version in the URL. The other most common solution is to put a version number in a request header, but after working with many different Third Party Developers, I can tell you that adding headers is no where near as easy as adding a URL Segment.

无论你正在构建什么，无论你在入手前做了多少计划，你核心的应用总会发生变化，数据关系也会变化，资源上的属性也会被增加或删除。只要你的项目还活着，并且有大量的用户在用，这种情况总是会发生。

请谨记一点，API是服务器与客户端之间的一个公共契约。如果你对服务器上的API做了一个更改，并且这些更改无法向后兼容，那么你就打破了这个契约，客户端又会要求你重新支持它。为了避免这样的事情，你既要确保应用程序逐步的演变，又要让客户端满意。那么你必须在引入新版本API的同时保持旧版本API仍然可用。

注：如果你只是简单的增加一个新的特性到API上，如资源上的一个新属性或者增加一个新的端点，你不需要增加API的版本。因为这些并不会造成向后兼容性的问题，你只需要修改文档即可。

随着时间的推移，你可能声明不再支持某些旧版本的API。申明不支持一个特性并不意味着关闭或者破坏它。而是告诉客户端旧版本的API将在某个特定的时间被删除，并且建议他们使用新版本的API。

 一个好的RESTful API会在URL中包含版本信息。另一种比较常见的方案是在请求头里面保持版本信息。但是跟很多不同的第三方开发者一起工作后，我可以很明确的告诉你，在请求头里面包含版本信息远没有放在URL里面来的容易。

Analytics

分析

Keep track of the version/endpoints of your API being used by Consumers. This can be as simple as incrementing an integer in a database each time a request is made. There are many reasons that keeping track of API Analytics is a good idea, for example, the most commonly used API calls should be made efficient.

For the purposes of building an API which Third Party Developers will love, the most important thing is that when you do deprecate a version of your API, you can actually contact developers using deprecated API features. This is the perfect way to remind them to upgrade before you kill the old API version.

The process of Third Party Developer notification can be automated, e.g. mail the developer every time 10,000 requests to a deprecated feature are made.

所谓API分析就是持续跟踪那些正为人使用的API的版本和端点信息。而这可能就跟每次请求都往数据库增加一个整数那样简单。有很多的原因显示API跟踪分析是一个好主意，例如，对那些使用最广泛的API来说效率是最重要的。

第三方开发者通常会关注API的构建目的，其中最重要的一个目的是你决定什么时候不再支持某个版本。你需要明确的告知开发者他们正在使用那些即将被移除的API特性。这是一个很好的方式在你准备删除旧的API之前去提醒他们进行升级。

当然第三方开发者的通知流程可以以某种条件被自动触发，例如每当一个过时的特性上发生10000次请求时就发邮件通知开发者。

API Root URL

API根URL

The root location of your API is important, believe it or not. When a developer (read as code archaeologist) inherits an old project using your API and needs to build new features, they may not know about your service at all. Perhaps all they know is a list of URLs which the Consumer calls out to. It’s important that the root entry point into your API is as simple as possible, as a long complex URL will appear daunting and can turn developers away.

Here are two common URL Roots:

https://example.org/api/v1/*
https://api.example.com/v1/*
If your application is huge, or you anticipate it becoming huge, putting the API on its own subdomain (e.g. api.) is a good choice. This can allow for some more flexible scalability down the road.

https://example.org/api/v1/*
https://api.example.com/v1/*
If you anticipate your API will never grow to be that large, or you want a much simpler application setup (e.g. you want to host the website AND API from the same framework), placing your API beneath a URL segment at the root of the domain (e.g. /api/) works as well.

It’s a good idea to have content at the root of your API. Hitting the root of GitHub’s API returns a listing of endpoints, for example. Personally, I’m a fan of having the root URL give information which a lost developer would find useful, e.g., how to get to the developer documentation for the API.

Also, notice the HTTPS prefix. As a good RESTful API, you must host your API behind HTTPS.

无论你信不信，API的根地址很重要。当一个开发者接手了一个旧项目（如进行代码考古时）。而这个项目正在使用你的API，同时开发者还想构建一个新的特性，但他们完全不知道你的服务。幸运的是他们知道客户端对外调用的那些URL列表。让你的API根入口点保持尽可能的简单是很重要的，因为开发者很可能一看到那些冗长而又复杂的URL就转身而走。

这里有两个常见的URL根例子：

https://example.org/api/v1/*
https://api.example.com/v1/*
如果你的应用很庞大或者你预期它将会变的很庞大，那么将API放到子域下通常是一个好选择。这种做法可以保持某些规模化上的灵活性。

 但如果你觉得你的API不会变的很庞大，或是你只是想让应用安装更简单些（如你想用相同的框架来支持站点和API），将你的API放到根域名下也是可以的。

让API根拥有一些内容通常也是个好主意。Github的API根就是一个典型的例子。从个人角度来说我是一个通过根URL发布信息的粉丝，这对很多人来说是有用的，例如如何获取API相关的开发文档。

同样也请注意HTTPS前缀，一个好的RESTful API总是基于HTTPS来发布的。

Endpoints

端点

An Endpoint is a URL wi个thin your API which points to a specific Resource or a Collection of Resources.

If you were building a fictional API to represent several different Zoo’s, each containing many Animals (with an animal belonging to exactly one Zoo), employees (who can work at multiple zoos) and keeping track of the species of each animal, you might have the following endpoints:

https://api.example.com/v1/zoos
https://api.example.com/v1/animals
https://api.example.com/v1/animal_types
https://api.example.com/v1/employees
When referring to what each endpoint can do, you’ll want to list valid HTTP Verb and Endpoint combinations. For example, here’s a semi-comprehensive list of actions one can perform with our fictional API. Notice that I’ve preceded each endpoint with the HTTP Verb, as this is the same notation used within an HTTP Request header.

GET /zoos: List all Zoos (ID and Name, not too much detail)
POST /zoos: Create a new Zoo
GET /zoos/ZID: Retrieve an entire Zoo object
PUT /zoos/ZID: Update a Zoo (entire object)
PATCH /zoos/ZID: Update a Zoo (partial object)
DELETE /zoos/ZID: Delete a Zoo
GET /zoos/ZID/animals: Retrieve a listing of Animals (ID and Name).
GET /animals: List all Animals (ID and Name).
POST /animals: Create a new Animal
GET /animals/AID: Retrieve an Animal object
PUT /animals/AID: Update an Animal (entire object)
PATCH /animals/AID: Update an Animal (partial object)
GET /animal_types: Retrieve a listing (ID and Name) of all Animal Types
GET /animal_types/ATID: Retrieve an entire Animal Type object
GET /employees: Retrieve an entire list of Employees
GET /employees/EID: Retreive a specific Employee
GET /zoos/ZID/employees: Retrieve a listing of Employees (ID and Name) who work at this Zoo
POST /employees: Create a new Employee
POST /zoos/ZID/employees: Hire an Employee at a specific Zoo
DELETE /zoos/ZID/employees/EID: Fire an Employee from a specific Zoo
In the above list, ZID means Zoo ID, AID means Animal ID, EID means Employee ID, and ATID means Animal Type ID. Having a key in your documentation for whatever convention you choose is a good idea.

I’ve left out the common API URL prefix in the above examples for brevity. While this can be fine during communications, in your actual API documentation, you should always display the full URL to each endpoint (e.g. GET http://api.example.com/v1/animal_type/ATID).

Notice how the relationships between data is displayed, specifically the many to many relationships between employees and zoos. By adding an additional URL segment, one can perform more specific interactions. Of course there is no HTTP verb for “FIRE”-ing an employee, but by performing a DELETE on an Employee located within a Zoo, we’re able to achieve the same effect.

一个端点就是指向特定资源或资源集合的URL。

如果你正在构建一个虚构的API来展现几个不同的动物园，每一个动物园又包含很多动物，员工和每个动物的物种，你可能会有如下的端点信息：

https://api.example.com/v1/zoos
https://api.example.com/v1/animals
https://api.example.com/v1/animal_types
https://api.example.com/v1/employees
针对每一个端点来说，你可能想列出所有可行的HTTP动词和端点的组合。如下所示，请注意我把HTTP动词都放在了虚构的API之前，正如将同样的注解放在每一个HTTP请求头里一样。（下面的URL就不翻译了，我觉得没啥必要翻)

GET /zoos: List all Zoos (ID and Name, not too much detail)
POST /zoos: Create a new Zoo
GET /zoos/ZID: Retrieve an entire Zoo object
PUT /zoos/ZID: Update a Zoo (entire object)
PATCH /zoos/ZID: Update a Zoo (partial object)
DELETE /zoos/ZID: Delete a Zoo
GET /zoos/ZID/animals: Retrieve a listing of Animals (ID and Name).
GET /animals: List all Animals (ID and Name).
POST /animals: Create a new Animal
GET /animals/AID: Retrieve an Animal object
PUT /animals/AID: Update an Animal (entire object)
PATCH /animals/AID: Update an Animal (partial object)
GET /animal_types: Retrieve a listing (ID and Name) of all Animal Types
GET /animal_types/ATID: Retrieve an entire Animal Type object
GET /employees: Retrieve an entire list of Employees
GET /employees/EID: Retreive a specific Employee
GET /zoos/ZID/employees: Retrieve a listing of Employees (ID and Name) who work at this Zoo
POST /employees: Create a new Employee
POST /zoos/ZID/employees: Hire an Employee at a specific Zoo
DELETE /zoos/ZID/employees/EID: Fire an Employee from a specific Zoo
在上面的列表里，ZID表示动物园的ID， AID表示动物的ID，EID表示雇员的ID，还有ATID表示物种的ID。让文档里所有的东西都有一个关键字是一个好主意。

为了简洁起见，我已经省略了所有API共有的URL前缀。作为沟通方式这没什么问题，但是如果你真要写到API文档中，那就必须包含完整的路径（如，GET http://api.example.com/v1/animal_type/ATID）。

请注意如何展示数据之间的关系，特别是雇员与动物园之间的多对多关系。通过添加一个额外的URL段就可以实现更多的交互能力。当然没有一个HTTP动词能表示正在解雇一个人，但是你可以使用DELETE一个动物园里的雇员来达到相同的效果。

Filtering

过滤器

When a Consumer makes a request for a listing of objects, it is important that you give them a list of every single object matching the requested criteria. This list could be massive. But, it is important that you don’t perform any arbitrary limitations of the data. It is these arbitrary limits which make it hard for a third party developer to know what is going on. If they request a certain Collection, and iterate over the results, and they never see more than 100 items, it is now their job to figure out where this limit is coming from. Is their ORM buggy and limiting items to 100? Is the network chopping up large packets?

Minimize the arbitrary limits imposed on Third Party Developers.

It is important, however, that you do offer the ability for a Consumer to specify some sort of filtering/limitation of the results. The most important reason for this is that the network activity is minimal and the Consumer gets their results back as soon as possible. The second most important reason for this is the Consumer may be lazy, and if the Server can do filtering and pagination for them, all the better. The not-so-important reason (from the Consumers perspective), yet a great benefit for the Server, is that the request will be less resource heavy.

Filtering is mostly useful for performing GETs on Collections of resources. Since these are GET requests, filtering information should be passed via the URL. Here are some examples of the types of filtering you could conceivably add to your API:

?limit=10: Reduce the number of results returned to the Consumer (for Pagination)
?offset=10: Send sets of information to the Consumer (for Pagination)
?animal_type_id=1: Filter records which match the following condition (WHERE animal_type_id = 1)
?sortby=name&order=asc: Sort the results based on the specified attribute (ORDER BY name ASC)
Some of these filterings can be redundant with endpoint URLS. For example I previously mentioned GET /zoo/ZID/animals. This would be the same thing as GET /animals?zoo_id=ZID. Dedicated endpoints being made available to the Consumer will make their lives easier, this is especially true with requests you anticipate they will make a lot. In the documentation, mention this redundancy so that Third Party Developers aren’t left wondering if differences exist.

Also, this goes without saying, but whenever you perform filtering or sorting of data, make sure you white-list the columns for which the Consumer can filter and sort by. We don’t want any database errors being sent to Consumers!

当客户端创建了一个请求来获取一个对象列表时，很重要一点就是你要返回给他们一个符合查询条件的所有对象的列表。这个列表可能会很大。但你不能随意给返回数据的数量做限制。因为这些无谓的限制会导致第三方开发者不知道发生了什么。如果他们请求一个确切的集合并且要遍历结果，然而他们发现只拿到了100条数据。接下来他们就不得不去查找这个限制条件的出处。到底是ORM的bug导致的，还是因为网络截断了大数据包？

尽可能减少那些会影响到第三方开发者的无谓限制

这点很重要，但你可以让客户端自己对结果做一些具体的过滤或限制。这么做最重要的一个原因是可以最小化网络传输，并让客户端尽可能快的得到查询结果。其次是客户端可能比较懒，如果这时服务器能对结果做一些过滤或分页，对大家都是好事。另外一个不那么重要的原因是（从客户端角度来说），对服务器来说响应请求的负载越少越好。

过滤器是最有效的方式去处理那些获取资源集合的请求。所以只要出现GET的请求，就应该通过URL来过滤信息。以下有一些过滤器的例子，可能是你想要填加到API中的：

?limit=10: 减少返回给客户端的结果数量（用于分页）
?offset=10: 发送一堆信息给客户端（用于分页）
?animal_type_id=1: 使用条件匹配来过滤记录
?sortby=name&order=asc:  对结果按特定属性进行排序
有些过滤器可能会与端点URL的效果重复。例如我之前提到的GET /zoo/ZID/animals。它也同样可以通过GET /animals?zoo_id=ZID来实现。独立的端点会让客户端更好过一些，因为他们的需求往往超出你的预期。本文中提到这种冗余差异可能对第三方开发者并不可见。

无论怎么说，当你准备过滤或排序数据时，你必须明确的将那些客户端可以过滤或排序的列放到白名单中，因为我们不想将任何的数据库错误发送给客户端。



Status Codes

状态码

It is very important that as a RESTful API, you make use of the proper HTTP Status Codes; they are a standard after all! Various network equipment is able to read these status codes, e.g. load balancers can be configured to avoid sending requests to a web server sending out lots of 50x errors. There are a plethora of HTTP Status Codes to choose from, however this list should be a good starting point:

对于一个RESTful API来说很重要的一点就是要使用HTTP的状态码，因为它们是HTTP的标准。很多的网络设备都可以识别这些状态码，例如负载均衡器可能会通过配置来避免发送请求到一台web服务器，如果这台服务器已经发送了很多的50x错误回来。这里有大量的HTTP状态码可以选择，但是下面的列表只给出了一些重要的代码作为一个参考：

200 OK – [GET]
The Consumer requested data from the Server, and the Server found it for them (Idempotent)
客户端向服务器请求数据，服务器成功找到它们
201 CREATED – [POST/PUT/PATCH]
The Consumer gave the Server data, and the Server created a resource
客户端向服务器提供数据，服务器根据要求创建了一个资源
204 NO CONTENT – [DELETE]
The Consumer asked the Server to delete a Resource, and the Server deleted it
客户端要求服务器删除一个资源，服务器删除成功
400 INVALID REQUEST – [POST/PUT/PATCH]
The Consumer gave bad data to the Server, and the Server did nothing with it (Idempotent)
客户端向服务器提供了不正确的数据，服务器什么也没做
404 NOT FOUND – [*]
The Consumer referenced an inexistant Resource or Collection, and the Server did nothing (Idempotent)
客户端引用了一个不存在的资源或集合，服务器什么也没做
500 INTERNAL SERVER ERROR – [*]
The Server encountered an error, and the Consumer has no knowledge if the request was successful
服务器发生内部错误，客户端无法得知结果，即便请求已经处理成功
Status Code Ranges

状态码范围

The 1xx range is reserved for low-level HTTP stuff, and you’ll very likely go your entire career without manually sending one of these status codes.

The 2xx range is reserved for successful messages where all goes as planned. Do your best to ensure your Server sends as many of these to the Consumer as possible.

The 3xx range is reserved for traffic redirection. Most APIs do not use these requests much (not nearly as often as the SEO folks use them ;), however, the newer Hypermedia style APIs will make more use of these.

The 4xx range is reserved for responding to errors made by the Consumer, e.g. they’re providing bad data or asking for things which don’t exist. These requests should be be idempotent, and not change the state of the server.

The 5xx range is reserved as a response when the Server makes a mistake. Often times, these errors are thrown by low-level functions even outside of the developers hands, to ensure a Consumer gets some sort of response. The Consumer can’t possibly know the state of the server when a 5xx response is received, and so these should be avoidable.

1xx范围的状态码是保留给底层HTTP功能使用的，并且估计在你的职业生涯里面也用不着手动发送这样一个状态码出来。

2xx范围的状态码是保留给成功消息使用的，你尽可能的确保服务器总发送这些状态码给用户。

3xx范围的状态码是保留给重定向用的。大多数的API不会太常使用这类状态码，但是在新的超媒体样式的API中会使用更多一些。

4xx范围的状态码是保留给客户端错误用的。例如，客户端提供了一些错误的数据或请求了不存在的内容。这些请求应该是幂等的，不会改变任何服务器的状态。

5xx范围的状态码是保留给服务器端错误用的。这些错误常常是从底层的函数抛出来的，并且开发人员也通常没法处理。发送这类状态码的目的是确保客户端能得到一些响应。收到5xx响应后，客户端没办法知道服务器端的状态，所以这类状态码是要尽可能的避免。

Expected Return Documents

预期的返回文档

When performing actions using the different HTTP verbs to Server endpoints, a Consumer needs to get some sort of information in return. This list is pretty typical of RESTful APIs:

GET /collection: Return a listing (array) of Resource objects
GET /collection/resource: Return an individual Resource object
POST /collection: Return the newly created Resource object
PUT /collection/resource: Return the complete Resource object
PATCH /collection/resource: Return the complete Resource object
DELETE /collection/resource: Return an empty document
Note that when a Consumer creates a Resource, they usually do not know the ID of the Resource being created (nor other attributes such as created and modified timestamps, if applicable). These additional attributes are returned with subsequent request, and of course as a response to the initial POST.

当使用不同的HTTP动词向服务器请求时，客户端需要在返回结果里面拿到一系列的信息。下面的列表是非常经典的RESTful API定义：

GET /collection: 返回一系列资源对象
GET /collection/resource: 返回单独的资源对象
POST /collection: 返回新创建的资源对象
PUT /collection/resource: 返回完整的资源对象
PATCH /collection/resource: 返回完整的资源对象
DELETE /collection/resource: 返回一个空文档
请注意当一个客户端创建一个资源时，她们常常不知道新建资源的ID（也许还有其他的属性，如创建和修改的时间戳等）。这些属性将在随后的请求中返回，并且作为刚才POST请求的一个响应结果。

Authentication

认证

Most of the time a Server will want to know exactly who is making which Requests. Sure, some APIs provide endpoints to be consumed by the general (anonymous) public, but most of the time work is being perform on behalf of someone.

OAuth 2.0 provides a great way of doing this. With each Request, you can be sure you know which Consumer is making requests, which User they are making requests on behalf of, and provides a (mostly) standardized way of expiring access or allowing Users to revoke access from a Consumer, all without the need for a third-party consumer to know the Users login credentials.

There are also OAuth 1.0 and xAuth, which fill the same space. Whichever method you choose, make sure it is something common and well documented with many different libraries written for the languages/platforms which your Consumers will likely be using.

I can honestly tell you that OAuth 1.0a, while it is the most secure of the options, is a huge pain in the ass to implement. I was surprised by the number of Third Party Developers who had to implement their own library since one didn’t exist for their language already. I’ve spent enough hours debugging cryptic “invalid signature” errors to recommend you choose an alternative.

服务器在大多数情况下是想确切的知道谁创建了什么请求。当然，有些API是提供给公共用户（匿名用户）的，但是大部分时间里也是代表某人的利益。

OAuth2.0提供了一个非常好的方法去做这件事。在每一个请求里，你可以明确知道哪个客户端创建了请求，哪个用户提交了请求，并且提供了一种标准的访问过期机制或允许用户从客户端注销，所有这些都不需要第三方的客户端知道用户的登陆认证信息。

还有OAuth1.0和xAuth同样适用这样的场景。无论你选择哪个方法，请确保它为多种不同语言/平台上的库提供了一些通用的并且设计良好文档，因为你的用户可能会使用这些语言和平台来编写客户端。

Content Type

内容类型

Currently, the most “exciting” of APIs provide JSON data from RESTful interfaces. This includes Facebook, Twitter, GitHub, you name it. XML appears to have lost the war a while ago (except in large corporate environments). SOAP, thankfully, is all but dead, and we really don’t see much APIs providing HTML to be consumed (unless, that is, you’re building a scraper!)

Developers using popular languages and frameworks can very likely parse any valid data format you return to them. You can even provide data in any of the aforementioned data formats (not including SOAP) quite easily, if you’re building a common response object and using a different serializer. What does matter though, is that you make use of the Accept header when responding with data.

Some API creators recommend adding a .json, .xml, or .html file extension to the URL (after the endpoint) for specifying the content type to be returned, although I’m personally not a fan of this. I really like the Accept header (which is built into the HTTP spec) and feel that is the appropriate thing to use.

目前，大多数“精彩”的API都为RESTful接口提供JSON数据。诸如Facebook，Twitter，Github等等你所知的。XML曾经也火过一把（通常在一个大企业级环境下）。这要感谢SOAP，不过它已经挂了，并且我们也没看到太多的API把HTML作为结果返回给客户端（除非你在构建一个爬虫程序）。

 只要你返回给他们有效的数据格式，开发者就可以使用流行的语言和框架进行解析。如果你正在构建一个通用的响应对象，通过使用一个不同的序列化器，你也可以很容易的提供之前所提到的那些数据格式（不包括SOAP）。而你所要做的就是把使用方式放在响应数据的接收头里面。

有些API的创建者会推荐把.json, .xml, .html等文件的扩展名放在URL里面来指示返回内容类型，但我个人并不习惯这么做。我依然喜欢通过接收头来指示返回内容类型（这也是HTTP标准的一部分），并且我觉得这么做也比较适当一些。

Hypermedia APIs

超媒体API

Hypermedia APIs are very likely the future of RESTful API design. They’re actually a pretty amazing concept, going “back to the roots” of how HTTP and HTML was intended to work.

When working with non-Hypermedia RESTful APIs, the URL Endpoints are part of the contract between the Server and the Consumer. These Endpoints MUST be known by the Consumer ahead of time, and changing them means the Consumer is no longer able to communicate with the Server as intended. This, as you can assume, is quite a limitation.

Now, API Consumers are of course not the only user agent making HTTP requests on the Internet. Far from it. Humans, with their web browsers, are the most common user agent making HTTP requests. Humans, however, are NOT locked into this predefined Endpoint URL contract that RESTful APIs are. What makes humans so special? Well, they’re able to read content, click links for headings which look interesting, and in general explore a website and interpret content to get to where they want to go. If a URL changes, a human is not affected (unless, that is, they bookmarked a page, in which case they go to the homepage and find a new route to their beloved data).

The Hypermedia API concept works the same way a human would. Requesting the Root of the API returns a listing of URLs which point perhaps to each collection of information, and describing each collection in a way which the Consumer can understand. Providing IDs for each resource isn’t important (or necessarily required), as long as a URL is provided.

With the Consumer of a Hypermedia API crawling links and gathering information, URLs are always up-to-date within responses, and do not need to be known beforehand as part of a contract. If a URL is ever cached, and a subsequent request returns a 404, the Consumer can simply go back to the root and discover the content again.

When retrieving a list of Resources within a Collection, an attribute containing a complete URL for the individual Resources are returned. When performing a POST/PATCH/PUT, the response can be a 3xx redirect to the complete Resource.

JSON doesn’t quite give us the semantics we need for specifying which attributes are URLs, nor how URLs relate to the current document. HTML, as you can probably guess, does provide this information. We may very well see our APIs coming full circle and returning back to consuming HTML. Considering how far we’ve come with CSS, one day we may even see  it be common practice for APIs and Websites to use the exact same URLs and content.

超媒体API很可能就是RESTful API设计的将来。超媒体是一个非常棒的概念，它回归到了HTTP和HTML如何运作的“本质”。

在非超媒体RESTful API的情景中，URL端点是服务器与客户端契约的一部分。这些端点必须让客户端事先知道，并且修改它们也意味着客户端可能再也无法与服务器通信了。你可以先假定这是一个限制。

时至今日，英特网上的API客户端已经不仅仅只有那些创建HTTP请求的用户代理了。大多数HTTP请求是由人们通过浏览器产生的。人们不会被哪些预先定义好的RESTful API端点URL所束缚。是什么让人们变的如此与众不同？因为人们可以阅读内容，可以点击他们感兴趣的链接，并浏览一下网站，然后跳到他们关注的内容那里。即使一个URL改变了，人们也不会受到影响（除非他们事先给某个页面做了书签，这时他们回到主页并发现原来有一条新的路径可以去往之前的页面）。

超媒体API概念的运作跟人们的行为类似。通过请求API的根来获得一个URL的列表，这个列表里面的每一个URL都指向一个集合，并且提供了客户端可以理解的信息来描述每一个集合。是否为每一个资源提供ID并不重要（或者不是必须的），只要提供URL即可。

一个超媒体API一旦具有了客户端，那么它就可以爬行链接并收集信息，而URL总是在响应中被更新，并且不需要如契约的一部分那样事先被知晓。如果一个URL曾经被缓存过，并且在随后的请求中返回404错误，那么客户端可以很简单的回退到根URL并重新发现内容。

在获取集合中的一个资源列表时会返回一个属性，这个属性包含了各个资源的完整URL。当实施一个POST/PATCH/PUT请求后，响应可以被一个3xx的状态码重定向到完整的资源上。

JSON不仅告诉了我们需要定义哪些属性作为URL，也告诉了我们如何将URL与当前文档关联的语义。正如你猜的那样，HTML就提供了这样的信息。我们可能很乐意看到我们的API走完了完整的周期，并回到了处理HTML上来。想一下我们与CSS一起前行了多远，有一天我们可能再次看到它变成了一个通用实践让API和网站可以去使用相同的URL和内容。

Documentation

文档

Honestly, if you don’t conform 100% to the criteria in this guide, your API will not necessarily be horrible. However, if you don’t properly document your API, nobody is going to know how to use it, and it WILL be a horrible API.

Make your Documentation available to unauthenticated developers.

Do not use automatic documentation generators, or if you do, at least make sure you’re doctoring it up and making it presentable.

Do not truncate example request and response bodies; show the whole thing. Use a syntax highlighter in your documentation.

Document expected response codes and possible error messages for each endpoint, and what could have gone wrong to cause those error messages.

If you’ve got the spare time, build a developer API console so that developers can immediately experiment with your API. It’s not as hard as you might think and developers (both internal and third party) will love you for it!

Make sure your documentation can be printed; CSS is a powerful thing; don’t be afraid to hide that sidebar when the docs are printed. Even if nobody ever prints a physical copy, you’d be surprised at how many developers like to print to PDF for offline reading.

老实说，即使你不能百分之百的遵循指南中的条款，你的API也不是那么糟糕。但是，如果你不为API准备文档的话，没有人会知道怎么使用它，那它真的会成为一个糟糕的API。

让你的文档对那些未经认证的开发者也可用
不要使用文档自动化生成器，即便你用了，你也要保证自己审阅过并让它具有更好的版式。
不要截断示例中请求与响应的内容，要展示完整的东西。并在文档中使用高亮语法。
文档化每一个端点所预期的响应代码和可能的错误消息，和在什么情况下会产生这些的错误消息
如果你有富余的时间，那就创建一个控制台来让开发者可以立即体验一下API的功能。创建一个控制台并没有想象中那么难，并且开发者们（内部或者第三方）也会因此而拥戴你。

另外确保你的文档能够被打印。CSS是个强大的工具可以帮助到你。而且在打印的时候也不用太担心边侧栏的问题。即便没有人会打印到纸上，你也会惊奇的发现很多开发者愿意转化成PDF格式进行离线阅读。

Errata: Raw HTTP Packet

勘误：原始的HTTP封包

Since everything we do is over HTTP, I’m going to show you a dissection of an HTTP packet. I’m often surprised at how many people don’t know what these things look like! When the Consumer sends a Request to the Server, they provide a set of Key/Value pairs, called a Header, along with two newline characters, and finally the request body. This is all sent in the same packet.

The server then responds in the say Key/Value pair format, with two newlines and then the response body. HTTP is very much a request/response protocol; there is no “Push” support (the Server sending data to the Consumer unprovoked), unless you use a different protocol such as Websockets.

When designing your API, you should be able to work with tools which allow you to look at raw HTTP packets. Consider using Wireshark, for example. Also, make sure you are using a framework / web server which allows you to read and change as many of these fields as possible.

因为我们所做的都是基于HTTP协议，所以我将展示给你一个解析了的HTTP封包。我经常很惊讶的发现有多少人不知道这些东西。当客户端发送一个请求道服务器时，他们会提供一个键值对集，先是一个头，紧跟着是两个回车换行符，然后才是请求体。所有这些都是在一个封包里被发送。

服务器响应也是同样的键值对集，带两个回车换行符，然后是响应体。HTTP就是一个请求/响应协议；它不支持“推送”模式（服务器直接发送数据给客户端），除非你采用其他协议，如Websockets。

当你设计API时，你应该能够使用工具去查看原始的HTTP封包。Wireshark是个不错的选择。同时，你也该采用一个框架/web服务器，使你能够在必要时修改某些字段的值。

Example HTTP Request

1
2
3
4
5
6
7
8
9
10
POST /v1/animal HTTP/1.1
Host: api.example.org
Accept: application/json
Content-Type: application/json
Content-Length: 24

{
  "name": "Gir",
  "animal_type": 12
}
 Example HTTP Response

1
2
3
4
5
6
7
8
9
10
11
12
13
HTTP/1.1 200 OK
Date: Wed, 18 Dec 2013 06:08:22 GMT
Content-Type: application/json
Access-Control-Max-Age: 1728000
Cache-Control: no-cache

{
  "id": 12,
  "created": 1386363036,
  "modified": 1386363036,
  "name": "Gir",
  "animal_type": 12
}
