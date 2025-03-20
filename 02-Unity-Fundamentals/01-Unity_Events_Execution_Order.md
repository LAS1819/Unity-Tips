# Unity Event Execution Order

Understanding the execution order of Unity events is crucial for writing robust and predictable code. This document outlines the most common Unity events and their execution order, along with best practices for using them.

---

## **Execution Order of Unity Events**

1. **`Awake`**:
   - Called **once** when the `GameObject` is created or loaded.
   - Ideal for initializing variables or setting up references before any other logic runs.
   - **Note**: If the `GameObject` is inactive, `Awake` is not called until it is activated.

2. **`OnEnable`**:
   - Called every time the `GameObject` is activated (either at the start or after being deactivated).
   - Ideal for subscribing to events or resetting states when the object is reactivated.

3. **`Start`**:
   - Called **once** just before the first `Update`, as long as the `GameObject` is active.
   - Ideal for initializing values that depend on other objects or components.

4. **`FixedUpdate`**:
   - Called at fixed time intervals (default: every 0.02 seconds).
   - Ideal for physics or any logic that requires consistent updates independent of framerate.

5. **`Update`**:
   - Called once per frame. The frequency depends on the game's framerate.
   - Ideal for game logic that needs to be updated continuously.

6. **`LateUpdate`**:
   - Called after `Update`, once per frame.
   - Ideal for calculations that depend on actions performed in `Update` (e.g., camera follow logic).

7. **`OnDisable`**:
   - Called every time the `GameObject` is deactivated.
   - Ideal for unsubscribing from events or cleaning up resources when the object is no longer active.

8. **`OnDestroy`**:
   - Called when the `GameObject` is destroyed.
   - Ideal for releasing resources or cleaning up references before the object is removed.

---

## **Scene Management Events**

### **`SceneManager.sceneLoaded`**

This event is triggered when a new scene has finished loading. It is part of Unity's `SceneManagement` API and is useful for performing actions after a scene is fully loaded.

#### **When is it called?**

- The `sceneLoaded` event is called **after** the new scene's `Awake` methods but **before** its `Start` methods.
- It is triggered when you load a scene using `SceneManager.LoadScene` or `SceneManager.LoadSceneAsync`.

#### **How to use it?**

You need to subscribe to the event using `SceneManager.sceneLoaded`. Here's an example:

```csharp
using UnityEngine;
using UnityEngine.SceneManagemenet;
public class SceneLoaderExample : MonoBehaviour
{
    private void OnEnable()
    {
        // Subscribe to the sceneLoaded event
        SceneManager.sceneLoaded += OnSceneLoaded;
    }

    private void OnDisable()
    {
        // Unsubscribe to avoid memory leaks
        SceneManager.sceneLoaded -= OnSceneLoaded;
    }

    private void OnSceneLoaded(Scene scene, LoadSceneMode mode)
    {
        Debug.Log($"Scene {scene.name} has been loaded with mode {mode}");
        
        // Perform actions after the scene is loaded
        InitializeSceneObjects();
    }

    private void InitializeSceneObjects()
    {
        // Example: Find and initialize objects in the new scene
        GameObject player = GameObject.Find("Player");
        if (player != null)
        {
            Debug.Log("Player found and initialized.");
        }
    }
}
```

##### **Important Notes**:

- **Subscription Timing**: always subscribe to `sceneLoaded` in `OnEnable` and unsuscribe in `OnDisable` to avoid memory leaks.
- **Execution Order**: since `sceneLoaded` is called after `Awake` but before `Start`, it's a good place to initialize objects that depend on the scene being fully loaded.
- **Avoid Duplicate Subscriptions**: ensure you don't subscribe to the event multiple times (e.g., if the scripts is enabled/disabled repeatedly).

---

## **Complete Execution Order (Summary)**

1. **`Awake`** → 2. **`OnEnable`** → 3. **`Start`** → 4. **`FixedUpdate`** → 5. **`Update`** → 6. **`LateUpdate`** → 7. **`OnDisable`** → 8. **`OnDestroy`**

---

## **Practical Example**

Imagine a `GameObject` with a script that is activated and deactivated multiple times during the game. Here's the execution order:

1. **First Activation**:
   - `Awake` → `OnEnable` → `Start`

2. **Normal Cycle (while active)**:
   - `FixedUpdate` → `Update` → `LateUpdate` (repeated every frame).

3. **Deactivation**:
   - `OnDisable`

4. **Reactivating**:
   - `OnEnable` → `FixedUpdate` → `Update` → `LateUpdate` (repeated every frame).

5. **Destruction**:
   - `OnDisable` → `OnDestroy`

---

## **Other Important Events**

In addition to the main events, Unity has other useful events:

- **`OnApplicationPause`**: Called when the game is paused (e.g., when the app is minimized).
- **`OnApplicationQuit`**: Called just before the game exits.
- **`OnTriggerEnter` / `OnCollisionEnter`**: Physics events triggered by collisions or triggers.
- **`OnMouseDown` / `OnMouseOver`**: Events related to mouse interaction.

---

## **Summary Table**

| Order | Event                | Description                                                                 |
|-------|----------------------|-----------------------------------------------------------------------------|
| 1     | `Awake`              | Initialization before any other method.                                     |
| 2     | `OnEnable`           | When the object is activated (also after being deactivated).                |
| 3     | `Start`              | Initialization before the first `Update`.                                   |
| 4     | `FixedUpdate`        | Update at fixed intervals (ideal for physics).                              |
| 5     | `Update`             | Update once per frame (ideal for game logic).                               |
| 6     | `LateUpdate`         | Update after `Update` (ideal for cameras or final actions).                 |
| 7     | `OnDisable`          | When the object is deactivated.                                             |
| 8     | `OnDestroy`          | When the object is destroyed.                                               |

---

## **Best Practices**

- Use **`Awake`** for initializing variables and setting up references.
- Use **`Start`** for initializations that depend on other objects or components.
- Use **`OnEnable` and `OnDisable`** to manage event subscriptions.
- Use **`FixedUpdate`** for physics and **`Update`** for game logic.
- Use **`OnDestroy`** to clean up resources or remove references.
- Use **`SceneManager.sceneLoaded`** to perform actions after a scene is fully loaded.

---

## **Example Script**

Here’s an example script that logs the execution order of events:

```csharp
using UnityEngine;

public class ExampleScript : MonoBehaviour
{
    private void Awake()
    {
        Debug.Log("Awake");
    }

    private void OnEnable()
    {
        Debug.Log("OnEnable");
    }

    private void Start()
    {
        Debug.Log("Start");
    }

    private void FixedUpdate()
    {
        Debug.Log("FixedUpdate");
    }

    private void Update()
    {
        Debug.Log("Update");
    }

    private void LateUpdate()
    {
        Debug.Log("LateUpdate");
    }

    private void OnDisable()
    {
        Debug.Log("OnDisable");
    }

    private void OnDestroy()
    {
        Debug.Log("OnDestroy");
    }
}
```

If you run this script, you'll see the order of messsages in the console.

For more information, refer to the Unity Documentation.
