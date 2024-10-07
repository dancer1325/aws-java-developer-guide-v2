# Migrate from version 1\.x to 2\.x of the AWS SDK for Java<a name="migration"></a>

* AWS SDK for Java 2\.x
  * MAJOR rewrite of the 1\.x code base
  * built | Java 8\+\
  * updates
    * improved consistency,
    * ease of use
    * strongly enforce immutability
* goal
  * describes the major NEW features about version 2\.x,
  * how to migrate from 1\.x\ to version 2\.x 

**Topics**
+ [What’s new](#migration-whats-new)
+ [What's different between 1\.x and 2\.x](migration-whats-different.md)
+ [Use the SDK for Java 1\.x and 2\.x side\-by\-side](migration-side-by-side.md)

## What’s new<a name="migration-whats-new"></a>

* own HTTP clients can be configured
  * [HTTP transport configuration](http-configuration.md)
* Async clients
  * NOW truly nonblocking
  * return `CompletableFuture` objects
  * [Asynchronous programming](asynchronous.md)\.
* Operations / return multiple pages -> have autopaginated responses
  * -> you can focus in your code 
  * [Pagination](pagination.md) 
* [SDK start time performance improvements](lambda-optimize-starttime.md) 
* new shorthand method -- for -- creating requests
  * _Example:_  

  ```
  dynamoDbClient.putItem(request -> request.tableName(TABLE))
  ```

* For more details
  * [Quick Start](get-started.md) 
  * [Setting up](setup.md) 
  * [Code examples for the AWS SDK for Java 2\.x](examples.md) 
  * [Use the SDK](using.md) 
  * [Security for the AWS SDK for Java](security.md) 