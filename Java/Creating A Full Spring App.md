# Creating A Spring App 

### Step 1
- Ctrl + Shift + P (in vscode)
- Spring Initializr: Maven
  - 3.0.10
  - Java
  - com.groupid (example: com.example)
  - artifact id (name of project lowecase-spearate-with-dash)
  - Jar
  - 17
  - Select
    - Last Used (@will provide dependencies)
  - Press Enter
  - Navigate to where you want the project to be created


### Step 2
- open project
  - src
    - main
      - java
        - com
          - groupid
            - name of project (open integrated terminal at this location)
                - only if you have set up the bash script otherwise use the other commands to setup folder configuration.
```bash
springJF "nameOfModel"
```
```bash
mkdir controllers models repositories services
```
```bash
touch controllers/NameOfModelController.java models/NameOfModel.java repositories/NameOfModelRepository.java services/NameOfModelService.java
```
### Step 3: Dynamic Rendering and Creating the Views

- Three minor configuration streps before we can pass data from controller to the .jsp file.

  1. Create the `src/main/webapp`</mark> folder if it does not exist
  2. Create the [`src/main/webapp/WEB-INF`](./src/main/webapp/WEB-INF/index.jsp) folder
  3. Edit the `src/main/resources/applications.properties` file to contain the following code:
```
spring.mvc.view.prefix=/WEB-INF/
# Data Persistence
spring.datasource.url=jdbc:mysql://localhost:3306/dbSchemaName #CHANGE THIS
spring.datasource.username=root
# Change this if your braxton!!
spring.datasource.password=root
spring.jpa.hibernate.ddl-auto=update
# For Update and Delete method hidden inputs
spring.mvc.hiddenmethod.filter.enabled=true

```
- in our pom.xml file, add the following dependencies:

```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-tomcat</artifactId>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.apache.tomcat.embed</groupId>
            <artifactId>tomcat-embed-jasper</artifactId>
        </dependency>
        <dependency>
            <groupId>jakarta.servlet.jsp.jstl</groupId>
            <artifactId>jakarta.servlet.jsp.jstl-api</artifactId>
        </dependency>
        <dependency>
            <groupId>org.glassfish.web</groupId>
            <artifactId>jakarta.servlet.jsp.jstl</artifactId>
        </dependency>
        <dependency>
            <groupId>com.mysql</groupId>
            <artifactId>mysql-connector-j</artifactId>
            <scope>runtime</scope>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-validation</artifactId>
        </dependency>
        <dependency>
            <groupId>org.mindrot</groupId>
            <artifactId>jbcrypt</artifactId>
            <version>0.4</version>
        </dependency>
        <dependency>
            <groupId>org.webjars</groupId>
            <artifactId>webjars-locator</artifactId>
            <version>0.47</version>
        </dependency>
        <dependency>
            <groupId>org.webjars</groupId>
            <artifactId>bootstrap</artifactId>
            <version>5.3.1</version>
        </dependency>
```
### Step 4: Creating css file
    - src
      - main (📁)
        - resources (📁)
          - static (📁)
            - css (📁)
    - add Bootswatch css file and name it main.css
              - main.css (📃)

### Boilerplate
#### [HomeController.java]((./src/main/webapp/WEB-INF/index.jsp))

```html

<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib prefix = "c" uri = "http://java.sun.com/jsp/jstl/core" %>
<%@ taglib uri="http://java.sun.com/jsp/jstl/fmt" prefix="fmt"  %>
<%@ taglib prefix="form" uri="http://www.springframework.org/tags/form"%>
<%@ page isErrorPage="true" %>

<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Login</title>
    <link rel="stylesheet" href="/webjars/bootstrap/css/bootstrap.min.css">
    <link rel="stylesheet" href="/css/main.css">
    <script defer src="/webjars/bootstrap/js/bootstrap.min.js"></script>
</head>
<body>
    <div class="container-fluid container-sm mt-5">
        <div class="card border-0">
            <div class="card-body">
                <h1 class="display-1">Change the heading</h1>
                <h2 class="card-color-primary">Change Me!!!</h2>
            </div>
        </div>
        <div class="row">
            <div class="col-lg-6 col-sm-12 col-md-12">
                <div class="card shadow border-secondary p-4 mt-5">
                    <div class="card-body">
                        <h2 class="display-3 mb-3">Registration</h2>
                        <form:form action="/register" method="POST" modelAttribute="newUser">
                            <div class="form-floating mb-3">
                                <form:input class="form-control" path="userName" placeholder="userName"/>
                                <form:label class="form-label" path="userName">User Name</form:label>
                                <form:errors class="form-text text-warning" path="userName"></form:errors>
                            </div>
                            <div class="form-floating mb-3">
                                <form:input class="form-control" path="email" placeholder="email"/>
                                <form:label class="form-label" path="email">Email</form:label>
                                <form:errors class="form-text text-warning" path="email"/>
                            </div>
                            <div class="form-floating mb-3">
                                <form:input class="form-control" path="password" type="password" placeholder="password"/>
                                <form:label class="form-label" path="password">Password</form:label>
                                <form:errors class="form-text text-warning" path="password"/>
                            </div>
                            <div class="form-floating mb-3">
                                <form:input class="form-control" path="confirm" type="password" placeholder="Confirm Password"/>
                                <form:label class="form-label" path="confirm">Confirm Password</form:label>
                                <form:errors class="form-text text-warning" path="confirm"/>
                            </div>
                            <div class="d-flex justify-content-end">
                                <input class="btn btn-outline-secondary" type="submit" value="Register">
                            </div>
                        </form:form>
                    </div>
                </div>
            </div>
            <div class="col-lg-6 col-sm-12 col-md-12">
                <div class="card shadow border-secondary p-4 mt-5">
                    <div class="card-body">
                        <h2 class="display-3 mb-3">Login</h2>
                        <form:form action="/login" method="POST" modelAttribute="newLogin">
                            <div class="form-floating mb-3">
                                <form:input class="form-control" path="email" placeholder="email"/>
                                <form:label class="form-label" path="email">Email</form:label>
                                <form:errors class="form-text text-warning" path="email"></form:errors>
                            </div>
                            <div class="form-floating mb-3">
                                <form:input class="form-control" path="password" placeholder="password" type="password"/>
                                <form:label class="form-label" path="password">Password</form:label>
                                <form:errors class="form-text text-warning" path="password"/>
                            </div>
                            <div class="d-flex justify-content-end">
                                <input class="btn btn-outline-secondary" type="submit" value="Login">
                            </div>
                        </form:form>
                    </div>
                </div>
            </div>
        </div> 
    </div>
</body>
</html>


```


#### [HomeController.java]((./src/main/java/com/example/beltexam/controllers/HomeController.java))

--do `alt+ shift+ o` to pull in the imports for the code below
```java
package com.example.beltexam.controllers;

@Controller
public class HomeController {
    
    // Add once service is implemented:
    @Autowired
    UserService userService;
    
    @GetMapping("/")
    public String index(Model model) {
    
        // Bind empty User and LoginUser objects to the JSP
        // to capture the form input
        model.addAttribute("newUser", new User());
        model.addAttribute("newLogin", new LoginUser());
        return "index.jsp";
    }
    
    @PostMapping("/register")
    public String register(@Valid @ModelAttribute("newUser") User newUser, 
            BindingResult result, Model model, HttpSession session) {
        
        // TO-DO Later -- call a register method in the service 
        User user = userService.register(newUser, result);
        // to do some extra validations and create a new user!
        
        if(result.hasErrors()) {
            // Be sure to send in the empty LoginUser before 
            // re-rendering the page.
            model.addAttribute("newLogin", new LoginUser());
            return "index.jsp";
        }
        
        // No errors! 
        // TO-DO Later: Store their ID from the DB in session, 
        session.setAttribute("user_id", user.getId());
        // in other words, log them in.
    
        return "redirect:/home";
    }
    
    @PostMapping("/login")
    public String login(@Valid @ModelAttribute("newLogin") LoginUser newLogin, 
            BindingResult result, Model model, HttpSession session) {
        
        // Add once service is implemented:
        User user = userService.login(newLogin, result);
    
        if(result.hasErrors()) {
            model.addAttribute("newUser", new User());
            return "index.jsp";
        }
        session.setAttribute("user_id", user.getId());
    
        // No errors! 
        // TO-DO Later: Store their ID from the DB in session, 
        // in other words, log them in.
    
        return "redirect:/home";
    }

    @GetMapping("/home")
    public String homePage(Model model, HttpSession session) {

        if(session.getAttribute("user_id")==null) {
            return "redirect:/";
        } else {
            User user = userService.getUserById((Long) session.getAttribute("user_id"));
            model.addAttribute("user", user);
            return "home.jsp";
        }
    }

    @GetMapping("/logout")
    public String logout(HttpSession session){
        session.invalidate();
        return "redirect:/";
    }
    
}

```
#### [User.java]((./src/main/java/com/example/beltexam/models/User.java))

--do `alt+ shift+ o` to pull in the imports for the code below

```java
package com.example.beltexam.models;

@Entity
@Table(name = "users")
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @NotEmpty(message = "Username is required!")
    @Size(min = 3, max = 30, message = "Username must be between 3 and 30 characters")
    private String userName;

    @NotEmpty(message = "Email is required!")
    @Email(message = "Please enter a valid email!")
    private String email;

    @NotEmpty(message = "Password is required!")
    @Size(min = 8, max = 128, message = "Password must be between 8 and 128 characters")
    private String password;

    @Transient
    @NotEmpty(message = "Confirm Password is required!")
    @Size(min = 8, max = 128, message = "Confirm Password must be between 8 and 128 characters")
    private String confirm;

    // This will not allow the createdAt column to be updated after creation
    @Column(updatable = false)
    @DateTimeFormat(pattern = "yyyy-MM-dd")
    private Date createdAt;
    @DateTimeFormat(pattern = "yyyy-MM-dd")
    private Date updatedAt;

    public User() {
    }

    public User(Long id, String userName, String email, String password, String confirm, Date createdAt,
            Date updatedAt) {
        this.id = id;
        this.userName = userName;
        this.email = email;
        this.password = password;
        this.confirm = confirm;
        this.createdAt = createdAt;
        this.updatedAt = updatedAt;
    }

    public Long getId() {
        return this.id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getUserName() {
        return this.userName;
    }

    public void setUserName(String userName) {
        this.userName = userName;
    }

    public String getEmail() {
        return this.email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    public String getPassword() {
        return this.password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    public String getConfirm() {
        return this.confirm;
    }

    public void setConfirm(String confirm) {
        this.confirm = confirm;
    }

    public Date getCreatedAt() {
        return this.createdAt;
    }

    public void setCreatedAt(Date createdAt) {
        this.createdAt = createdAt;
    }

    public Date getUpdatedAt() {
        return this.updatedAt;
    }

    public void setUpdatedAt(Date updatedAt) {
        this.updatedAt = updatedAt;
    }

    // other getters and setters removed for brevity
    @PrePersist
    protected void onCreate() {
        this.createdAt = new Date();
    }

    @PreUpdate
    protected void onUpdate() {
        this.updatedAt = new Date();
    }

}

```
#### [LoginUser.java]((./src/main/java/com/example/beltexam/models/LoginUser.java))

--do `alt+ shift+ o` to pull in the imports for the code below
```java
package com.example.beltexam.models;

public class LoginUser {
    
    @NotEmpty(message="Email is required!")
    @Email(message="Please enter a valid email!")
    private String email;
    
    @NotEmpty(message="Password is required!")
    @Size(min=8, max=128, message="Password must be between 8 and 128 characters")
    private String password;
    
    public LoginUser() {}


    public String getEmail() {
        return this.email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    public String getPassword() {
        return this.password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

}
```

#### [UserRepository.java]((./src/main/java/com/example/beltexam/repositories/UserRepository.java))
--do `alt+ shift+ o` to pull in the imports for the code below

```java
package com.example.beltexam.repositories;

@Repository
public interface UserRepository extends CrudRepository<User, Long> {
    
    Optional<User> findByEmail(String email);
    
}
```



#### [LoginUser.java]((./src/main/java/com/example/beltexam/services/UserService.java))
--do `alt+ shift+ o` to pull in the imports for the code below
```java
package com.example.beltexam.services;

@Service
public class UserService {
    
    @Autowired
    UserRepository userRepository;
    
    // TO-DO: Write register and login methods!
    public User register(User newUser, BindingResult result) {
        
        // TO-DO - Reject values or register if no errors:
        if(userRepository.findByEmail(newUser.getEmail()).isPresent()) {
            result.rejectValue("email", "Unique", "Email is already in use!");
        }
        // Reject if email is taken (present in database)
        // Reject if password doesn't match confirmation
        if (!newUser.getPassword().equals(newUser.getConfirm())) { // Check to make sure password matches confirm
            // password
            result.rejectValue("confirm", "Matches", "The Confirm Password must match Password!");
        }
        
        // Return null if result has errors
        if(result.hasErrors()) {
            return null;
        }
        String hashed = BCrypt.hashpw(newUser.getPassword(), BCrypt.gensalt());
        newUser.setPassword(hashed);
        // Hash and set password, save user to database
        return userRepository.save(newUser);
    }

    public User login(LoginUser newLoginObject, BindingResult result) {

        Optional<User> user = userRepository.findByEmail(newLoginObject.getEmail());

        if (!user.isPresent()) {
            result.rejectValue("email", "Unique", "Email not found bub");
        } else if (!BCrypt.checkpw(newLoginObject.getPassword(), user.get().getPassword())) {
            result.rejectValue("password", "Unique", "Password no good bub...");
        }

        if(result.hasErrors()) {
            return null;
        }

        return user.get();
    }

    public User getUserById(Long attribute) {
        return userRepository.findById(attribute).orElse(null);
    }
}
```
### Final Step: to run the application
```bash
mvn clean spring-boot:run
```
or 
```bash
mvn spring-boot:run
```



