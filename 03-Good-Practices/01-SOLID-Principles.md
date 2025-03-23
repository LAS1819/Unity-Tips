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

- **Additional Notes**
  - **Real-World Analogy**: think of SRP as a team where each member has a specific role. For example, a chef cooks, a waiter serves, and a cashier handles payments. If one person tries to do everything, the system becomes inefficient and error-prone.
  - **When to apply**: use SRP when you notice that a class is handling multiple tasks or when changes to one funcionality risk breaking another.

---

### 2. Open/Closed Principle (OCP)

- **Definition**: software entities (classes, modules, funcions, etc.) should be open for extension but closed for modification. This means that you shuld be able to add new functionality without changing existing code.
- **Objective**: to make the system more maintanable and scalable by allowing new features to be added with a minimal risk of introducing bugs in existing code.
- **Benefits**
  - **Maintainability**: existing code doesn't beed to be modified, reducing the risk of introducing bugs.
  - **Scalability**: new functionality can be added easily by creating new classes.
  - **Reusability**: the existing codebase remains reusable and stable.
  - **Testability**: since existing code isn't modified, existing tests don't need to be updates.
- **Common Pitfalls**
  - **Over-Engineering**: creating too many abstractions can make the code harder to understand.
  - **Misidentifying Extension Points**: not all parts of the system need to be open for extension. Focus on areas that are likey to change.
  - **Tight Coupling**: if new extensions require changes to existing code, the principle is violated.
- **Additional Notes**
  - **Real-World Analogy**: think of OCP like a smartphone. You can extend its funcionality by installing new apps (extensions) without modifying the phone's operating system (closed for modification).
  - **When to apply**: use OCP when you anticipate that certain parts of your system will need to change or grow over time, sucha as adding new shapes, enemies, or behaviours in a game.

#### Unity Example: OCP in Action

Imagine your're developing a game with different types of enemies. Instead of modifiying the existing enemy logic every time you add a new enemy type, you can use OCP to make the system extensible.

- **Unity Implementation**

  1. **Define an Interface for Enemies**:

  ```csharp
  public interface IEnemy
  {
    void Attack();
  }
  ```

  1. **Create Specific Enemy Types**:

  ```csharp
  public class Zombie : MonoBehaviour, IEnemy
  {
    public void Attack()
    {
      Debug.Log("Zombie attacks with a bite!");
    }
  }

  public class Skeleton() : MonoBehaviour, IEnemy
  {
    public void Attack()
    {
      Debug.Log("Skeleton attacks with a sword!");
    }
  }

  public class Dragon() : MonoBehaviour, IEnemy
  {
    public void Attack()
    {
      Debug.Log("Dragon attacks with fire breath!");
    }
  }
  ```

  1. **Use the Enemies in a Game Manager**:

  ```csharp
  public class GameManager : MonoBehaviour
  {
    private List<IEnemy> _enemies = new List<IEnemy>();

    void Start()
    {
      _enemies.Add(new Zombie());
      _enemies.Add(new Skeleton());
      _enemies.Add(new Dragon());

      foreach (var enemy in _enemies)
      {
        enemy.Attack();
      }
    }      
  }
  ```

- **Benefits of OCP in Unity**
  - **Extensibiltiy**: you can add new enemy types (e.g., Dragon, Goblin) without modifiying the GameManager or existing enemy classes.
  - **Maintainability**: existing enemy logic remains untouched, reducing the risk of bugs.
  - **Scalability**: the system can grow easily as new requirements arise.

---

### 3. Liskov Substitution Principle (LSP)

- **Definition**: derived classes must be substitueble for ther base classes without altering the correctess of the program. In other words, objects of a superclass should be replaceable with objects of a subclass without affecting the functionality of the program.
- **Objective**: to ensure that inheritance is used correctyly, maintaining the expected behavior of the base class in all derived classes.
- **Benefits**
  - **Consistency**: ensures that derived classes adhere to the contract defined by the base class.
  - **Reusability**: promotes code reuse through inheritance while maintaining reliablity.
  - **Maintainability**: reduces the risk of introducing bugs when extending funcionality.
- **Common Pitfalls**
  - **Violating the Base Class Contract**: derived classes that change the expected behavior of the base class.
  - **Overriding Methods Incorrectly**: modifying the behavior of overridden methods in a way that breaks the base class's logic.
  - **Adding Unsupported Features**: introducing new methods or properties in derived classes that the base class doesn't support.
  
- **Additional Notes**
  - **Real-World Analogy**: think of LSP like a power socket. You can plug in any device (derived class) that adheres to the socket's interface (base class), and it should work without causing issues. If a device doesn't fit or behaves unexpectedly, it violates the principle.
  - **When to apply**: use LSP whenever you use inheritance. It's especially important in large systems where derived classes must seamlessly replace base classes.

#### Unity Example: LSP in Action

Imagine you're developing a game with differents types of charactes. Each character can move and attack, but they do so in different ways. You want to ensure that any character can be substituted for another without breaking the game.

- **Unity Implementation**
  1. Define a Base Class for Characters:
  
  ```csharp
  public abstract class Character : MonoBehaviour
  {
      public abstract void Move();
      public abstract void Attack();
  }
  ```

  1. Create Derived Classes:

  ```csharp
  public class Knight : Character
  {
      public override void Move()
      {
        Debug.Log("Knight moves forward");
      }

      public override void Attack()
      {
        Debug.Log("Knight swings a sword.");
      }
  }

  public class Archer : Character
  {
      public override void Move()
      {
        Debug.Log("Archer moves stealthily");
      }

      public override void Attack()
      {
        Debug.Log("Archer shoots an arrow.");
      }
  }
  ```

  1. Use Characters in a Game Manager:

  ```csharp
  public class GameManager : MonoBehaviour
  {
    private List<Character> _characters = new List<Characters>();

    void Start()
    {
        _characters.Add(new Knight());
        _characters.Add(new Archer());

        foreach (var character in _characters)
        {
            character.Move();
            character.Attack();
        }
    }
  }
  ```

  1. Add a New Character Without Breaking the System:

  ```csharp
  public class Mage : Character
  {
    public override void Move()
    {
        Debug.Log("Mage teleports.");
    }

    public override void Attack()
    {
        Debug.Log("Mage casts a spell.");
    }
  }
  ```

- **Benefits of LSP in Unity**
  - **Interchangeability**: you can replace any character with another (e.g., `Knight`, `Archer`, `Mage`) without breaking the game logic.
  - **Extensibility**: adding new character types is easy and doesn't requiere changes to existing code.
  - **Reliability**: ensures that all characters behave as expected, maintaining the game's integrity.

---

### 4. Interface Segregation Principle (ISP)

- **Definition**: customers should not depend on interfaces that they do not use.
- **Objective**: 
- **Benefits**
  
- **Common Pitfalls**
  
- **Additional Notes**
  - **Real-World Analogy**:
  - **When to apply**:

#### Unity Example: ISP in Action

[Add example description]

- **Unity Implementation**


- **Benefits of ISP in Unity**

- **C# Native Example**:

---

### 5. Dependency Inversion Principle (DIP)

- **Definition**: rely on abstractions, not concrete implementations.
- **Objective**: 
- **Benefits**
  
- **Common Pitfalls**
  
- **Additional Notes**
  - **Real-World Analogy**:
  - **When to apply**:

#### Unity Example: DIP in Action

[Add example description]

- **Unity Implementation**


- **Benefits of DIP in Unity**

- **C# Native Example**:

---

## Additional Resources

- [Link to Official Documentation](#)
