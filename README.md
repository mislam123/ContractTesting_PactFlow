# ContractTesting_PactFlow

# Pre-requirements:
- Java 8
- Spring Boot
- Junit 5
- Pact
- Pactflow (SAAS version of pact broker)
- Jenkins
- IntelliJ (Match 2019 0r, later version)

Overview:
=========
There are two components in scope for our workshop.

1. Product Catalog application (Consumer). It provides a console interface to query the Product service for product information.
2. Product Service (Provider). Provides useful things about products, such as listing all products and getting the details of an individual product.


Get Started:
============
- Clone the master brunch of the ContractTesting_PactFlow project: {project reposatory link}
- After cloning the reposatoty, you will get the core framework/ code to work with in step by step.

Now follow below steps to build, publish and run for the contract testing.


Step 1: Simple Consumer calling Provider:
=========================================
- Check out the brunch {step 1}

You can see the client interface we created in 
consumer/src/main/au/com/dius/pactworkshop/consumer/ProductService.java:

@Service
public class ProductService {

    private final RestTemplate restTemplate;

    @Autowired
    public ProductService(RestTemplate restTemplate) {
        this.restTemplate = restTemplate;
    }

    public List<Product> getAllProducts() {
        return restTemplate.exchange("/products",
                HttpMethod.GET,
                null,
                new ParameterizedTypeReference<List<Product>>(){}).getBody();
    }

    public Product getProduct(String id) {
        return restTemplate.getForEntity("/products/{id}", Product.class, id).getBody();
    }
}


Now we can run the client with 
> ./gradlew consumer:bootRun
 - it should fail with the error below, because the Provider is not running.
Caused by: org.springframework.web.client.ResourceAccessException: I/O error on GET request for "http://localhost:8085/products": Connection refused: connect; nested exception is java.net.ConnectException: Connection refused: connect
