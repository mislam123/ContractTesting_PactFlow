# Contract Testing Workshop
This workshop is setup with a number of steps that can be run through. Each step is in a branch, so to run through a step of the workshop just check out the branch for that step (i.e. git checkout step1) or, check out the branch by the IDE.

# Pre-requirements:
   - Java 11
   - Spring Boot
   - Junit 5
   - Pact
   - Pactflow (SAAS version of pact broker)
   - Jenkins
   - IntelliJ (Match 2019 0r, later version)
   - Git

Overview:
=========
This workshop is aimed at demonstrating core features and benefits of contract testing with Pact.
There are two components in scope for our workshop.

1. Product Catalog application (Consumer). It provides a console interface to query the Product service for product information.
2. Product Service (Provider). Provides useful things about products, such as listing all products and getting the details of an individual product.


Get Started:
============
- Clone the master brunch of the "contract testing Workshop" (sfnta_im0153_contract_testing_workshop)from the bitbucket repo.
- After cloning the reposatoty, you will get the core framework/ code to work with in step by step.

Now follow below steps to build, publish and run for the contract testing step by step.


Step 1: Simple Consumer calling Provider
========================================
Check out the brunch {step 1}

We need to first create an HTTP client to make the calls to our provider service:

![image](https://user-images.githubusercontent.com/5817220/137738279-3790464e-0e11-4e98-a7a8-40aa3f75b091.png)

The Consumer has implemented the product service client which has the following:

   - GET /products - Retrieve all products
   - GET /products/{id} - Retrieve a single product by ID
The diagram below highlights the interaction for retrieving a product with ID 10:

![image](https://user-images.githubusercontent.com/5817220/137738501-2ce54ee1-6647-4bbd-8aa7-0f1e79a2049d.png)


At first, we can check the configaration is set up in consumer ....
    \\consumer\src\main\java\com\freddie\pact\consumer\configaration\ConsumerConfiguration.java

    @Configuration
    public class ConsumerConfiguration {

    @Bean
    RestTemplate customerRestTemplate(@Value("${provider.port:8080}") int port) {
        return new RestTemplateBuilder().rootUri(String.format(http://localhost:%d, port)).build();
        }
    }

Then we can see the client interface we created in ...
    \\consumer\src\main\java\com\freddie\pact\consumer\service\CustomerService.java


    @Service
    public class CustomerService {

        private static final String BASE_URI_CUSTOMERS = "/customers";
        private static final String SLASH = "/";
        
        private final RestTemplate restTemplate;

    @Autowired
    public CustomerService(RestTemplate restTemplate) {
        this.restTemplate = restTemplate;
        }

    public Customer getCustomer(String id) {
        return restTemplate.getForObject(BASE_URI_CUSTOMERS + SLASH + id, Customer.class);
        }

    public List<Customer> getCustomers() {
        return restTemplate.exchange(BASE_URI_CUSTOMERS, HttpMethod.GET, null,
                new ParameterizedTypeReference<List<Customer>>() {
                }).getBody();
        }

    public Customer createCustomer(Customer customer) {
        return restTemplate.postForObject(BASE_URI_CUSTOMERS, customer, Customer.class);
        }

    public void updateCustomer(Customer customer) {
        restTemplate.put(BASE_URI_CUSTOMERS, customer);
        }

    public void deleteCustomer(String id) {
        restTemplate.delete(BASE_URI_CUSTOMERS + SLASH + id);
        }
    }
 

Now we can run the client with 
> ./gradlew service:bootRun

 - it should fail with the error below, because the Provider is not running.

     Caused by: org.springframework.web.client.ResourceAccessException: I/O error on GET request for "http://localhost:80**/products": Connection refused: connect;      nested exception is java.net.ConnectException: Connection refused: connect

Move on to step 2

Step 2: Client Tested but integration fails
===========================================


