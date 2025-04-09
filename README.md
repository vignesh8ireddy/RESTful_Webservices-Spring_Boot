# REST Webservices

* Distributed Application makes components of same or different servers belonging to same or different machines
communicate and exchange data.
* XML and JSON data is platform independent, OS independent, language and technology independent.
  * XML and JSON data is called global data and used for exchange of information by distributed apps.
* Every Web application has four layers:
  * Presentation Layer (UI layer) : React, Angular,...
  * Service Layer : Java class / Spring bean with business logics
  * Persistence Layer (Data Access layer) : Spring Data JPA, Hibernate, Spring JDBC,...
  * Data Layer : Database, Text files, Properties files,...
* Distributed application has one more layer called integration layer along with the above four layers in both
consumer app and producer app.
  * Integration Layer of Consumer App : Stubs logics
  * Integration Layer of Producer App : Skeleton logics
* Webservices is a mechanism of linking two applications belonging to a Distributed App as consumer and producer app
using the HTTP protocol.
* Webservices supports interoperability i.e It is platform independent, architecture independent, and language
independent.
  * Develop the producer app in any language and consume its services in any language.
* Two ways of implementing Webservices
  1. SOAP (Simple Object Access Protocol) Webservices
  2. RESTful (REpresentational State Transfer) Webservices
     * REST is not a protocol, it's kind of an architecture.
* Limitations of SOAP Webservices
  * Based on SOA where service registry is required to find and publish the webservices. Registries=> heavy maintenance.
  * SOAP messages in http request and http response are complex to build, costly and heavy to send over the network.
  * Understanding the web service by reading WSDL document is not an easy process.
  * Does not give flexibility to send data in different global formats, only supports XML format as SOAP messages and
  XML is becoming outdated with JSON become very popular.
* RESTful webservices overcome the limitations of SOAP Webservices.
* The HTTP request and HTTP response contains two parts:
  1. HEAD part: Contains the following two sections
     * Initial Line: This section has Http method/mode, path, protocol and HTTP version.
     * Header: This section has lot of xxxxxnonsensexxxxxx information.
  2. BODY part
* 9 HTTP request methods/modes: 
  * GET: Read Operations i.e Getting Data, (HTML Hyperlinks)
  * POST: Request with Data
  * DELETE: Delete Request
  * PUT: Full Update Request
  * PATCH: Partial Update Request
  * HEAD: Request without body
  * OPTIONS: Gives the possible http request modes that can be used to generate the request to web component.
  * TRACE: To trace/debug components involved for the success or failure of request processing.
  * CONNECT (Reserved for future)
* HTTP Status Code:
  * 1xx (100-199) : Informational
  * 2xx (200-299) : Success
  * 3xx (300-399) : Redirection
  * 4xx (400-499) : Incomplete (Client side errors)
  * 5xx (500-599) : Server side errors
* RESTful webservices are created using Spring MVC using @RestController class.
  * The business methods of @RestController class are called REST Operations or Endpoints.
  * To invoke the EndPoint from consumer app we must know the method related URL, request path, request mode and
  parameters. All these details together called EndPoint details.
  * Producer App i.e @RESTController class is also called API or REST API.
* @RestController = @Controller + @ResponseBody
* It is Spring Boot MVC without ViewResolver and View Components, because the @RestController applied on Producer app,
automatically applies @ResponseBody on all the Endpoints which makes these methods to send the generated results
directly to the consumer app through FrontController Servlet component.
* ResponseEntity<T>, T : String, Java Objects or Collection
  * ResponseEntity object contains two parts:
    1. Response Content/Body - Represents result/output i.e T type object
    2. Response Status code - 100 to 599 numbers or ResponseStatus enum constants
  * If <T> is String then the response body goes to client as plain text.
  * If <T> is Object/Array/Collection then the response body is converted into JSON key value pairs by the Jackson API
  which is provided by the Spring Web starter by default.
* Web Application: @Controller class with @GetMapping, @PostMapping
  * Browser as a client can generate only GET and POST type requests.
* Distributed Application: @Controller class with @ResponseBody + @GetMapping, @PostMapping, @DeleteMapping,
@PutMapping, @PatchMapping,....
  * Restful application as a client can generate all 9 types of requests.
* POSTMAN Tool is a ready-made simulator for a consumer app which can send different modes of request.
* JSON - JavaScript Object Notation
  * It is a way of representing Object data using key:value pairs format.
  * Key must be in " ", and value can be anything, if the value is also string then it must be in " ".
  * { } represents JSON object or sub object.
  * [] represents array/list/set collection values i.e 1D Array
  * Map collection values is treated as 2D Array in JSON.
  * Map collection and HAS-A property elements are represented using sub objects.
  * JSON doesn't support commenting
  * Less secure because of its key value pair format
  * Less structured compared to XML
  * Lightweight
  * Serialization: Object to JSON, DeSerialization: JSON to Object
  * Used in Restful Webservices
* XML - eXtensible markup language.
  * Supports commenting
  * More secure because of hierarchy structure
  * More structured (Major drawback)
  * Heavyweight
  * Marshalling: Object to XML, UnMarshalling: XML to Object
  * Can be used in SOAP Webservices, Restful Webservices
* Complex Object (int, String, double, String[], List, Set, Map, Object) to JSON and vice-versa.
* "Accept" of request header specifies response content type i.e JSON, XML
  * Accept: application/JSON - Gets response body as JSON if non-string
  * Accept: application/XML - Gets response body as XML if non-string
  * Accept: */* - Default, same as application/JSON
  * If response body is String then it is sent as plain text always.
* For exchanging data in XML format instead of JSON, add this dependency from mvnrepository explicitly,
  <pre>
    jackson-dataformat-xml
  </pre>
* The return type of Endpoint need not be ResponseEntity<T>, it can also be String, Collection, Java Object,... But,
doing so, we lose the control over response status code, it is always 200 if ResponseEntity<T> is not used.
  * Using ResponseEntity<T> as return type of EndPoint is recommended.
* XML/JSON to REST API : @RequestBody parameter of EndPoint converts XML/JSON to Model class object.
  * If request type is not GET, then content is sent as Request Body.
  * XML/JSON <-> java.time.LocalDateTime :: "date":"yyyy-MM-ddTHH:mm:ss" <-> LocalDatetime date = "yyyy:MM:ddTHH:mm:ss"
  * If the request body content type is XML and the producer app is not having jackson-dataformat-xml.jar file we get
  415 error i.e Unsupported Media Type.
  * @RequestBody is useful to convert XML/JSON data to Object and @ResponseBody is useful to convert Non-String data to
  XML/JSON.
  * If request body contains invalid JSON pattern content then RestController throws 400 error i.e Bad Request error to
  client.
* If request type is GET, then content is not sent as Request Body, GET request can carry limited amount of data which
is sent as query string along with URL.
* Request Parameters to REST API : @RequestParam parameter of EndPoint
  * Request Parameters -  url?key1=val1?key2=val2?key3=val3
  * @RequestParam("key") T paramName or @RequestParam T key
  * required attribute of @RequestParam
* Path Variables to REST API : @PathVariable parameter of EndPoint along with
@XxxxMapping()"/staticPath/{pathVar1}/{pathVar2}").
  * Path Variables - url/value1/value2/value3
  * @PathVariable("pathVar1") T paramName or @PathVariable T pathVar1
  * required attribute of @PathVariable
* We can have multiple request paths for an EndPoint because value parameter of @GetMapping is String[].
  * @GetMapping(value={"/staticPath/{pathVar1}/{pathVar2}","/staticPath/{pathVar}"})
* RestTemplate