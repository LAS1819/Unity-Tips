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