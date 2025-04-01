# SOLID Principles [WORK IN PROGRESS 🚧]

> **📍 Location**: `Good-Practices/01-SOLID-Principles/01-SOLID-Introduction.md`
> **🔗 See Also**: [Good Practices Overview](../00-Good-Practices-Intro.md)

## Introduction

The **SOLID principles** are five object-oriented design guidelines that help developers create **maintainable, scalable, and robust software**, especially critical in game development with Unity where requirements evolve rapidly. Originally introduced by [Robert C. Martin](https://en.wikipedia.org/wiki/Robert_C._Martin) ("Uncle Bob"), these principles are battle-tested in Unity projects of all sizes.

### Why SOLID Matters in Unity?

1. **Scene Complexity**: games often have hundreds of GameObjects interacting – SOLID keeps this manageable.
1. **Team Collaboration**: clean architecture prevents merge conflicts and spaghetti code.
1. **Asset Reusability**: well-designed components can be reused across projects.
1. **Performance**: decoupled systems are easier to optimize.

---

## Key Concepts

| Principle       | Unity Analogy                  | Key Benefit for Developers |
| --------------- | ------------------------------ | -------------------------- |
| **🧩 SRP** | One script = one responsibility (e.g., `PlayerMovement` ≠ `PlayerHealth`) | Easier debugging |
| **🔓 OCP** | New enemy types without modifying `EnemyManager` | Safe modding support |
| **🔄 LSP** | Any `Weapon` subclass works in `AttackSystem` | Polymorphism done right |
| **✂️ ISP** | `IMovable` for movement vs `IDamageable` for combat | No empty methods |
| **📦 DIP** | `IInventoryService` instead of direct `Database` references | Easy testing |

---

## Common SOLID Violations in Unity

- **God Scripts**: Monobehaviours that handle input, physics, UI, and scoring (violates SRP).
- **Switch-Based Enemy Types**: using `enum` + `switch` instead of polymorphism (violates OCP/SP)

    ```csharp
    // Anti-pattern
    void UpdateEnemy(EnemyType type)
    {
      switch(type)
      {
        case EnemyType.Zombie: /* ... */ break;
        case EnemyType.Dragon: /* ... */ break;
      }
    }
    ```

- **MonoBehaviour Inheritance Chains**: deep inheritance like `Enemy > FlyingEnemy > Dragon` (often violates LSP).

---

## How to Use This Section

1. **For Beginners**: implement one principle at a time (start with SRP).
1. **For Pros**: combine SOLID with Unity-specific patterns like:
   - **Component-based design** (built into Unity)
   - **ScriptableObjects** for data-driven architecture
   - **EventSystem** for decoupling

---

## Additional Resources

- [[E-BOOK] Create a C# style guide: Write cleaner code that scales](https://web.archive.org/web/20240518091705/https://unity.com/resources/create-code-c-sharp-style-guide-e-book?ungated=true)
- [[YouTube] Unite Austin 2017 - S.O.L.I.D. Unity](https://www.youtube.com/watch?v=eIf3-aDTOOA)
- [[GitHub] Example: SOLID Unity Project](https://github.com/gr4ndsmurf/SOLID-Principles)

> 🎮 **Pro Tip**: In Unity, apply SOLID at the **script level** first, then expand to systems like AI or UI.

---

## Index

Navigate to each principle:

1. [Single Responsibility Principle (SRP)](02-SOLID-SRP.md)
2. [Open/Closed Principle (OCP)](03-SOLID-OCP.md)
3. [Liskov Substitution Principle (LSP)](04-SOLID-LSP.md)
4. [Interface Segregation Principle (ISP)](05-SOLID-ISP.md)
5. [Dependency Inversion Principle (DIP)](06-SOLID-DIP.md)
