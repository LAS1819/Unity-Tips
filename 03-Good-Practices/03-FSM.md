# Finite State Machine (FSM) in Unity with C#

A **Finite State Machine (FSM)** is a design pattern used to manage the behavior of an object by dividing it into a finite number of states. Each state defines a specific behaviour, and transitions between states are triggered by events or conditions. FSMs are widely used in game development to manage logic, AI, animations, and more.

This document explains the basics of FSMs and how to implement in Unity using C#.

---

## **What is a Finite State Machine?**

A finite State Machine consists of:
1. **States**: the different behaviors or bodes an object can be in (e.g., `Idle`, `Walking`, `Running`).
2. **Transitions**: the rules that define how the object moves from one state to another (e.g., "When the player presses the jump button, transition from `Walking` to `Jumping`").
3. **Actions**: the behavior or logic executed when entering, exiting, or staying in a state.

---

## **Why use an FSM?**

- **Modularity**: each state is dependent, making it easier to add, remove, or modify behaviors.
- **Readability**: the code is organized and easier to understand.
- **Maintainability**: changes to one state do not affect others.
- **Predictability**: the object can only be in one state at a time, avoiding unexpected behavior.

---

## **Implementing an FSM in Unity**

### **1. Define the States**
First, define the possible states as an `enum`. For example:

```csharp
public enum GameState
{
    MainMenu,
    Playing,
    Paused,
    GameOver
}
```

---

### **2. Create a State Machine**
Create a class to manage the states and transitions. This Class will handle the current state and execute actions when entering or exiting states.

```csharp
using System;

public class StateMachine
{
    public GameState CurrentState { get; private set; }

    // Change the current state and execute actions
    public void ChangeState(GameState newState, Action onEnter = null, Action onExit = null)
    {
        if (CurrentState == newState) return; // Avoid redundant changes

        // Execute the exit action of the current state
        onExit?.Invoke();

        // Change to the new state
        CurrentState = newState;

        // Execute the enter action of the new state
        onEnter?.Invoke();
    }
}
```

---

### **3. Use the State Machine in a MonoBehaviour
Integrate the state machine into a `MonoBehaviour` (e.g., `GameManager`) to manage the game's bahavior.

```csharp
using UnityEngine;

public class GameManager : MonoBehaviour
{
    private StateMachine _stateMachine;

    private void Awake()
    {
        // Initialize the state machine
        _stateMachine = new StateMachine();

        // Set the initial state
        _stateMachine.ChangeState(GameState.MainMenu, OnEnterMainMenu, OnExitMainMenu);
    }

    // Define actions for each state
    private void OnEnterMainMenu()
    {
        Debug.Log("Entering MainMenu state");
        // Show the main menu UI
    }

    private void OnExitMainMenu()
    {
        Debug.Log("Exiting MainMenu state");
        // Hide the main menu UI
    }

    private void OnEnterPlaying()
    {
        Debug.Log("Entering Playing state");
        // Start the game logic
    }

    private void OnExitPlaying()
    {
        Debug.Log("Exiting Playing state");
        // Stop the game logic
    }

    // Example: Change state to Playing
    public void StartGame()
    {
        _stateMachine.ChangeState(GameState.Playing, OnEnterPlaying, OnExitPlaying);
    }
}
```

---

### **4. Handle Transitions**
Transitions between states cab be triggered by events, user input, or conditions. For example:

```csharp
private void Update()
{
    if (_stateMachine.CurrentState == GameState.Playing && Input.GetKeyDown(KeyCode.Escape))
    {
        _stateMachine.ChangeState(GameState.Paused, OnEnterPaused, OnExitPaused)
    }
}
```

---

### **5. Benefits of Using an FSM in Unity**

- **Clear Separation of Concerns**: each state encapsulates its own behavior.
- **Easy Debugging**: you can log state changes to track the flow of the game.
- **Scalability**: adding new states or modifying existing ones is straightforward.
- **Reusability**: the state machine can be reused across different objects or scenes.

---

### **Example: Player Controller with FSM**
Here's an example of how to use an FSM to manage a player's behavior:

```csharp
public enum PlayerState
{
    Idle,
    Walking,
    Running,
    Jumping
}

public class PlayerController : MonoBehaviour
{
    private StateMachine _stateMachine;

    private void Awake()
    {
        _stateMachine = new StateMachine();
        _stateMachine.ChangeState(PlayerState.Idle, OnEnterIdle, OnExitIdle);
    }

    private void Update()
    {
        if (Input.GetKeyDown(KeyCode.Space) && _stateMachine.CurrentState != PlayerState.Jumping))
        {
            _stateMachine.ChangeState(PlayerState.Jumping, OnEnterJumping, OnExitJumping);
        }
    }

    private void OnEnterIdle()
    {
        Debug.Log("Entering Idle state");
        // Play idle animation
    }

    private void OnExitIdle()
    {
        Debug.Log("Exiting Idle state");
    }

    private void OnEnterJumping()
    {
        Debug.Log("Entering Jumping state");
        // Apply jump force
    }

    private void OnExitJumping()
    {
        Debug.Log("Exiting Jumping state");
    }
}
```

---

### **Best Practices**

1. **Keep States Simple**: each state should handle a single behavior.
2. **Avoid Hardcoding Transitions**: use events or conditions to trigger transitions.
3. **Use Delegates for Actions**: pass `Actions` delegates to `ChangeState` for flexibility.
4. **Log State Changes**: use Debug.Log to track state transitions during development.
5. **Reuse the State Machine**: create a generic `StateMachine` class that can be reused across different objects.

---

### **Conclusion**
A Finite State Machine (FSM) is a powerful tool for managing complex behaviors in Unity. By dividing logic into state and transitions, you can create modular, maintainable, and scalable code. Whether you're managing game states, AI behavior, or player actions, an FSM can help you organize your code and avoid spaghetti logic.

For more advanced use cases, consider exploring Hierarchial State Machines or State Desgin Patterns.

### **References**

- [Unity Manual: State Machines](https://docs.unity3d.com/Manual/StateMachineBasics.html)
- [Game Programming Patterns: State](https://gameprogrammingpatterns.com/state.html)