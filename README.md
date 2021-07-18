# gRPC
**WHY gRPC ?**

We started with **RPC**, Remote Procedural Call. In this, the client invokes a procedure on the server site in a different address space. As if the client was trying to call the function locally. RPC uses TCP protocol which hinders inter operability. RPCs are overwhelmingly complex to implement. It induces a certain amount of vulnerability because you're trying to connect to a different system in a different address space. And hence also incurs a lot of costs. Popular implementations of RPC are Java RMI, CORBA, etcetera. Owing to limitations of RPC, 

we switched to a new standard, **SOAP**, Simple Object Access Protocol. This specification was designed and promoted by a number of big enterprises. SOAP is based on the services oriented architecture, or as we say, the SOA principle. In this, the client and the server exchange data in the form of XML. And since it is XML, it can be implemented across platforms and systems, which are completely different from each other. SOAP works over both DCP and HTTP protocol. SOAP is prevalent in a lot of legacy services today because there are a few limitations. XML is difficult to deal with. It has a strict set of rules that you need to know to work with it. Which means even the smallest string message transmitted between the client and the server amounts to a heavy payload which eventually leads to performance issues. 

Agility of distributed applications are a problem to implement with the SOAP specification. Hence, we moved on to REST. **REST** is a specification in which you define the methods of a service. And a service then is a bunch of resources, each with a unique identifier, URL. REST works only with HTTP protocol. REST is the de facto standard for implementing microservices today. However, it also comes in with a few limitations. Firstly, REST has text-based message calls. The client always produces data in a binary format. It has to be converted to text to get transmitted over the HTTP protocol. That is because HTTP understands only text. When the response is received on the server side, the server has to convert that message from text again to binary. So this conversion from binary to text and text to binary amounts to performance overhead. REST only exposes CRUD, or CRUD behaviors, create, read, update, delete. And these behaviors has got a set name of methods that you need to define in your service The names of those methods are, get ghost, put delete, patch, etcetera. Which means if you need to give a custom name to your method, you will always have to work around these set names in order to expose your specific API. This becomes a little cumbersome in the long run. REST does not define strict interface types. Even when you have open API or swagger specifications, it is not tightly bound with the underlying architecture or the message in protocol. The service and the type definitions are sometimes not even written completely before the implementation gets started. At times, the developers even just look over the wire for the data format and code the functionality accordingly. This results in incompatibility, inconsistency and a lot of runtime problems when the calls are made. So then what do you do in order to overcome these limitations of REST? 
The answer to that question is, let's switch to the **gRPC** framework.


HOW:
gRPC framework is built on a couple of strong foundations, which we need to understand before we move further in this course. Firstly, HTTP/2 protocol. We've already discussed before that the framework works on this protocol. So it would be nice to get an understanding of the working mechanism of this protocol. Secondly, protocol buffers or they're commonly known as protobuffs. These are used as the interface definition language, IDL, in gRPC. IDL is the way to define your service contract. That contract will contain the methods and the messages that you will pass when you call the methods. Protocol buffers is not the only way to do IDL in gRPC, but Google promotes this as a standard when you try to build services with this framework.


**HTTP-1 protocol issues:**
Http1 has few issues like:
![image](https://user-images.githubusercontent.com/11258384/125916778-5a9a6e2d-f058-4bed-9e69-8024c9f975c3.png)


**Http1.1 solved some problems**
An incremental version of HTTP that is 1.1 was introduced, web pipelining came into existence. Pipelining leads to persistent connections. What is it? It means that one TCP connection alone can handle multiple requests. There is no need to wait for another connection. Requests can be sent one after the other so that the server starts their processing. This means that if you want to run a lot of requests in parallel, you end up spawning multiple TCP connections. However, note that there is a limit to the number of TCP connections that the client wants to set up. But even if that happens, the responses still must come in the same order as the request sent. This means that if the response of a particular request is delayed, for example the request was held up for a database connection or some other resource than all the other responses are blocked and cannot be sent back to the client. This results in a big issue which is called head of line blocking, HOL. But this is an unintended side effect. These problems solved by the HTTP/2 protocol.


**EXAMPLE:**
**How to Run**
Run applications as:
java -jar grpc-springboot-demo\demo-eureka-server\target\demo-eureka-server-0.0.1-SNAPSHOT.jar
java -jar grpc-springboot-demo\demo-employee-service\target\demo-employee-service-0.0.1-SNAPSHOT.jar
java -jar grpc-springboot-demo\demo-allocation-service\target\demo-allocation-service-0.0.1-SNAPSHOT.jar

Service Methods are:
Unary RPCs (gRPC Server: employee-service, gRPC Client: allocation-service)
Proto Definition: rpc getEmployee (Employee) returns (Employee) { }
End Point: {IP Address}:8082/allocation/{allocationID}

Server streaming RPCs (gRPC Server: allocation-service, gRPC Client: employee-service)
Proto Definition: rpc getAllocationByEmployee (Allocation) returns (stream Allocation) { }
Synchronous End Point: {IP Address}:8089/employee/{employeeID}/allocation
Asynchronous End Point: {IP Address}:8089/employee/{employeeID}/allocation?isSyncClient=N

Client streaming RPCs (gRPC Server: employee-service, gRPC Client: allocation-service)
Proto Definition: rpc getMostExperiencedEmployee (stream Employee) returns (Employee) { }
End Point: {IP Address}:8082/{projectID}/allocation/getexperiencedemployeeinproject

Bidirectional streaming RPCs (gRPC Server: employee-service, gRPC Client: allocation-service)
Proto Definition: rpc getAllEmployeesByIDList (stream Employee) returns (stream Employee) { }
End Point: {IP Address}:8082/allocation?projectID={projectID}
