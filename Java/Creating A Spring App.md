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
    - Spring Boot DevTools
    - Spring Web
  - Press Enter
  - Navigate to where you want the project to be created


### Step 2
- open project
  - src
    - main
      - java
        - com
          - groupid
            - name of project
              - here we write controller

- in controller file we need @RestController and @RequestMapping("") imported
  
```java
    //example SomethingController.java
    package com.groupid.name-of-project;

    import org.springframework.web.bind.annotation.RequestMapping;
    import org.springframework.web.bind.annotation.RestController;

    @RestController
    public class SomethingController {
        @RequestMapping("")
        public String test(){
            return "Test";
        }
        @RequestMapping("/daikichi")
        public String daikichi(){
            return "Daikichi stuff goes here";
        }
        
    }

```

- to run the project use the following command

```bash
mvn clean spring-boot:run
```

### (JSP) and JSTL

- create a folder in main called webapp(folder name lower-case)
- inside webapp write the jsp file (example demo.jsp)
- in our pom.xml file, add the following dependencies:

```xml
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
```

-add this to jsp file (in this example we have bootstrap link)
```html
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8" %>
<!-- New line below to use the JSP Standard Tag Library -->
<%@ taglib prefix = "c" uri = "http://java.sun.com/jsp/jstl/core"%>
    <!DOCTYPE html>
    <html>

    <head>
        <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.1/dist/css/bootstrap.min.css" rel="stylesheet"
            integrity="sha384-4bw+/aepP/YC94hEpVNVgiZdgIC5+VKNBQNGCHeKRQN+PtmoHDEXuppvnDJzQIu9" crossorigin="anonymous">
        <meta charset="UTF-8">
        <title>Demo JSP</title>
    </head>

    <body >
        <h1 class="text-center">Hello World</h1>
        <h2>Fruit of the day: </h2>
        <h3><c:out value="${fruit}"/></h3>
    </body>

    </html>
```
- in our project-name folder
- our TestController.java will look like this: 
- we import @Controller @GetMapping("")  

```java
package com.immanuel.daikichiroutes;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class TestController {
    
    @GetMapping("/")
    public String demo(Model model){
        model.addAttribute("fruit", "banana");
        return "index.jsp";
    }
}

```
#### Dynamic Rendering
- Three minor configuration streps before we can pass data from controller to the .jsp file.

  1. Create the `src/main/webapp`</mark> folder if it does not exist
  2. Create the `src/main/webapp/WEB-INF` folder
  3. Edit the `src/main/resources/applications.properties` file to contain the following code:
   ```
   spring.mvc.view.prefix=/WEB-INF/
   ```



