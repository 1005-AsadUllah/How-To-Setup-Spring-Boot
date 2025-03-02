# How to Setup Spring Boot Application

---

## 🚀 Step 01: Add Dependencies (Essential for Every Spring Boot Application)

### 1. 🌐 Spring Web

- **Purpose:** Build **web applications** using **Spring MVC** or **RESTful APIs**.
- **Key Features:**
  - Supports `@RestController`, `@RequestMapping`, `@GetMapping` annotations.
  - Enables **embedded servers** like **Tomcat** and **Jetty**.
- **Use Cases:** Creating **dynamic web pages** and **RESTful web services**.

### 2. 🚧 Spring Boot DevTools

- **Purpose:** Enhance the **development experience**.
- **Key Features:**
  - **Automatic restarts** and **live reloading**.
  - Disables **caching** during development for **faster feedback**.
- **Pro Tip:** Pair with **Thymeleaf** or **Spring MVC** for **live updates**.

### 3. 🛠️ Spring Configuration Processor

- **Purpose:** Generate **metadata** for **custom configuration properties**.
- **Key Benefits:**
  - 🎯 **IDE Assistance:** **Auto-complete**, **error checking**, and **inline documentation**.
  - 🔍 **Validation:** Ensures **type safety** of properties.
  - 📄 **Documentation:** Creates `META-INF/spring-configuration-metadata.json` for **property metadata**.
- **Best For:** Managing **custom settings** in **application.properties** or **application.yml**.

### 4. ✂️ Lombok

- **Purpose:** Reduce **boilerplate code** using **annotations**.
- **Key Features:**
  - Generates **getters**, **setters**, **constructors**, **toString**, **equals**, and **hashCode** methods.
  - Supports **Builder Pattern** with `@Builder` annotation.
- **Tip:** Helps keep **model classes** **clean** and **maintainable**.

### 5. 🛢️ Spring Data JPA

- **Purpose:** Simplify **database interactions** with **JPA**.
- **Key Features:**
  - Provides **CRUD** operations through **JpaRepository**.
  - Supports **query generation** and **custom queries** with `@Query`.
  - Manages **entity relationships** (`@OneToMany`, `@ManyToOne`, etc.).
- **Ideal For:** Building **repositories** and **managing persistence layers**.

### 6. 🗄️ SQL Driver (MySQL, PostgreSQL, H2, etc.)

- **Purpose:** Connect your **Spring Boot application** to a **database**.
- **Common Choices:**
  - 🐬 **MySQL:** `mysql-connector-java`
  - 🐘 **PostgreSQL:** `postgresql-driver`
  - 🏠 **H2** (for **in-memory databases**): `h2`

---

## 🚀 Step 02: Connect with Database

### 1. 🛠️ Configure Application Properties

#### Example (`application.properties`):->

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/your_database
spring.datasource.username=your_username
spring.datasource.password=your_password
spring.datasource.driverClassName=com.mysql.cj.jdbc.Driver

spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.database-platform=org.hibernate.dialect.MySQLDialect
```

#### Example (`application.yml`):->

```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/your_database
    username: your_username
    password: your_password
    driver-class-name: com.mysql.cj.jdbc.Driver

  jpa:
    hibernate:
      ddl-auto: update
    show-sql: true
    database-platform: org.hibernate.dialect.MySQLDialect
```

---

## 📂 Application Structure: Four Main Sections

### 1. 📦 **Entity Layer**

#### Purpose of Entity Layer:->

- Represents **database tables** as **Java objects**.
- Maps **class fields** to **table columns** using **JPA annotations**.

#### Example (`Note.java`):->

```java
import jakarta.persistence.*;
import lombok.Data;

@Entity
@Data
public class Note {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String title;
    private String content;
}
```

#### Best Practices for Controller Layer:->

- Use **Lombok** (`@Data`, `@Getter`, `@Setter`) to reduce **boilerplate code**.
- Define **relationships** using `@OneToMany`, `@ManyToOne`, etc.

---

### 2. 🛠️ **Repository Layer**

#### Purpose of Repository Layer:->

- Provides an **interface** to perform **CRUD operations** on **entities**.
- Utilizes **Spring Data JPA's JpaRepository** to **automatically generate queries**.

#### Example (`NoteRepository.java`):->

```java
import org.springframework.data.jpa.repository.JpaRepository;
import java.util.List;

public interface NoteRepository extends JpaRepository<Note, Long> {
    List<Note> findByTitleContaining(String keyword);
}
```

#### Pro Tips:->

- Define **custom queries** using **method names** or `@Query` annotation.
- Use **pagination** and **sorting** with **Pageable**.

---

### 3. 🧠 **Service Layer**

#### Purpose of Service Layer :->

- **Encapsulates business logic**.
- Acts as a **bridge** between the **repository** and **controller layers**.

#### Example (`NoteService.java`):->

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import java.util.List;

@Service
public class NoteService {
    @Autowired
    private NoteRepository noteRepository;

    public List<Note> getAllNotes() {
        return noteRepository.findAll();
    }

    public Note save(Note note) {
        return noteRepository.save(note);
    }

    public List<Note> searchNotes(String keyword) {
        return noteRepository.findByTitleContaining(keyword);
    }
}
```

#### Best Practices :->

- Use `@Transactional` for **database operations** that require **consistency**.
- Handle **exceptions** and **validation logic** in the **service layer**.

---

### 4. 🌐 **Controller Layer**

#### Purpose of Controller Layer:->

- Manages **incoming requests** and **maps** them to **services**.
- Returns **responses** to the **client**.

#### Example (`NoteController.java`):->

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;
import java.util.List;

@RestController
@RequestMapping("/notes")
public class NoteController {
    @Autowired
    private NoteService noteService;

    @GetMapping
    public List<Note> getAllNotes() {
        return noteService.getAllNotes();
    }

    @PostMapping
    public Note createNote(@RequestBody Note note) {
        return noteService.save(note);
    }

    @GetMapping("/search")
    public List<Note> searchNotes(@RequestParam String keyword) {
        return noteService.searchNotes(keyword);
    }
}
```

#### Best Practices:->

- Use `@RequestMapping` or `@GetMapping`, `@PostMapping` for **RESTful endpoints**.
- Implement **error handling** with `@ExceptionHandler` or `@ControllerAdvice`.

---
