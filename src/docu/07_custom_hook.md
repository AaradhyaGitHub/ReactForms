# **Breaking Down Custom Hooks: Why & How**

## **Why Use a Custom Hook?**

When working with forms in React, we often find ourselves handling user input with `useState`, tracking whether the user has interacted with the fields, and writing event handlers for `onChange` and `onBlur`. The code for managing state and events can become repetitive, especially when dealing with multiple inputs.

To make our form logic reusable and cleaner, we can extract this functionality into a **custom hook**.

## **Refactoring: From Repetitive Code to a Custom Hook**

Initially, our form state management looked like this:

```jsx
const [enteredValues, setEnteredValues] = useState({
    email: "",
    password: ""
});

const [didEdit, setDidEdit] = useState({
    email: false,
    password: false
});

function handleInputChange(identifier, value) {
    setEnteredValues((prevValues) => ({
      ...prevValues,
      [identifier]: value
    }));
    setDidEdit((prevEdit) => ({
      ...prevEdit,
      [identifier]: false
    }));
}

function handleInputBlur(identifier) {
    setDidEdit((prevEdit) => ({
      ...prevEdit,
      [identifier]: true
    }));
}
```

### **Problems With This Approach**
- The logic is tied to a specific form structure (email and password fields).
- If we want to add more fields, we must update the state objects and handlers manually.
- The logic for handling user input is not reusable across different components.

## **Introducing a Custom Hook: `useInput`**

To solve these issues, we create a custom hook that encapsulates the logic for handling input changes and blurs:

```jsx
import { useState } from "react";

export function useInput(defaultValue) {
  const [enteredValue, setEnteredValue] = useState(defaultValue);
  const [didEdit, setDidEdit] = useState(false);

  function handleInputChange(event) {
    setEnteredValue(event.target.value);
    setDidEdit(false);
  }

  function handleInputBlur() {
    setDidEdit(true);
  }

  return {
    value: enteredValue,
    handleInputChange,
    handleInputBlur
  };
}
```

### **How This Works**
1. **Encapsulation:** Instead of managing multiple state values inside a component, we move the logic into a function (`useInput`) that returns the necessary state and handlers.
2. **Reusability:** This hook can now be used in multiple form inputs without duplicating logic.
3. **Simplifies Components:** The form component no longer needs to manage input state manually, reducing complexity.

### **How to Use `useInput` in a Component**

```jsx
function LoginForm() {
  const emailInput = useInput("");
  const passwordInput = useInput("");

  return (
    <form>
      <input
        type="email"
        value={emailInput.value}
        onChange={emailInput.handleInputChange}
        onBlur={emailInput.handleInputBlur}
      />
      <input
        type="password"
        value={passwordInput.value}
        onChange={passwordInput.handleInputChange}
        onBlur={passwordInput.handleInputBlur}
      />
    </form>
  );
}
```

## **Next Steps**

This is just the beginning! The `useInput` hook currently only tracks the value and blur state, but we can extend it to handle validation, error messages, and more. Feel free to add more logic as needed!

---


