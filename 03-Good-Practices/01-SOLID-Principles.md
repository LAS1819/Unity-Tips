# SOLID Principles [WORK IN PROGRESS 🚧]

## Introduction

The SOLID principles are a set of five object-oriented design guidelines that help create more maintainable, scalable and robust software. They were introduced by [Robert C. Martin](https://es.wikipedia.org/wiki/Robert_C._Martin) (also known as Uncle Bob) and are widely used in software development.

---

## Key Concepts

The following is a breakdown of each of the SOLID principles.

### 1. Single Responsabilty Principle (SRP)

- **Definition**: a class should have only one reason to change, meaning it should have only one responsibility.
- **Objective**: to simplify design and facilitate maintenance by ensuring that each class focuses on a single task.
- **C# Example**:

    ```csharp
    // Incorrect: one class with multiple responsibilites
    public class User
    {
        public void AddUser() { /* Logic for adding a user */ }
        public void SendEmail() { /* Logic for sending an email */ }
    }

    // Correct: separate responsibilites into different classes
    public class User
    {
        public void AddUser() { /* Logic for adding a user */ }
    }

    public class EmailService
    {
        public void SendEmail() { /* Logic for sending an email */ }
    }
    ```

- **Benefits**:

  - **Easier Maintenance**: changes to one resposibility do not affect other functionalities.
  - **Improved Readability**: classes are smaller and more focused.
  - **Reusability** single-responsibility classes are easier to reuse in other parts of the application.

- **Common Pitfalls**
  - **Over-Splitting**: creating too many small classes can lerad to unnecessary complexity.
  - **Misidentifying Responsibilites**: ensure that each responsibility os truly distinct and not artificially separated.

#### **Additional Notes**

- **Real-World Analogy**: think of SRP as a team where each member has a specific role. For example, a chef cooks, a waiter serves, and a cashier handles payments. If one person tries to do everything, the system becomes inefficient and error-prone.
- **When to apply**: use SRP when you notice that a class is handling multiple tasks or when changes to one funcionality risk breaking another.

---

### 2. Open/Closed Principle (OCP)

- **Definition**:
- **Objective**:
- **C# Example**:

    ```csharp
    ```

---

## Additional Resources

- [Link to Official Documentation](#)