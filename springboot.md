Perfect. Since you're preparing for Java/Spring Boot interviews, I'll explain these exactly the way an interviewer expects. I'll also relate them to your NestJS experience whenever possible, because many concepts are almost identical.

---

# 1. What is `@SpringBootApplication`?

This annotation marks the **main class** of a Spring Boot application.

It tells Spring:

* This is the starting point of the application.
* Scan the project for Spring components.
* Enable Spring Boot auto-configuration.

Example:

```java
@SpringBootApplication
public class EmployeeApplication {

    public static void main(String[] args) {
        SpringApplication.run(EmployeeApplication.class, args);
    }

}
```

When you run this class,

```
main()
   ↓
SpringApplication.run()
   ↓
Starts Tomcat
Loads Beans
Connects Database
Starts REST APIs
```

Without this annotation, Spring Boot doesn't know where to begin.

### Internally it is actually three annotations

```java
@SpringBootApplication
```

is equivalent to

```java
@Configuration
@EnableAutoConfiguration
@ComponentScan
```

### Interview Answer

> `@SpringBootApplication` is the main annotation that starts a Spring Boot application. It combines configuration, component scanning, and auto-configuration so Spring can automatically configure the application and scan for components.

---

# 2. What is `@RestController`?

This tells Spring:

> This class contains REST API endpoints.

Example

```java
@RestController
public class EmployeeController {

}
```

Without it,

Spring won't expose APIs.

Example

```java
@RestController
public class EmployeeController {

    @GetMapping("/hello")
    public String hello() {
        return "Hello";
    }

}
```

Open

```
localhost:8080/hello
```

Output

```
Hello
```

---

### Difference between Controller and RestController

`@Controller`

Returns HTML pages.

```java
@Controller
public class HomeController {

}
```

Used for JSP/Thymeleaf.

---

`@RestController`

Returns JSON.

```java
@RestController
public class UserController {

}
```

Output

```json
{
   "name":"John"
}
```

Most backend interviews use `@RestController`.

---

### NestJS Equivalent

```typescript
@Controller("users")
```

Almost identical.

---

### Interview Answer

> `@RestController` marks a class as a REST controller. It combines `@Controller` and `@ResponseBody`, so every method automatically returns JSON instead of rendering an HTML page.

---

# 3. What is `@RequestMapping`?

It defines the base URL for a controller or maps requests.

Example

```java
@RestController
@RequestMapping("/employees")
public class EmployeeController {

}
```

Now every API starts with

```
/employees
```

Example

```java
@GetMapping("/{id}")
```

Actual URL becomes

```
/employees/5
```

Another example

```java
@RequestMapping("/api/v1")
```

All APIs become

```
/api/v1/users
/api/v1/orders
```

You can also specify HTTP method

```java
@RequestMapping(
    value="/hello",
    method=RequestMethod.GET
)
```

Nowadays we usually use

```java
@GetMapping
@PostMapping
```

instead.

---

### Interview Answer

> `@RequestMapping` maps incoming HTTP requests to a controller or method. It is commonly used at the class level to define a base URL for all endpoints.

---

# 4. What is `@PostMapping`?

Used for HTTP POST requests.

Usually

* Create User
* Register
* Login
* Save Data

Example

```java
@PostMapping("/employees")
public Employee createEmployee() {

}
```

Client sends

```
POST /employees
```

Body

```json
{
   "name":"Rahul",
   "salary":50000
}
```

Spring receives it.

---

NestJS equivalent

```typescript
@Post()
```

Exactly the same.

---

### Interview Answer

> `@PostMapping` maps HTTP POST requests to a method. It is mainly used for creating new resources or submitting data to the server.

---

# 5. What is `@GetMapping`?

Handles GET requests.

Used for

* Fetch User
* Fetch Products
* Fetch Employees

Example

```java
@GetMapping("/employees")
public List<Employee> getEmployees() {

}
```

Client

```
GET /employees
```

Returns

```json
[
   {
      "id":1,
      "name":"Rahul"
   }
]
```

---

Example

```java
@GetMapping("/employees/{id}")
```

```
GET /employees/10
```

Returns one employee.

---

NestJS equivalent

```typescript
@Get()
```

---

### Interview Answer

> `@GetMapping` maps HTTP GET requests to a controller method. It is mainly used to retrieve resources from the server.

---

# 6. What is `@RequestBody`?

Suppose client sends

```json
{
   "name":"Rahul",
   "salary":45000
}
```

Spring has to convert JSON into a Java object.

That's what `@RequestBody` does.

Example

```java
@PostMapping("/employees")
public Employee createEmployee(
        @RequestBody Employee employee) {

    return employee;
}
```

JSON

↓

```json
{
   "name":"Rahul",
   "salary":45000
}
```

↓

Java Object

```java
Employee employee
```

Spring automatically performs this conversion using the Jackson library.

---

### Without RequestBody

Spring cannot read JSON from the request body.

---

NestJS equivalent

```typescript
@Post()
create(@Body() dto: CreateUserDto)
```

Exactly the same concept.

---

### Interview Answer

> `@RequestBody` binds the HTTP request body to a Java object. Spring automatically converts incoming JSON into the specified Java class.

---

# 7. What is `@PathVariable`?

Suppose URL

```
GET /employees/15
```

How do we access 15?

Use

```java
@PathVariable
```

Example

```java
@GetMapping("/employees/{id}")
public Employee getEmployee(
      @PathVariable int id) {

}
```

Spring extracts

```
15
```

and assigns

```java
id = 15
```

---

Another example

```
GET /users/3/orders/20
```

```java
@GetMapping("/users/{userId}/orders/{orderId}")
```

```java
public String getOrder(
    @PathVariable int userId,
    @PathVariable int orderId)
```

Spring gives

```
userId = 3
orderId = 20
```

---

NestJS equivalent

```typescript
@Get(":id")
findOne(@Param("id") id: string)
```

---

### Interview Answer

> `@PathVariable` extracts values from the URL path and binds them to method parameters. It is commonly used to identify a specific resource by its ID.

---

# 8. What is a DTO?

DTO stands for **Data Transfer Object**.

It is a simple class used to transfer data between the client and the server without exposing the internal entity.

Suppose the database entity is:

```java
public class Employee {

    private int id;
    private String name;
    private String password;
    private double salary;

}
```

When creating an employee, the client should not send an `id`, and you may not want to expose the `password`.

Instead, define a DTO:

```java
public class EmployeeDTO {

    private String name;
    private double salary;

}
```

Controller:

```java
@PostMapping("/employees")
public Employee createEmployee(
        @RequestBody EmployeeDTO dto) {

}
```

**Why use DTOs?**

* Hide sensitive fields (e.g., passwords).
* Validate input.
* Keep API models separate from database models.
* Make APIs easier to evolve without changing database entities.

**NestJS equivalent:**

```typescript
export class CreateEmployeeDto {
  name: string;
  salary: number;
}
```

### Interview Answer

> A DTO (Data Transfer Object) is an object used to transfer data between the client and the server. It helps hide internal entity details, supports validation, and keeps API models separate from persistence models.

---

# 9. What is a Service Layer?

The service layer contains the **business logic** of the application.

A common Spring Boot structure is:

```
Controller
    ↓
Service
    ↓
Repository
    ↓
Database
```

Example:

**Controller**

```java
@RestController
public class EmployeeController {

    @Autowired
    private EmployeeService employeeService;

    @PostMapping("/employees")
    public Employee createEmployee(@RequestBody EmployeeDTO dto) {
        return employeeService.createEmployee(dto);
    }
}
```

**Service**

```java
@Service
public class EmployeeService {

    public Employee createEmployee(EmployeeDTO dto) {
        // Business logic
        // Validate data
        // Convert DTO to Entity
        // Save to database
    }
}
```

The controller should focus on handling HTTP requests, while the service layer handles application logic.

**NestJS equivalent:**

```typescript
@Controller()
export class EmployeeController {
  constructor(private service: EmployeeService) {}
}
```

### Interview Answer

> The service layer contains the application's business logic. Controllers receive HTTP requests and delegate processing to services, which interact with repositories and perform validations, calculations, and other business operations.

---

# 10. What is Dependency Injection (DI)?

Dependency Injection is a design pattern where Spring creates and provides objects (called **beans**) instead of you creating them manually.

Without DI:

```java
public class EmployeeController {

    EmployeeService service = new EmployeeService();

}
```

This tightly couples the controller to a specific implementation.

With DI:

```java
@RestController
public class EmployeeController {

    @Autowired
    private EmployeeService service;

}
```

Or, preferably, using constructor injection:

```java
@RestController
public class EmployeeController {

    private final EmployeeService service;

    public EmployeeController(EmployeeService service) {
        this.service = service;
    }
}
```

Spring creates the `EmployeeService` bean and injects it into the controller.

**Benefits:**

* Loose coupling.
* Easier testing (you can inject mock implementations).
* Better maintainability.
* Spring manages the object lifecycle.

**NestJS equivalent:**

```typescript
constructor(private readonly service: EmployeeService) {}
```

The concept is essentially the same.

### Interview Answer

> Dependency Injection is a design pattern in which Spring creates and manages objects (beans) and injects them where needed. It reduces coupling, improves testability, and allows Spring to manage the lifecycle of application components.

---

## A simple request flow tying everything together

```text
Client
   │
   │ POST /employees
   │ JSON Body
   ▼
@RestController
   │
@PostMapping
   │
@RequestBody → EmployeeDTO
   │
Service Layer
   │
Repository
   │
Database
```

This end-to-end flow is one of the most common things interviewers expect you to understand for Spring Boot REST APIs.
