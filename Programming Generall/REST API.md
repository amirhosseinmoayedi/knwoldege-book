#general #rest #api #versiooning #best_practice #architecture 
## Represent State Transfer ( <font color="#92cddc">REST</font> )
**Is an architecture of API**.

<font color="#e36c09">RESTful APIs</font> use [[HTTP]] methods (such as GET, POST, PUT, and DELETE) to manipulate resources, and the API's resources are identified by [[URL]] (Uniform Resource Identifiers).

## <font color="#76923c">REST Architecture</font>:
1. <font color="#fac08f">client-server architecture</font>: <u>client manages user interface</u> and <u>server manages data in the server</u>, it's proper of concern  
2.  <font color="#fac08f">stateless</font>: REST does not keep user data and information(we can use tokens and etc to manage other things)
3. <font color="#fac08f">caching</font>
4. <font color="#fac08f">layered system</font>: system is designed that API consumer does not know or care if the data is from [[CDN]] or ...
5.  <font color="#fac08f">code on demand</font>: servers can compile and send code like javascript to client so they run it.
6.  <font color="#fac08f">uniform interface</font>
    1.  resource identifier in request: uri to locate or send request
    2.  resource manuplation in representation: client can modify the data
    3.  self-descriptive message: each endpoint must have data of format
    4.  hypermedia as the engine of application state:
        there must be a collection and documentation of all API endpoints

## Versioning 

Versioning in REST refers to the practice of assigning a version number to the API, which allows clients to specify which version of the API they want to use.

There are two main approaches to versioning in REST:

1. <font color="#548dd4"> URI Versioning</font>: This approach involves including the <u>version number in the URI of the API</u>. For example, the URI might include "/v1" or "/v2" to indicate the version of the API being used. **One advantage of this approach is that it is easy to understand and implement**. However, **it can lead to long, complex URIs and can make it difficult to change the version number in the future**.
    
2.  <font color="#548dd4">Header Versioning</font>: This approach involves <u>including the version number in an HTTP header</u>, rather than in the URI. This **allows for cleaner, simpler URIs and makes it easier to change the version number in the future**. However, it can be **more complex to implement and may require changes to the client application**.
    

There are also different types of versioning that can be used, such as <font color="#ffc000">major versioning</font> (e.g., v1, v2) or <font color="#ffc000">semantic versioning</font> (e.g., 1.0.0, 1.1.0). The type of versioning used depends on the needs of the API and the development team.

## <font color="#ffc000">Some Best Practice</font>
* <font color="#92d050">Consistency</font>
	the way endpoints are designed should be similar to each other beacuse other people are using it and each endpoint collection should not be different with other.
* <font color="#92d050">Stability in representation</font>
	representation of a resource should be same across the all the related enpoints, the only situation this can change is with the relation with other resource, which the representaion can differe.
* <font color="#92d050">They all should have ID</font>
	Id's should be in all endpoint of that resource, if there are other parameter and search subject like name it should be added beside ID
* <font color="#92d050">Naming for resources matters</font> 
* <font color="#92d050">A Resource data can be from multiple data-base tables</font>
	User should not be aware of the backend structure, it should only know about Resource