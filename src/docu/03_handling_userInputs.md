# Handling User Inputs in React

There are three main ways to handle user inputs in React:

## 1. **Individual State for Each Input Field**
In this approach, we maintain separate pieces of state for each input field.

```jsx
const [enteredEmail, setEnteredEmail] = useState("");
const [enteredPassword, setEnteredPassword] = useState("");

function handleEmailChange(event) {
  setEnteredEmail(event.target.value);
}
```

This approach is simple but can become cumbersome as the number of inputs increases.

---

## 2. **Single Object State for All Input Fields**
A more scalable approach is to manage all input fields in a single state object.

```jsx
const [enteredValues, setEnteredValues] = useState({
  email: '',
  password: ''
});

function handleInputChange(identifier, value) {
  setEnteredValues(prevValues => ({
    ...prevValues,
    [identifier]: value
  }));
}
```

### Breaking Down the `handleInputChange` Function:
- The function takes two parameters:
  - `identifier`: The name of the input field being updated (e.g., "email" or "password").
  - `value`: The new value entered by the user.
- `setEnteredValues` uses the **previous state (`prevValues`)** to ensure that updates are not lost.
- The `[identifier]: value` syntax is **dynamic object property assignment**, allowing us to update the correct field without manually checking which input was changed.

### How It's Used in JSX:
```jsx
<input
  id="email"
  type="email"
  name="email"
  onChange={(event) => handleInputChange('email', event.target.value)}
  value={enteredValues.email}
/>
```
- `event.target.value` refers to the **current value** inside the input field.
- The `handleInputChange` function dynamically updates the corresponding field inside the `enteredValues` state object.

---

## 3. **Using `useRef` for Uncontrolled Components**
This requires **less code** but comes with **downsides**:
- It is **harder to reset** the input fields.
- You end up with **many refs**, which you must manually connect to each input field.
- This approach **manipulates the DOM directly**, which is considered **anti-React**.

### Example Using `useRef`:
```jsx
import { useRef } from 'react';

function LoginForm() {
  const email = useRef();
  const password = useRef();

  function handleSubmit(event) {
    event.preventDefault();
    console.log("Form Submitted!");

    const enteredEmail = email.current.value;
    const enteredPassword = password.current.value;
    console.log(enteredEmail, enteredPassword);
  }

  return (
    <form onSubmit={handleSubmit}>
      <input type="email" ref={email} placeholder="Email" />
      <input type="password" ref={password} placeholder="Password" />
      <button type="submit">Login</button>
    </form>
  );
}
```

### Breaking Down the `useRef` Logic:
1. **Creating Refs:**
   - `const email = useRef();` and `const password = useRef();` create references to store direct references to DOM elements.
   - Unlike state, `useRef` **does not trigger re-renders** when updated.

2. **Accessing Input Values:**
   - `email.current.value` gives us direct access to the input field's value **without** React controlling it.

3. **Preventing Default Form Submission Behavior:**
   - `event.preventDefault();` stops the page from reloading when the form is submitted.

### Why `useRef` Is Considered **Anti-React**:
- React promotes **state-driven UI updates**, whereas `useRef` accesses values directly from the DOM.
- With `useRef`, React does not track the input value changes, meaning we lose benefits like **reactive updates and controlled validation**.
- Resetting values requires **manual DOM manipulation** (`email.current.value = ""`), which is cumbersome.

### When Should You Use `useRef`?
- When you need **raw access to a DOM element**, like focusing an input field.
- When performance is critical, and you **want to avoid unnecessary re-renders**.
- When handling **file inputs** or **third-party UI libraries** that require direct DOM interaction.

In most cases, **controlled components using state** are preferred over `useRef`.
