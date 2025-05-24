# SharpRL Documentation

## Table of Contents

-   [Introduction](#introduction)
-   [Classes](#classes)
    -   [Agent](#agent)
    -   [Configuration](#configuration)
    -   [Environment](#environment)
    -   [IAgent](#iagent)
    -   [IEnvironment](#ienvironment)
    -   [NoState](#nostate)
    -   [StatelessAgent](#statelessagent)
-   [Learning](#learning)
    -   [IActionSelectionPolicy](#iactionselectionpolicy)
    -   [ILearningAlgorithm](#ilearningalgorithm)
    -   [IQValueTable](#iqvaluetable)
    -   [QValue](#qvalue)
    -   [QValueTable](#qvaluetable)
    -   [Algorithms](#algorithms)
        -   [QLearning](#qlearning)
        -   [Sarsa](#sarsa)
    -   [SelectionPolicy](#selectionpolicy)
        -   [DecayHelpers](#decayhelpers)
        -   [EGreedy](#egreedy)
        -   [PolicyHelpers](#policyhelpers)
        -   [Softmax](#softmax)
-   [Util](#util)
    -   [NormalRandomGenerator](#normalrandomgenerator)

## Introduction

This document provides detailed information about the SharpRL library, including its classes, methods, and properties.

## Classes

### Agent

```csharp
public abstract class Agent<TEnvironment, TAgent, TState> : IAgent<TEnvironment, TAgent, TState>
    where TEnvironment : IEnvironment<TEnvironment, TAgent, TState>
    where TAgent : IAgent<TEnvironment, TAgent, TState>
```

Represents a base implementation of the Agent interface.

**Type Parameters:**

*   `TEnvironment`: The type of the environment.
*   `TAgent`: The type of the current agent.
*   `TState`: The type of the state.

**Properties:**

*   `State`: Gets or sets the current state of the agent.
*   `PrevState`: Gets the previous state of the agent.
*   `Learner`: Gets the learning algorithm used by the agent.

**Methods:**

*   `Initialize(TEnvironment environment)`: Initializes the agent with the given environment.
    *   `environment`: The environment that the agent is added to.
*   `Reset(int episode)`: Resets the agent to the given episode number.
    *   `episode`: The episode number.
*   `Update(int timeStep, int episode)`: Updates the agent at the given time step.
    *   `timeStep`: The current time step.
    *   `episode`: The current episode number.
*   `Reward(double reward, bool isTerminal, int episode)`: Rewards the agent after the agent has been updated.
    *   `reward`: The reward given by the environment.
    *   `isTerminal`: Indicates whether the new state is terminal.
    *   `episode`: The current episode number.

### Configuration

```csharp
public sealed class Configuration
```

Contains the configuration for the system.

**Properties:**

*   `MaxEpisodes`: Gets the maximum number of episodes.
*   `Random`: Gets the random number generator.

**Methods:**

*   `Configuration(int maxEpisodes, Random random = null)`: Initializes a new instance of the <[`Configuration`](relative/SharpRL/Configuration.cs:12)> class.
    *   `maxEpisodes`: The maximum number of episodes.
    *   `random`: The random number generator.
*   `Add(string key, object value)`: Adds a key-value pair to the configuration.
    *   `key`: The key to add.
    *   `value`: The value to add.
*   `ContainsKey(string key)`: Determines whether the configuration contains the specified key.
    *   `key`: The key to check.
*   `Get<T>(string key)`: Gets the value associated with the specified key.
    *   `T`: The type of the value to retrieve.
    *   `key`: The key of the value to get.
    *   `Returns`: The value associated with the specified key, or the default value for the type `T` if the key is not found.

### Environment

```csharp
public abstract class Environment<TEnvironment, TAgent, TState> : IEnvironment<TEnvironment, TAgent, TState>
    where TEnvironment : IEnvironment<TEnvironment, TAgent, TState>
    where TAgent : IAgent<TEnvironment, TAgent, TState>
```

Represents a base implementation of the IEnvironment interface.

**Type Parameters:**

*   `TEnvironment`: The type of the environment.
*   `TAgent`: The type of the agent.
*   `TState`: The type of the state.

**Properties:**

*   `Config`: Gets the configuration for the environment.

**Methods:**

*   `Environment(Configuration config)`: Initializes a new instance of the <[`Environment`](relative/SharpRL/Environment.cs:15)> class.
    *   `config`: The configuration for the environment.
*   `GetDefaultState(TAgent agent)`: Gets the default state for the given agent.
    *   `agent`: The agent to get the default state for.
    *   `Returns`: The default state for the agent.
*   `AddAgent(TAgent agent)`: Adds an agent to the environment.
    *   `agent`: The agent to add.
*   `Initialize()`: Initializes the environment.
*   `Reset(int episode)`: Resets the environment for the given episode.
    *   `episode`: The episode number.
*   `Update(int episode)`: Updates the environment.
    *   `episode`: The episode number.
    *   `Returns`: `true` if the current episode is over; otherwise, `false`.
*   `Execute(TAgent agent, int actionNumber, int episode)`: Executes the given action for the given agent.
    *   `agent`: The agent to execute the action for.
    *   `actionNumber`: The index of the action to execute.
    *   `episode`: The episode number.

### IAgent

```csharp
public interface IAgent<TEnvironment, TAgent, TState>
    where TEnvironment : IEnvironment<TEnvironment, TAgent, TState>
    where TAgent : IAgent<TEnvironment, TAgent, TState>
```

Represents an agent in the reinforcement learning environment.

**Type Parameters:**

*   `TEnvironment`: The type of the environment.
*   `TAgent`: The type of the agent.
*   `TState`: The type of the state.

**Properties:**

*   `State`: Gets or sets the current state of the agent.

**Methods:**

*   `Initialize(TEnvironment environment)`: Initializes the agent with the given environment.
    *   `environment`: The environment that the agent is added to.
*   `Reset(int episode)`: Resets the agent to the given episode number.
    *   `episode`: The episode number.
*   `Update(int timeStep, int episode)`: Updates the agent at the given time step.
    *   `timeStep`: The current time step.
    *   `episode`: The current episode number.
*   `Reward(double reward, bool isTerminal, int episode)`: Rewards the agent after the agent has been updated.
    *   `reward`: The reward given by the environment.
    *   `isTerminal`: Indicates whether the new state is terminal.
    *   `episode`: The current episode number.

### IEnvironment

```csharp
public interface IEnvironment<TEnvironment, TAgent, TState>
    where TEnvironment : IEnvironment<TEnvironment, TAgent, TState>
    where TAgent : IAgent<TEnvironment, TAgent, TState>
```

Represents an environment in which an agent can learn.

**Type Parameters:**

*   `TEnvironment`: The type of the environment.
*   `TAgent`: The type of the agent.
*   `TState`: The type of the state.

**Properties:**

*   `Config`: Gets the configuration of the environment.

**Methods:**

*   `AddAgent(TAgent agent)`: Adds an agent to the environment.
    *   `agent`: The agent to add.
*   `Initialize()`: Initializes the environment.
*   `GetDefaultState(TAgent agent)`: Gets the default state for the given agent.
    *   `agent`: The agent.
    *   `Returns`: The default state for the agent.
*   `Reset(int episode)`: Resets the environment to the given episode.
    *   `episode`: The episode number.
*   `Update(int episode)`: Updates the environment.
    *   `episode`: The episode number.
    *   `Returns`: `true` if the current episode is over; otherwise, `false`.
*   `Execute(TAgent agent, int action, int episode)`: Executes the given action for the given agent.
    *   `agent`: The agent.
    *   `action`: The index of the action to execute.
    *   `episode`: The current episode number.

### NoState

```csharp
public struct EmptyState
{
}
```

Represents an empty state used for Stateless agents.

### StatelessAgent

```csharp
public sealed class StatelessAgent<TEnvironment> : IAgent<TEnvironment, StatelessAgent<TEnvironment>, EmptyState>
    where TEnvironment : IEnvironment<TEnvironment, StatelessAgent<TEnvironment>, EmptyState>
```

Represents an agent without state.

**Type Parameters:**

*   `TEnvironment`: The type of the environment.

**Properties:**

*   `Learner`: Gets the learning algorithm used by the agent.

**Methods:**

*   `StatelessAgent(Func<TEnvironment, ILearningAlgorithm<EmptyState>> createLearnerFn)`: Initializes a new instance of the <[`StatelessAgent`](relative/SharpRL/StatelessAgent.cs:14)> class.
    *   `createLearnerFn`: The function that creates the learner.
*   `Initialize(TEnvironment environment)`: Initializes the agent with the given environment.
    *   `environment`: The environment that the agent is added to.
*   `Reset(int episode)`: Resets the agent to the given episode number.
    *   `episode`: The episode number.
*   `Update(int timeStep, int episode)`: Updates the agent at the given time step.
    *   `timeStep`: The current time step.
    *   `episode`: The current episode number.
*   `Reward(double reward, bool isTerminal, int episode)`: Rewards the agent after the agent has been updated.
    *   `reward`: The reward given by the environment.
    *   `isTerminal`: Indicates whether the new state is terminal.
    *   `episode`: The current episode number.

## Learning

### IActionSelectionPolicy

```csharp
public interface IActionSelectionPolicy
```

Represents an action selection policy.

**Methods:**

*   `Update(int episode)`: Updates the selection policy.
    *   `episode`: The episode number.
*   `Select(QValue value)`: Selects an action from the given Q-value.
    *   `value`: The Q-value.
    *   `Returns`: The index of the action to execute.

### ILearningAlgorithm

```csharp
public interface ILearningAlgorithm<TState>
```

Represents a learning algorithm.

**Type Parameters:**

*   `TState`: The type of the state.

**Properties:**

*   `FollowPolicy`: Indicates whether the learner should follow the policy.

**Methods:**

*   `SelectAction(TState state)`: Returns the action to execute in the given state.
    *   `state`: The state.
    *   `Returns`: The action to execute.
*   `Update(TState currentState, TState newState, int action, double reward)`: Updates the learning algorithm.
    *   `currentState`: The current state.
    *   `newState`: The new state.
    *   `action`: The action that was executed.
    *   `reward`: The reward that was received.
*   `AfterEpisode(int episode)`: Called after the current episode is done.
    *   `episode`: The episode number.

### IQValueTable

```csharp
public interface IQValueTable<TState>
```

Represents a Q-value table.

**Type Parameters:**

*   `TState`: The type of the state.

**Properties:**

*   `this[TState state, int actionNumber]`: Sets the Q-value for the given action in the given state.
*   `this[TState state]`: Returns the Q-values for the given state.

**Methods:**

*   `Contains(TState state)`: Indicates whether the table contains the given state.
    *   `state`: The state.
    *   `Returns`: `true` if the table contains the given state; otherwise, `false`.

### QValue

```csharp
public struct QValue
```

Represents a Q-value.

**Properties:**

*   `Count`: Gets the number of values in the Q-value.

**Methods:**

*   `this[int index]`: Gets or sets the value at the given index.
    *   `index`: The index of the value to get or set.

### QValueTable

```csharp
public sealed class QValueTable<TState> : IQValueTable<TState>
```

Represents a Q-value table.

**Type Parameters:**

*   `TState`: The type of the state.

**Constructors:**

*   `QValueTable(int numActions, double defaultValue = 0)`: Initializes a new instance of the <[`QValueTable`](relative/SharpRL/Learning/QValueTable.cs:13)> class.
    *   `numActions`: The number of actions.
    *   `defaultValue`: The default value for entries in the table.

**Methods:**

*   `this[TState state, int action]`: Gets or sets the Q-value for the given action in the given state.
    *   `state`: The state.
    *   `action`: The index of the action.
*   `this[TState state]`: Gets the Q-value for the given state.
    *   `state`: The state.
*   `Contains(TState state)`: Indicates whether the table contains the given state.
    *   `state`: The state.
    *   `Returns`: `true` if the table contains the given state; otherwise, `false`.

### Algorithms

#### QLearning

```csharp
public sealed class QLearning<TState> : ILearningAlgorithm<TState>
```

Represents the Q-learning algorithm.

**Type Parameters:**

*   `TState`: The type of the state.

**Constructors:**

*   `QLearning(QValueTable<TState> qValueTable, IActionSelectionPolicy selectionPolicy, double alpha, double gamma, Random random)`: Initializes a new instance of the <[`QLearning`](relative/SharpRL/Learning/Algorithms/QLearning.cs:14)> class.
    *   `qValueTable`: The Q-value table.
    *   `selectionPolicy`: The selection policy.
    *   `alpha`: The learning rate.
    *   `gamma`: The discount factor.
    *   `random`: The random generator.

**Properties:**

*   `FollowPolicy`: Indicates whether the learner should follow the policy.

**Methods:**

*   `SelectAction(TState state)`: Returns the action to execute in the given state.
    *   `state`: The state.
    *   `Returns`: The action to execute.
*   `Update(TState currentState, TState newState, int action, double reward)`: Updates the learning algorithm.
    *   `currentState`: The current state.
    *   `newState`: The new state.
    *   `action`: The action that was executed.
    *   `reward`: The reward that was received.
*   `AfterEpisode(int episode)`: Called after the current episode is done.
    *   `episode`: The episode number.

#### Sarsa

```csharp
public sealed class Sarsa<TState> : ILearningAlgorithm<TState>
```

Represents the SARSA algorithm.

**Type Parameters:**

*   `TState`: The type of the state.

**Constructors:**

*   `Sarsa(QValueTable<TState> qValueTable, IActionSelectionPolicy selectionPolicy, double alpha, double gamma, Random random)`: Initializes a new instance of the <[`Sarsa`](relative/SharpRL/Learning/Algorithms/Sarsa.cs:14)> class.
    *   `qValueTable`: The Q-value table.
    *   `selectionPolicy`: The selection policy.
    *   `alpha`: The learning rate.
    *   `gamma`: The discount factor.
    *   `random`: The random generator.

**Properties:**

*   `FollowPolicy`: Indicates whether the learner should follow the policy.

**Methods:**

*   `SelectAction(TState state)`: Returns the action to execute in the given state.
    *   `state`: The state.
    *   `Returns`: The action to execute.
*   `Update(TState currentState, TState newState, int action, double reward)`: Updates the learning algorithm.
    *   `currentState`: The current state.
    *   `newState`: The new state.
    *   `action`: The action that was executed.
    *   `reward`: The reward that was received.
*   `AfterEpisode(int episode)`: Called after the current episode is done.
    *   `episode`: The episode number.

### SelectionPolicy

#### DecayHelpers

```csharp
public static class DecayHelpers
```

Contains decay functions.

**Methods:**

*   `ConstantDecay(int startEpisode, int stopEpisode, double startValue, double stopValue)`: A constant decay function.
    *   `startEpisode`: The episode to start decaying.
    *   `stopEpisode`: The episode to stop decaying. At this episode, value = stopValue.
    *   `startValue`: The start value.
    *   `stopValue`: The stop value.
    *   `Returns`: The decay function.

#### EGreedy

```csharp
public sealed class EGreedy : IActionSelectionPolicy
```

Represents an E-greedy action selection policy.

**Constructors:**

*   `EGreedy(double epsilon, Random random, Decay decayFn = null)`: Initializes a new instance of the <[`EGreedy`](relative/SharpRL/Learning/SelectionPolicy/EGreedy.cs:14)> class.
    *   `epsilon`: The probability that a random action will be selected.
    *   `random`: The random generator.
    *   `decayFn`: The decay function for epsilon. If null, epsilon is not decayed.

**Properties:**

*   `Epsilon`: Gets the epsilon.

**Methods:**

*   `Update(int episode)`: Updates the selection policy.
    *   `episode`: The episode number.
*   `Select(QValue value)`: Selects an action from the given Q-value.
    *   `value`: The Q-value.
    *   `Returns`: The index of the action to execute.

#### PolicyHelpers

```csharp
public static class PolicyHelpers
```

Contains helper methods for selection policies.

**Methods:**

*   `SelectMax(QValue value, Random random)`: Selects the best action from the given Q-value. If there are more than one best value, a random one is choosen uniformly.
    *   `value`: The Q-value.
    *   `random`: The random generator.
    *   `Returns`: The index of the action.
*   `SelectRandom(QValue value, Random random)`: Selects a random action.
    *   `value`: The Q-value.
    *   `random`: The random generator.
    *   `Returns`: The index of the action.

#### Softmax

```csharp
public sealed class Softmax : IActionSelectionPolicy
```

Represents a Softmax action selection policy.

**Constructors:**

*   `Softmax(double tau, Random random, Decay decayFn = null)`: Initializes a new instance of the <[`Softmax`](relative/SharpRL/Learning/SelectionPolicy/Softmax.cs:13)> class.
    *   `tau`: The temperature.
    *   `random`: The random generator.
    *   `decayFn`: The decay function for epsilon. If null, epsilon is not decayed.

**Properties:**

*   `Tau`: Returns the Tau.

**Methods:**

*   `Update(int episode)`: Updates the selection policy.
    *   `episode`: The episode number.
*   `Select(QValue value)`: Selects an action from the given Q-value.
    *   `value`: The Q-value.
    *   `Returns`: The index of the action to execute.

## Util

### NormalRandomGenerator

```csharp
public sealed class NormalRandomGenerator
```

Represents a random generator using a normal distribution.

**Constructors:**

*   `NormalRandomGenerator(Random random)`: Initializes a new instance of the <[`NormalRandomGenerator`](relative/SharpRL/Util/NormalRandomGenerator.cs:13)> class.
    *   `random`: The uniform random generator to use.

**Methods:**

*   `Next(double standardDeviation = 1.0, double mean = 0.0)`: Generates a random value from the given normal distribution.
    *   `standardDeviation`: The standard deviation.
    *   `mean`: The mean.
    *   `Returns`: A random number following a normal distribution.