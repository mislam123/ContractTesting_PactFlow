# ContractTesting_PactFlow

# Pre-requirements:
- Java 8
- Spring Boot
- Junit 5
- Pact
- Pactflow (SAAS version of pact broker)
- Jenkins
- IntelliJ (Match 2019 0r, later version)


Get Started:
============

Clone the project this reposatory: {project reposatory link}

Now follow below steps to build, publish and run for the contract testing.


Step # 1:
=========
Check out the brunch step 1
We can run the client with 
> ./gradlew consumer:bootRun
 - it should fail with the error below, because the Provider is not running.
Caused by: org.springframework.web.client.ResourceAccessException: I/O error on GET request for "http://localhost:8085/products": Connection refused: connect; nested exception is java.net.ConnectException: Connection refused: connect
