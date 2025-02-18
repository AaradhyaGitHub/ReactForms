## **Creating a Flexible Input Component**

To improve reusability and maintainability, we extract the input field logic into a reusable component, `Input.jsx`. This makes our form components cleaner and more modular.

### **Original Hardcoded Input Component**
```jsx
<div className="control no-margin">
  <label htmlFor="email">Email</label>
  <input
    id="email"
    type="email"
    name="email"
    onBlur={() => handleInputBlur("email")}
    onChange={(event) => handleInputChange("email", event.target.value)}
    value={enteredValues.email}
  />
  <div className="control-error">
    {emailIsInvalid && <p>Please Enter a valid email address</p>}
  </div>
</div>
```

### **Making It More Flexible**
We refactor the code into a reusable component that can handle any type of input dynamically:

```jsx
export default function Input({ label, id, error, ...props }) {
  return (
    <div className="control no-margin">
      <label htmlFor={id}>{label}</label>
      <input id={id} {...props} />
      <div className="control-error">{error && <p>{error}</p>}</div>
    </div>
  );
}
```

### **Breaking It Down:**
- **`label`**: Dynamically sets the label text.
- **`id`**: Ensures each input has a unique identifier.
- **`error`**: Receives an error message and conditionally renders it.
- **`...props`**: Allows passing any additional props dynamically (e.g., `type`, `name`, `value`, `onChange`, `onBlur`).

### **Using the Flexible Input Component**

Instead of manually writing out each input field, we can now use our `Input` component:

```jsx
<Input 
  label="Email"
  id="email"
  type="email"
  name="email"
  onBlur={() => handleInputBlur("email")}
  onChange={(event) => handleInputChange("email", event.target.value)}
  value={enteredValues.email}
/>
```

### **Handling Errors as Props**

One major advantage of our new component is passing error messages dynamically as props:

```jsx
<Input
  label="Email"
  id="email"
  type="email"
  name="email"
  onBlur={() => handleInputBlur("email")}
  onChange={(event) => handleInputChange("email", event.target.value)}
  value={enteredValues.email}
  error={emailIsInvalid && 'Please Enter a Valid Email'}
/>
```

### **What's Happening Here?**
- **`error={emailIsInvalid && 'Please Enter a Valid Email'}`**:
  - If `emailIsInvalid` is `true`, the error message `'Please Enter a Valid Email'` is passed as a prop.
  - If `emailIsInvalid` is `false`, nothing is rendered in the error section.
- This allows dynamic error handling without cluttering the main form logic.

## **Conclusion**
By refactoring our input fields into a reusable `Input` component, we:
- Simplify form code and reduce redundancy.
- Improve maintainability by handling error messages dynamically.
- Ensure flexibility, allowing for multiple input types and error handling in a clean, readable way.

This approach greatly enhances form validation and improves user experience.

