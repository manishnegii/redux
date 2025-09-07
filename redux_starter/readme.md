# Redux: A Descriptive Guide

This repository contains a detailed explanation of **Redux**, a state management library for JavaScript applications, with examples and modern usage using **Redux Toolkit**.

---

## Table of Contents

1. [What is Redux?](#what-is-redux)
2. [Core Principles of Redux](#core-principles-of-redux)
3. [Core Concepts](#core-concepts)
4. [How Redux Works](#how-redux-works)
5. [Example: Counter App](#example-counter-app)
6. [Why Use Redux?](#why-use-redux)
7. [Redux Toolkit](#redux-toolkit)
8. [References](#references)

---

## What is Redux?

Redux is a **state management library** for JavaScript applications.  
It acts as a **central brain** for your app, storing all important data (state) in one place called the **store**, making state management predictable and easier to debug.

In a React app, for example, each component can have its own state. But as your app grows, sharing data between components becomes messy. Redux solves this by storing all state in a single place, called the store

**Analogy:**  
Imagine your app is a company. Components are employees. Each employee (component) could keep its own notes (state), but it’s confusing if they all have different versions. Redux acts like the central office filing system, where all important information is stored and accessible to everyone
---

## Core Principles of Redux

Redux follows **three main principles**:

1. **Single Source of Truth**  
   - The entire app state is stored in a single store object.

2. **State is Read-Only**  
   - State cannot be modified directly; it changes only through **actions**.

3. **Changes are Made with Pure Functions**  
   - **Reducers** are pure functions that calculate new state from the current state and an action.

---

## Core Concepts

| Concept       | Description |
|---------------|-------------|
| **Store**     | Holds the app state and allows reading, dispatching actions, and subscribing to changes. |
| **Action**    | A plain object describing what happened. Must have a `type` property, can have a payload. |
| **Reducer**   | A pure function that updates state based on the action. |
| **Dispatch**  | Sends an action to the store. |
| **Selector**  | Extracts specific parts of the state from the store. |


1. **Store**

The store is the central object holding the application state.

Responsibilities:
Hold the current state.
Allow reading the state.
Allow dispatching actions.
Allow subscribing to state changes.



2. **Action**

An action is a plain JavaScript object describing what happened.
Every action must have a type (a string), and it can optionally have a payload (data for the update).


## Example:
 
```javascript
{ type: 'INCREMENT' }   // simple action
{ type: 'ADD_TODO', payload: { id: 1, text: 'Learn Redux' } }
```

3. **Reducer**

A reducer is a function that determines how the state changes in response to an action.
It takes the current state and the action and returns a new state.


 ## Example:

```javascript
function counterReducer(state = { count: 0 }, action) {
  switch (action.type) {
    case 'INCREMENT':
      return { count: state.count + 1 };
    case 'DECREMENT':
      return { count: state.count - 1 };
    default:
      return state;
  }
}
```

4. **Dispatch**

The dispatch function sends an action to the store, which then calls the reducer.


Example:
```javascript

store.dispatch({ type: 'INCREMENT' });

```
5. **Selector**

A selector is a function that extracts specific parts of the state from the store.
This helps components access only the data they need.



---

## How Redux Works

1. **User triggers an event** (e.g., clicks a button)  
2. **Dispatch an action** describing what happened  
3. **Reducer calculates the new state** without modifying the old one  
4. **Store updates the state**  
5. **UI reacts** and renders based on the updated state

---

## Example: Counter App

```javascript

///////reducer.js///////////

// Initial state
const initialState = { count: 0 };

// Reducer
export default function counterReducer(state = initialState, action) {
  switch(action.type) {
    case 'INCREMENT':
      return { count: state.count + 1 };
    case 'DECREMENT':
      return { count: state.count - 1 };
    default:
      return state;
  }
}

/////////////store.js//////////

// Create store
import counterReducer from './reducer';
import { createStore } from 'redux';
const store = createStore(counterReducer);

// Subscribe to state changes
store.subscribe(() => console.log(store.getState()));



///////////index.js////////////    entry point of the app

import store from './store';
// Dispatch actions
store.dispatch({ type: 'INCREMENT' }); // { count: 1 }
store.dispatch({ type: 'INCREMENT' }); // { count: 2 }
store.dispatch({ type: 'DECREMENT' }); // { count: 1 }

```

---


## Why Use Redux?

  1. Predictable state updates: Every change goes through actions and reducers.
  2. Centralized state: Easy to share data across components.
  3. Debugging is easier: DevTools let you track every state change over time.
  4. Middleware support: Handle asynchronous tasks like API calls using tools like redux-thunk or redux-saga.



---

## Redux Toolkit (Modern Redux)

Redux Toolkit simplifies Redux setup and reduces boilerplate:


```javascript


import { createSlice, configureStore } from '@reduxjs/toolkit';

const counterSlice = createSlice({
  name: 'counter',
  initialState: { count: 0 },
  reducers: {
    increment: state => { state.count += 1 },
    decrement: state => { state.count -= 1 },
  },
});

const store = configureStore({ reducer: counterSlice.reducer });

store.dispatch(counterSlice.actions.increment());
console.log(store.getState()); // { count: 1 }

```

1. (createSlice) combines actions and reducers in one place.
2. State updates can be written mutably thanks to Immer.js under the hood.