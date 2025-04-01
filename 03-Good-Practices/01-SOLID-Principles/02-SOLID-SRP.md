# SOLID Principles - Single Responsabilty Principle (SRP)

> **📍 Location**: `Good-Practices/01-SOLID-Principles/02-SOLID-SRP.md`
> **🔗 See Also**: [SOLID Introduction](01-SOLID-Introduction.md)

## **Definition**

A class should have **only one reason to change**, meanint it should encapsulate **a single responsability or funcionality**.

---

## **Objective**

- **Decouple** funcionalities to minimize ripple effects from changes.
- Create **focused, testable units** of code.
- Facilitate **team collaboration** (different developers can work on different responsabilites).

---

## **Benefits**

| Benefit | Impact in Unity Projects |
| ------- | ------------------------ |
| 🛠️ **Easier Maintenance** | Changing movement logic won't break attack systems |
| 📖 **Improved Readability** | Scripts like `PlayerJump` clearly reveal their purpose |
| ♻️ **Reusability** | `HealthSystem` can be reused across enemies, players, NPCs |
| 🧪 **Simpler Testing** | Isolated components are easier to unit test |

---

## **Common Pitfalls**

- **Over-Splitting**: creating too many small classes (micro-classes) can lead to unnecessary complexity.
- **Misidentifying Responsibilites**: ensure that each responsibility os truly distinct and not artificially separated.
- **False SRP**: splitting code physically but keeping logical coupling (e.g., `PlayerMovement` requiring exact variable names from `PlayerInput`).

---

## **Additional Notes**

### 🧠 Mental Model

Think of SRP like **specialized workers** in a restaurant:

- **Chef** -> Only cooks.
- **Waiter** -> Only serves.
- **Cashier** -> Only handles payments.

In Unity terms:

- `PlayerMovement` -> Just moves.
- `PlayerHealth` -> Just manages HP.
- `PlayerAudio` -> Just handles sounds.

### ⚡ When to Apply

1. When a script exceeds **200-300 lines**.
1. When you need to change the same script for **unrelated reasons** (e.g., adjusting jump physics breaks UI updates).
1. When working with **team members** on the same GameObject.

---

## **Unity Example: SRP in Action**

### **Scenario**: Basic Player Controller

#### ❌ Before SRP

```csharp
public class PlayerController : MonoBehaviour {
    // Mixed responsibilities
    [SerializeField] private Slider healthBar;
    
    void Update() {
        // Input handling
        float moveX = Input.GetAxis("Horizontal");
        
        // Movement
        transform.Translate(moveX * speed * Time.deltaTime, 0, 0);
        
        // Health management
        if (Input.GetKeyDown(KeyCode.H)) 
            Heal(10);
        
        // UI update
        healthBar.value = currentHealth / maxHealth;
    }
}
```

#### ✅ After SRP

```csharp
// Responsibility 1: Movement
public class PlayerMovement : MonoBehaviour {
    public void Move(float direction) {
        transform.Translate(direction * speed * Time.deltaTime, 0, 0);
    }
}

// Responsibility 2: Health
public class PlayerHealth : MonoBehaviour {
    public void Heal(int amount) { /* ... */ }
}

// Responsibility 3: Input
public class PlayerInput : MonoBehaviour {
    void Update() {
        float moveX = Input.GetAxis("Horizontal");
        GetComponent<PlayerMovement>().Move(moveX);
    }
}

// Responsibility 4: UI
public class HealthUI : MonoBehaviour {
    [SerializeField] private Slider healthBar;
    public void UpdateUI(float health) => healthBar.value = health;
}
```

#### 🛠️ Implementation Notes

1. Communication Between Components:
    - Use `GetComponent<>()` for simple cases.
    - Better: use UnityEvents or a messaging system.

    ```csharp
    // In PlayerHealth:
    public UnityEvent<float> OnHealthChanged;

    // In HealthUI (assigned via inspector):
    [SerializeField] private PlayerHealth playerHealth;
    void Start() {
        playerHealth.OnHealthChanged.AddListener(UpdateUI);
    }
    ```

---

## **Benefits of SRP in Unity**

1. **Sager Hot-Reloading**: smaller scripts reloaded faster in Play Mode.
1. **Better Prefab Organization**:

    ```yaml
    PlayerPrefab/
    ├── PlayerMovement.cs
    ├── PlayerHealth.cs
    ├── PlayerInput.cs
    └── HealthUI.cs
    ```

1. **Easier Debugging:** isolates issues (e.g. movement bugs won't affect health systems).

---

## **Pro Tips**

1. **Naming Convention**: use `<Entity><Responsability>` (e.g., `EnemyPathfinding`, `DoorAnimation`).
1. **Editor Tools**: use the [Unity Component Filter](https://docs.unity3d.com/Manual/Searching.html) to quickly find specific behaviours.
1. **Refactoring Steps**:

    ```mermaid
    graph TD
      A[Identify Mixed Responsabilites] --> B[Create New Scripts]
    ```

---

[Next >>: Open/Close Principle](03-SOLID-OCP.md) // [<< Back: SOLID Introduction](01-SOLID-Introduction.md)
