# What is Redux?
**Redux is a pattern and library for managing and updating application state, using events called "actions".** It serves as a centralized store for state that needs to be used across your entire application, with rules ensuring that the state can only be updated in a predictable fashion.

# Why should I use Redux?
- Redux helps you manage "global" state - state that is needed across many parts of your application.
- **The patterns and tools provided by Redux make it easier to understand when, where, why, and how the state in your application is being updated, and how your application logic will behave when those changes occur.**
- Redux guides you towards writing code that is predictable and testable, which helps give you confidence that your application will work as expected.

# When should I use Redux?
- You have large amounts of application state that are needed in many places in the app
- The app state is updated frequently over time
- The logic to update that state may be complex
- The app has a medium or large-sized codebase, and might be worked on by many people

# Redux Libraries and Tools
Redux is a small standalone JS library. However, it is commonly used with several other packages:
## React-Redux
- Redux can integrate with any UI framework, and is most frequently used with React.
- **React-Redux** is the official package that lets your React components interact with a Redux store by reading pieces of state and dispatching actions to update the store.

## Redux-Toolkit
- **Redux Toolkit** is the recommended approach for writing Redux logic.
- It contains packages and functions that we think are essential for building a Redux app.
- Redux Toolkit builds in suggested best practices, simplifies most Redux tasks, prevents common mistakes, and makes it easier to write Redux applications.

## Redux DevTools Extension
- The **Redux DevTools Extension** shows a history of the changes to the state in your Redux store over time.
- This allows you to debug your applications effectively, including using powerful techniques like "time-travel debugging".

# Terminology
### Actions
- An **action** is a plain JavaScript object that has a `type` field.
- **You can think of an action as an event that describes something that happened in the application.**
- The `type` field should be a string that gives this action a descriptive name, like `"todos/todoAdded"`.
- An action object can have other fields with additional information about what happened. By convention, we put that information in a field called `payload`.
```
const addTodoAction = {
  type: 'todos/todoAdded',
  payload: 'Buy milk'
}
```
### Action Creators
- An **action creator** is a function that creates and returns an action object. We typically use these so we don't have to write the action object by hand every time.
```
const addTodo = text => {
  return {
    type: 'todos/todoAdded',
    payload: text
  }
}
```
### Reducers
- A **reducer** is a function that receives the current `state` and an `action` object, decides how to update the state if necessary, and returns the new state: `(state, action) => newState`.
- **You can think of a reducer as an event listener which handles events based on the received action (event) type.**
**Reducers must always follow some specific rules:**
  - They should only calculate the new state value based on the `state` and `action` arguments
  - They are not allowed to modify the existing `state`. Instead, they must make *immutable updates*, by copying the existing `state` and making changes to the copied values.
  - They must not do any *asynchronous logic*, calculate random values, or cause other "side effects"
  The logic inside reducer functions typically follows the same series of steps:
  - Check to see if the reducer cares about this action
    * If so, make a copy of the state, update the copy with new values, and return it
  - Otherwise, return the existing state unchanged
```
const initialState = { value: 0 }

function counterReducer(state = initialState, action) {
  // Check to see if the reducer cares about this action
  if (action.type === 'counter/increment') {
    // If so, make a copy of `state`
    return {
      ...state,
      // and update the copy with the new value
      value: state.value + 1
    }
  }
  // otherwise return the existing state unchanged
  return state
}
```
### Store
- The current Redux application state lives in an object called the **store**
- The store is created by passing in a reducer, and has a method called `getState` that returns the current state value
```
import { configureStore } from '@reduxjs/toolkit'

const store = configureStore({ reducer: counterReducer })

console.log(store.getState())
// {value: 0}
```
### Dispatch
- The Redux store has a method called `dispatch`.
- **The only way to update the state is to call `store.dispatch()` and pass in an action object.**
- The store will run its reducer function and save the new state value inside, and we can call `getState()` to retrieve the updated value
```
store.dispatch({ type: 'counter/increment' })

console.log(store.getState())
// {value: 1}
```
### Selector
- **Selectors** are functions that know how to extract specific pieces of information from a store state value.
- As an application grows bigger, this can help avoid repeating logic as different parts of the app need to read the same data
```
const selectCounterValue = state => state.value

const currentValue = selectCounterValue(store.getState())
console.log(currentValue)
// 2
```
### Redux Slices
- **A "slice" is a collection of Redux reducer logic and actions for a single feature in your app**, typically defined together in a single file.
- The name comes from splitting up the root Redux state object into multiple "slices" of state.
```
import { configureStore } from '@reduxjs/toolkit'
import usersReducer from '../features/users/usersSlice'
import postsReducer from '../features/posts/postsSlice'
import commentsReducer from '../features/comments/commentsSlice'

export default configureStore({
  reducer: {
    users: usersReducer,
    posts: postsReducer,
    comments: commentsReducer
  }
})
```

# How to use Redux

### Create a redux store with `configureStore`
- `configureStore` accepts a `reducer` function as a named argument
- `configureStore` automatically sets up the store with good default settings

### Provide the Redux store to the React application components
- Put a React Redux `<Provider>` component around your `<App />`
- Pass the Redux store as `<Provider store={store}>`

### Create a Redux "slice" reducer with `createSlice`
- Call `createSlice` with a string name, an initial state, and named reducer functions
- Reducer functions may "mutate" the state using Immer
- Export the generated slice reducer and action creators

### Use the React Redux `useSelector/useDispatch` hooks in React components
- Read data from the store with the `useSelector` hook
- Get the `dispatch` function with the `useDispatch` hook, and dispatch actions as needed

