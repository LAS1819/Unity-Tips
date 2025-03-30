# SOLID Principles - Open/Closed Principle (OCP)

> **📍 Location**: `Good-Practices/01-SOLID-Principles/03-SOLID-OCP.md`
> **🔗 See Also**: [SOLID Introduction](01-SOLID-Introduction.md)

## **Definition**

Software entities should be:

- **Open for extension**: new behavior can be added via new code.
- **Closed for modification**: existing code remains unchanged.

---

## **Objective**

To make the system more maintanable and scalable by allowing new features to be added with a minimal risk of introducing bugs in existing code.

- **Future-proof** your code against new requeriments.
- **Minimize regression risks** when adding features.
- Enable **modding support** through extensible systems.

---

## **Benefits**

| Benefit | Impact in Unity Projects |
| ------- | ------------------------ |
| 🛡️ **Stability** | Core systems remain untouched during expansions |
| 🧩 **Modularity** | New enemy types, weapons, or abilities plug in seamlessly |
| ♻️ **Efficiency** | Less time spent refactoring existing code |

---

## **Common Pitfalls**

- **Over-Engineering**: creating too many abstractions can make the code harder to understand.
- **Misidentifying Extension Points**: not all parts of the system need to be open for extension. Focus on areas that are likey to change.
- **Tight Coupling**: if new extensions require changes to existing code, the principle is violated.

---

## **Additional Notes**

### 🧠 Mental Model

Think of OCP like Unity's Particle System:

- **Closed**: the core particle engine doesn't change.
- **Open**: you can create infinite new effects by tweaking parameters and modules.

### ⚡ When to Apply

1. When you anticipate **frequent additions** (enemies, skills, UI elements).
1. For **core systems** that multiple teams members work on.
1. When using **ScriptableObjects** for data-driven design.

---

## Unity Example: OCP in Action

### 🎮 **Scenario**: Weapon System

#### ❌ Before OCP

```csharp
public class Weapon : MonoBehaviour
{
    public enum WeaponType { Sword, Gun }
    public WeaponType type;
    
    void Attack()
    {
        switch(type)
        {
            case WeaponType.Sword: SwingSword(); break;
            case WeaponType.Gun: FireGun(); break;
        }
    }
    // Must edit this class to add new weapons
}
```

#### ✅ After OCP

1. **Define the Abstraction**

    ```csharp
    public interface IWeapon
    {
        void Attack();
    }
    ```

1. **Implement Concrete Weapons**

    ```csharp
    // Sword.cs
    public class Sword : MonoBehaviour, IWeapon
    {
        public void Attack()
        {
            Debug.Log("Sword slash!");
            PlaySwingAnimation();
        }
    }

    // Gun.cs
    public class Gun : MonoBehaviour, IWeapon
    {
        public void Attack()
        {
            Debug.Log("Gun shot!");
            FireBullet();
        }
    }

    // New weapon type (added without changing existing code)
    public class Grenade : MonoBehaviour, IWeapon
    {
        public void Attack()
        {
            Debug.Log("Grenade throw!");
            SpawnExplosion();
        }
    }
    ```

1. **Usage**

    ```csharp
    public class PlayerCombat : MonoBehaviour
    {
        private IWeapon _currentWeapon;

        public void EquipWeapon(IWeapon weapon)
        {
            _currentWeapon = weapon;
        }

        void Update()
        {
            if (Input.GetMouseButtonDown(0))
                _currentWeapon?.Attack();
        }
    }
    ```

#### 🛠️ Implementation Notes

1. **ScriptableObject Integration**:

    ```csharp
    // WeaponSO.cs
    public abstract class WeaponSO : ScriptableObject
    {
        public abstract void Attack();
    }

    // Usage: create assets for each weapon type
    ```

1. **UnityEvent Alternative**:

    ```csharp
    public class WeaponController : MonoBehaviour
    {
        public UnityEvent onAttack;
        void Attack() ==> onAttack.Invoke();
    }
    ```

---

## **Benefits of OCP in Unity**

1. **Content Pipeline**: designers can add new weapons/enemies without programmer intervention.
1. **Assets Reuse**: existing VFX/SFX systems work with new weapon types automatically.
1. **Safer Updates**: mods or DLCs can extend systems without touching core code.

---

## **Pro Tips**

1. **Use ScriptableObjects** for data-driven extensibility:

    ```csharp
    // Create new weapons as assets
    [CreateAssetMenu]
    public class MagicStaffSO : WeaponSO
    {
        public override void Attack()
        {
            // Staff-specific logic
        }
    }
    ```

1. **Editor Tools**: automate creation of new type templates with [MenuItem] attributes.
1. **Component-Based Approach**:

    ```yaml
    Player/
    ├── WeaponSlot (holds IWeapon)
    └── Weapon_Base (implements IWeapon)
        ├── Weapon_Sword
        └── Weapon_Gun
    ```

---

[Next >>: Liskov Substitution Principle (LSP)](04-SOLID-LSP.md) // [<< Back: Single Responsibility Principle (SRP)](02-SOLID-SRP.md)
