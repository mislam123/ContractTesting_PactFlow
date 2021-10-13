# ContractTesting_PactFlow

# Pre-requirements:
- Java 8
- Spring Boot
- Junit 5
- Pact
- Pactflow (SAAS version of pact broker)
- Jenkins
- IntelliJ (Match 2019 0r, later version)
- Git

Overview:
=========
There are two components in scope for our workshop.

1. Product Catalog application (Consumer). It provides a console interface to query the Product service for product information.
2. Product Service (Provider). Provides useful things about products, such as listing all products and getting the details of an individual product.


Get Started:
============
- Clone the master brunch of the Contract testing PactFlow project (sfnta_im0153_contract_testing_workshop)from the bitbucket repo.
- After cloning the reposatoty, you will get the core framework/ code to work with in step by step.

Now follow below steps to build, publish and run for the contract testing.


Step 1: Simple Consumer calling Provider:
=========================================
- Check out the brunch {step 1}

We can see the client interface we created in
\\consumer\src\main\java\com\freddie\pact\consumer\service


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
> ./gradlew consumer:bootRun

 - it should fail with the error below, because the Provider is not running.
Caused by: org.springframework.web.client.ResourceAccessException: I/O error on GET request for "http://localhost:8085/products": Connection refused: connect; nested exception is java.net.ConnectException: Connection refused: connect
