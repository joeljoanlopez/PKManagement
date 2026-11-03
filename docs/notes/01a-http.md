# Chapter 01a - HTTP and client-server model

## Self-assessment questions

### 1. Difference between header and body in an HTTP message

An HTTP message header is the information about the client sending the message. For example, these headers can come with the host, the language, the IP, etc.

### 2. Difference between a safe method and an idempotent one.

A safe method is the method that does no modifications or mutations to the server data. For example, GET is a read-only method so it should be considered a safe one. 
An idempotent method is the one that modifies data in the server, but they can be safely retried. For example, one can call twice PUT or DELETE, and making one of those requests will have the same output as making several equal requests (DELETE /users/1 called once will have the same effect as called 10 times)

### 3. Why is POST not idempotent but PUT is? What about PATCH?

POST is not idempotent because if, for example, the operation is incremental (POST /increment-score), the effect of a second call will modify the data further (in a different way than the first one). PUT always modifies data in the same way, so a PUT method would just set a specific counter to 1 (PUT /score/1).