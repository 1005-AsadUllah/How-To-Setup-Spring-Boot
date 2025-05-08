# 📌 About Annotations in Spring Boot

## 1. **@Controller Annotation**

- **Purpose:**
  `@Controller` is a Spring MVC annotation used to define a class as a **web controller**, which can handle **HTTP requests**.

- **Details:**
  It is typically used in conjunction with `@RequestMapping` to map web requests to specific handler methods.

- **Behavior:**
  Returns a **view name** (e.g., a Thymeleaf or JSP page) instead of raw data.

- **📘 Example:**

  ```java
  import org.springframework.stereotype.Controller;
  import org.springframework.web.bind.annotation.GetMapping;

  @Controller
  public class HomeController {

      @GetMapping("/")
      public String home() {
          return "index"; // returns "index.html" or "index.jsp" from templates/views folder
      }
  }
  ```

- ✅ Use `@Controller` when you want to **render HTML pages** via templates (like Thymeleaf, JSP).

---

## 2. **@ResponseBody Annotation**

- **Purpose:**
  `@ResponseBody` tells Spring **not to return a view**, but instead to return **data (like JSON or plain text)** directly in the HTTP response body.

- **Use case:**
  Used when you want to **send data** (not HTML pages) to the client—like in REST APIs.

---

- **📘 Example:**

```java
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class ApiController {

    @GetMapping("/message")
    @ResponseBody
    public String message() {
        return "Hello from Spring Boot!";
    }
}
```

✅ Output:

```output
Hello from Spring Boot!
```

🔸 Without `@ResponseBody`, Spring would think `"Hello from Spring Boot!"` is a **view name** like an HTML file.
🔸 With `@ResponseBody`, it **sends that string directly** in the HTTP response.

---

### 🧠 Bonus Tip :

- When you use `@RestController` instead of `@Controller`, **Spring automatically adds `@ResponseBody` to every method**.

---

## 3. **@RestController Annotation**

- **Purpose:**
  `@RestController` is a special version of `@Controller` **used to build RESTful web services**. It tells Spring to **return data (like JSON or XML)** directly in the HTTP response, not views (HTML).

- **Behind the scenes:**
  It combines:

  ```java
  @Controller + @ResponseBody
  ```

---

### 📘 Example:

```java
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class UserController {

    @GetMapping("/user")
    public String getUser() {
        return "Asad Ullah";
    }
}
```

✅ Output in browser:

```out
Asad Ullah
```

🔹 No HTML page is rendered.
🔹 Just plain data (String, JSON, etc.) is returned to the client.

---

### 🧠 When to Use:

- Use `@RestController` when you’re building **APIs** that return **JSON/XML data**, not web pages.
- For example, in mobile apps, React/Angular frontends, or third-party services.

---

## 4. **@GetMapping Annotation**

- **Purpose:**
  `@GetMapping` is used to **map HTTP GET requests** to a specific method in a controller.

- **GET request?**
  A GET request is what happens when you visit a URL in your browser. It’s used to **retrieve data** (not submit it).

---

### 🧩 Part of Spring Web MVC

It's a shortcut for:

```java
@RequestMapping(method = RequestMethod.GET)
```

---

### 📘 Example:

```java
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HelloController {

    @GetMapping("/hello")
    public String sayHello() {
        return "Hello, Asad!";
    }
}
```

✅ When you open `http://localhost:8080/hello` in your browser, you’ll see:

```out
Hello, Asad!
```

---

### 🧠 Notes:

- `@GetMapping("/path")` → maps a specific URL to a method.
- It's used in **REST APIs** or **controller methods** that return data or render a page (when used with `@Controller`).

---

## 5. **@PostMapping Annotation**

- **Purpose:**
  Handles **HTTP POST** requests — usually used to **submit data or Create data** (like a form or JSON object) to the server.

- **Example:**

  ```java
  @RestController
  public class UserController {

      @PostMapping("/user")
      public String createUser(@RequestBody String name) {
          return "User " + name + " created!";
      }
  }
  ```

  ✅ If you send a POST request with `"Asad"` in the body, it responds:

  ```out
  User Asad created!
  ```

---

## 6. **@PutMapping Annotation**

- **Purpose:**
  Handles **HTTP PUT** requests — typically used to **update** an existing resource.

- **Example:**

  ```java
  @RestController
  public class UserController {

      @PutMapping("/user/{id}")
      public String updateUser(@PathVariable int id, @RequestBody String name) {
          return "User with ID " + id + " updated to " + name;
      }
  }
  ```

  ✅ A PUT request to `/user/1` with `"Asad Updated"` in the body returns:

  ```out
  User with ID 1 updated to Asad Updated
  ```

---

## 7. **@DeleteMapping Annotation**

- **Purpose:**
  Handles **HTTP DELETE** requests — used to **delete** a resource.

- **Example:**

  ```java
  @RestController
  public class UserController {

      @DeleteMapping("/user/{id}")
      public String deleteUser(@PathVariable int id) {
          return "User with ID " + id + " deleted";
      }
  }
  ```

  ✅ A DELETE request to `/user/1` returns:

  ```out
  User with ID 1 deleted
  ```

---

### 🔁 Summary Table

| Annotation       | HTTP Method | Common Use  |
| ---------------- | ----------- | ----------- |
| `@GetMapping`    | GET         | Read data   |
| `@PostMapping`   | POST        | Create data |
| `@PutMapping`    | PUT         | Update data |
| `@DeleteMapping` | DELETE      | Delete data |

---

Great! Let’s dive into `@PathVariable`, a very useful Spring annotation for handling dynamic values in URLs.

---

## 8. **@PathVariable Annotation**

- **Purpose:**
  `@PathVariable` is used to **extract values from the URL path** and bind them to method parameters.

- This allows you to make **dynamic URLs**, like `/user/1`, `/user/2`, etc.

---

### 📘 Example:

```java
import org.springframework.web.bind.annotation.*;

@RestController
public class UserController {

    @GetMapping("/user/{id}")
    public String getUser(@PathVariable int id) {
        return "User ID: " + id;
    }
}
```

✅ When you visit:

```out
http://localhost:8080/user/5
```

You’ll get:

```out
User ID: 5
```

---

### 🔍 How It Works:

- The `{id}` in the URL is a **placeholder**.
- `@PathVariable int id` takes that value from the URL and gives it to the method.

---

## 9. **@RequestBody Annotation**

- **Purpose:**
  `@RequestBody` is used to **bind the HTTP request body** (usually in JSON format) to a **Java object** or **primitive type**.

- Commonly used in **REST APIs** when the client sends data as JSON.

---

### 📘 Example: Accepting a JSON request

Suppose the client sends this JSON:

```json
{
  "name": "Asad",
  "email": "asad@example.com"
}
```

Here’s the Java code to map it:

```java
@RestController
public class UserController {

    @PostMapping("/user")
    public String createUser(@RequestBody User user) {
        return "User created: " + user.getName();
    }
}

// Java class to map JSON
class User {
    private String name;
    private String email;

    // Getters and Setters (required)
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }

    public String getEmail() { return email; }
    public void setEmail(String email) { this.email = email; }
}
```

✅ Output if JSON is sent:

```out
User created: Asad
```

---

**🧠 Key Points:**

- The class used with '@RequestBody' must have 'getters and setters'.

- `@RequestBody` reads JSON (or XML, if configured) from the **request body** and converts it to a Java object using **Jackson** (by default).

- Spring Boot uses Jackson (by default) to convert JSON to Java objects and vice versa.

- @RequestBody is commonly used in APIs that receive complex input data.

---
