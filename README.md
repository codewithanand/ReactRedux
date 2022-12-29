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

