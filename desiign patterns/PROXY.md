# Proxy Design Pattern in Java

## 1. What is the Proxy Design Pattern?

Proxy is a **structural design pattern** that provides a substitute or representative object to control access to another object.

The proxy receives the request first and decides whether or how to forward it to the actual object.

### Real-time example: Accessing a secure document

Imagine an application where users can access confidential documents.

Before showing the document, we need to check whether the user is authorized.

Instead of adding the authorization check inside every client, we can create a proxy that checks access before calling the actual document service.

## 2. Java Implementation

### Step 1: Create an interface

```java
interface Document {
    void display();
}
```

### Step 2: Create the real object

```java
class RealDocument implements Document {

    private String fileName;

    public RealDocument(String fileName) {
        this.fileName = fileName;
        loadFromDatabase();
    }

    private void loadFromDatabase() {
        System.out.println("Loading document: " + fileName);
    }

    @Override
    public void display() {
        System.out.println("Displaying document: " + fileName);
    }
}
```

### Step 3: Create the Proxy class

```java
class DocumentProxy implements Document {

    private RealDocument realDocument;
    private String fileName;
    private boolean authorized;

    public DocumentProxy(String fileName, boolean authorized) {
        this.fileName = fileName;
        this.authorized = authorized;
    }

    @Override
    public void display() {

        if (!authorized) {
            System.out.println("Access denied!");
            return;
        }

        if (realDocument == null) {
            realDocument = new RealDocument(fileName);
        }

        realDocument.display();
    }
}
```

### Step 4: Main class

```java
public class Main {

    public static void main(String[] args) {

        Document document =
                new DocumentProxy("salary-report.pdf", false);

        document.display();

        Document authorizedDocument =
                new DocumentProxy("salary-report.pdf", true);

        authorizedDocument.display();
    }
}
```

### Output

```text
Access denied!
Loading document: salary-report.pdf
Displaying document: salary-report.pdf
```

## 3. How does it work?

1. The client calls `display()` on the proxy.
2. The proxy checks whether the user is authorized.
3. If access is denied, the real object is not called.
4. If access is allowed, the proxy creates the real object when needed.
5. The proxy forwards the request to the real object.

### Flow

```text
          Client
             |
             v
       DocumentProxy
             |
       Check Authorization
             |
       -----------------
       |               |
    Denied           Allowed
       |               |
   Access Denied       v
                 RealDocument
                       |
                       v
                Display Document
```

## 4. Types of Proxy

1. **Virtual Proxy:** Creates an expensive object only when it is required.
2. **Protection Proxy:** Controls access based on permissions.
3. **Remote Proxy:** Represents an object that exists in another process or location.
4. **Caching Proxy:** Stores results to avoid repeated expensive operations.

## 5. Proxy in Spring Boot

Spring uses proxies in several features, including:
- `@Transactional`
- Method-level security
- Some caching operations using `@Cacheable`

For example:

```java
@Transactional
public void transferMoney() {
    // Business logic
}
```

Spring can create a proxy around the service bean to manage transaction behavior when the method is invoked through the proxy.

Important: Proxy-based Spring features have limitations. For example, self-invocation within the same object usually bypasses the Spring proxy.

## 6. Advantages

- Controls access to the actual object.
- Supports authorization and security checks.
- Can add lazy initialization and caching.
- Keeps additional access logic separate from business logic.

## 7. Disadvantages

- Adds another class or layer.
- Can make debugging and execution flow more complex.

## 8. Common Interview Questions

**Q1. What is the Proxy Design Pattern?**

It provides a substitute object that controls access to the real object.

**Q2. Give a real-time example.**

A document proxy checks user authorization before allowing access to a confidential document.

**Q3. Where is Proxy used in Spring Boot?**

Spring uses proxies for features such as transaction management, method security and caching.

**Q4. What is the difference between Proxy and Adapter?**

Proxy controls access to an object while keeping the same interface. Adapter changes or translates an interface so incompatible classes can work together.

## 9. Quick Revision

- Proxy is a structural design pattern.
- It provides a substitute for the real object.
- It controls access to the real object.
- Common uses include security, lazy loading, remote access and caching.
- Spring uses proxies for features such as `@Transactional`.

### Interview-ready answer

"Proxy is a structural design pattern that provides a substitute object to control access to the actual object. For example, before displaying a confidential document, a proxy can check whether the user is authorized. If access is allowed, it forwards the request to the actual document object. Spring also uses proxies for transaction management and security."
