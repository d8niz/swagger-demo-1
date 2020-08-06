# Swagger-Demo-1
API docs with Swagger first demo application

## Prerequisites
- `Java` dev. environment
- `Spring Boot` (IDE plugin or using browser)
- _Keyboard and fingers (attached to a pair of hands)_

## Step by Step

- Bootstrap a `Maven` project using `Spring Initialzr`
- Choose `Java` as the programming language
- Choose a `group id` and `artifact` name to your preference
- Choose a latest version of `Spring Boot`
- Choose a suitable directory to extract the project.
- Open the project with your fav. IDE (Eclipse is preferred, I will use VSC for complicated reasons)

- Confirm your dependency(ies) are correctly placed in `pom.xml`
    - `mvn clean install` will build the project
- Give it a dry-run after it retrieves necessary `Maven` dependencies`
    - Run your project using the main `application` class, that Spring labeled as `@SpringBootApplication`

- Go head and create a Resource named `AddressBookResource.java` that has following method signatures:
    - `Contact getContact(String id){}`
        - Will fetch a contact by a `path variable` named `ID` of type `String.java`
    - `List<Contact> getAllContacts(){}`
        - Will return a list of contacts 
        - (Hint: Store your contacts in memory using a specific `HashMap<x,y>()` type (i.e. `ConcurentHashMap<x,y>()`) at class level. Return them in here.)
    - `Contact addContact()`
        - Will add and return a contact by a `request body` parameter of type `Contact.java`

    - Assign necessary mappings to methods after implementation (i.e. `@GetMapping`)

- You need a `Contact.java` model, go ahead create your model with following attributes and methods:
    - `-id: string`
    - `-name: string`
    - `-phone: string`
    - `-address: string`
    - Accessors & Mutators for each att. 


- The resource class will look something like this:

```java
@RestController
@RequestMapping("/api")
public class AdressBookResource {

    ConcurrentHashMap<String,Contact> contacts = new ConcurrentHashMap<>();

    @GetMapping("/{id}")
    public Contact getContact(@PathVariable String id){
        return contacts.get(id);

    }

    @GetMapping("/")
    public List<Contact> getAllContacts(){
        return new ArrayList<Contact>(contacts.values());

    }

    @PostMapping("/")
    public Contact addContact(@RequestBody Contact contact){
    
        contacts.put(contact.getId(), contact);
        return contact;
 
    }
}
```

- The model class will look something like this:

```java
public class Contact {
    private String id;
    private String name;
    private String phone;

    public String getId() {
        return this.id;
    }

    public void setId(String id) {
        this.id = id;
    }

    public String getName() {
        return this.name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getPhone() {
        return this.phone;
    }

    public void setPhone(String phone) {
        this.phone = phone;
    }   
}
```
- Run your application and test your endpoints using an API client (i.e. `Postman`, `SOAPUI`)


- Now add `Swagger 2` Maven dependency to your `pom.xml` file:

```xml
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>2.9.2</version>
</dependency>
```

- Now enable Swagger 2 in your application by modifying main application class:
    - Add `@EnableSwagger2` annotation at class level.

- Run your application and go to this [URL](http://localhost:8080/swagger-resources/)
    - Notice that your API is documented by `Swagger 2` in `JSON` format at the given url on the web-page as `/v2/api-docs` on top of the base of URL of your application that is `/`. Go to that [URL](http://localhost:8080/v2/api-docs) and see it for yourself.
    - If you want to see `JSON` in pretty format, add `a Browser extension`, or just make a request on `Postman`.



- Now add `Swagger-UI` Maven dependency to your `pom.xml` file:

```xml
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
    <version>2.9.2</version>
</dependency>
</dependencies>
```

- Add a `@RequestMapping("/api")` to your controller at class level, not to take over (occupy) the root `URL`. This will allow to accept requests in and `Swagger-UI` will handle the root URL.

- Run your application and just go to this [link](http://localhost:8080/swagger-ui.html).

- Et voila... You have just learned the first few steps of API documentation using `Swagger 2` by taking advantage of speed that `Spring Boot` provides.

- Notice that `Swagger` documents all the endpoints (meaning default ones that comes witg `Spring` itselft), next you need to learn how to customize `Swagger` for your documentation needs.