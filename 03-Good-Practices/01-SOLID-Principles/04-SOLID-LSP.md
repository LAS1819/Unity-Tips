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