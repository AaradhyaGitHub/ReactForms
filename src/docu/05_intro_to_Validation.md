# **Quick Overview on Input Validation**

## **Providing a Good User Experience is Tricky**

Ensuring smooth and intuitive input validation is essential for good user experience. However, choosing the right approach is challenging, as different methods impact how users interact with the form.

---

## **Approach #1: Validate on Every Keystroke**

One way to validate user input is by checking the input field on every keystroke. However, this requires a **state-based approach**, as we need to monitor every change dynamically. Using `useRef` wouldn’t work effectively in this scenario.

### **Basic Validation Example**

```jsx
const emailIsInvalid =
  enteredValues.email !== "" && !enteredValues.email.includes("@");
```

This approach ensures that the error message is displayed as soon as the user types, and the input is non-empty but lacks an `@` symbol:

```jsx
<div className="control-error">
  {emailIsInvalid && <p>Please Enter a valid email address</p>}
</div>
```

### **User Experience Issue**

- **Problem:** This method immediately throws an error message as soon as the user starts typing.
- **Impact:** The experience feels frustrating since the user hasn’t had a chance to complete their input.

---

## **Approach #2: Validate on Lost Focus**

A better way is to validate input only when the user **loses focus** (i.e., when they move to another field). This prevents premature error messages while still ensuring proper validation.

### **Using State to Track Focus**

```jsx
const [didEdit, setDidEdit] = useState({
  email: false,
  password: false
});

const emailIsInvalid = didEdit.email && !enteredValues.email.includes("@");
```

### **Handling Input Blur Event**

To detect when the user leaves the field, we create an event listener:

```jsx
function handleInputBlur(identifier) {
  setDidEdit((prevEdit) => ({
    ...prevEdit,
    [identifier]: true
  }));
}
```

This ensures that the error message only appears **after** the user has left the input field, reducing frustration.

---

## **Refinement: Reset Validation on Typing**

While validating on blur is helpful, we can improve user experience further by also **removing** errors when the user starts typing again. This creates a more seamless interaction.

### **Handling Input Change Event**

```jsx
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
```

### **Benefits of This Approach**

- No errors initially.
- Error appears **only after** the user loses focus.
- If the user starts typing again, the error message disappears.
- If they lose focus again without correcting the mistake, the error reappears.

This combines the best of both worlds: **real-time validation and focus-based validation**.

---

## **Key to the Best User Experience**

The optimal approach to validation involves:
✅ Validating input on **every keystroke** to provide real-time feedback.
✅ Validating input on **lost focus** to prevent premature error messages.
✅ **Resetting validation state** when the user resumes typing.

By combining these strategies, we create an intuitive and frustration-free form experience.

---

# **Approach#3 Validating upon Form Submission**

## **Why Validate on Submission?**

While live validation provides a great user experience by giving instant feedback, there are situations where validation should only happen when the user submits the form. This ensures that the submitted data meets all requirements before being processed.

## **Implementation Using `useRef`**

For this approach, we will revert to using `useRef` to extract user input. When using `useRef`, we cannot validate on every keystroke since refs do not cause re-renders when their values change. Instead, we can only validate when the form is submitted.

### **Defining State for Validation**

We introduce a state to track if the email input is invalid:

```javascript
const [emailIsInvalid, setEmailIsInvalid] = useState(false);
```

### **Checking the Validity of the Email**

We define a constant to determine whether the entered email is valid:

```javascript
const emailIsValid = enteredEmail.includes("@");
```

If the email is invalid, we update the state and prevent further execution (such as sending an HTTP request with invalid data):

```javascript
if (!emailIsValid) {
  setEmailIsInvalid(true);
  return; // Stop further execution to prevent invalid submissions
}
```

### **Dynamically Displaying the Validation Message**

Now, we can conditionally display an error message if the email is invalid:

```jsx
<div className="control-row">
  <div className="control no-margin">
    <label htmlFor="email">Email</label>
    <input id="email" type="email" name="email" ref={email} />
    <div className="control-error">
      {emailIsInvalid && <p>Please enter a valid email address</p>}
    </div>
  </div>
</div>
```

## **Why Submission Validation is Crucial**

While live validation (on each input change) improves user experience, **validation upon form submission is essential** to ensure data integrity. Operations depending on form data should never receive incorrect or incomplete information. Here’s why:

- **Ensuring Data Completeness**: Without final validation on submission, users could bypass input checks and submit invalid or empty fields.
- **Preventing Incorrect Data from Propagating**: If data is being sent to an API or database, invalid data could cause errors or corrupt stored information.
- **Security Considerations**: Even if client-side validation is present, always assume that data can be tampered with. Server-side validation is still necessary.

### **Issue with the Second Approach**

While the second approach (validating on keystrokes) is elegant, it **does not prevent users from submitting empty fields**. Without submission validation, users can bypass individual checks and send incomplete or incorrect data.

By ensuring that form validation happens **both live and upon submission**, we create a robust and user-friendly experience while maintaining data integrity.

---

# **Approach#4 Validating via Built-in Validation Prop **

Example: `required` prop on all input fields or `minLength` on passwords

## required exammple on **Email
```jsx
    <input id="email" type="email" name="email" required />
```

## required and minLength exammple on **Password
```jsx
    <input
        id="password"
        type="password"
        name="password"
        required
        minLength={6}
    />
```

## required exammple on **Select
```jsx
    <select id="role" name="role" required>
```

# **Approach#5: COMBINING EVERYTHING!**

## **Ensuring that the Two Passwords Match**

One of the critical parts of a form validation system is ensuring that the user enters matching passwords. To handle this, we introduce a state variable:

```jsx
const [passwordsAreNotEqual, setPasswordsAreNotEqual] = useState(false);
```

### **Breaking It Down:**
1. **State Initialization:**
   - We use `useState` to create a boolean state variable called `passwordsAreNotEqual`.
   - Initially, it is set to `false`, assuming the passwords match until proven otherwise.

2. **Password Matching Logic:**
   - When the user submits the form or updates the password fields, we compare the `password` and `confirm-password` fields.
   - If they do not match, we update the state to `true`, indicating an error.
   - The `return` statement ensures that the function execution stops here if the passwords don't match, preventing further processing.

```jsx
if (data.password !== data["confirm-password"]) {
  setPasswordsAreNotEqual(true);
  return;
}
```

### **Main Logic:**
- `data.password` and `data["confirm-password"]` are extracted from the form inputs.
- If they do not match, `setPasswordsAreNotEqual(true)` triggers a state update, which can later be used to display an error message dynamically.
- The `return` statement ensures that no further validation or form submission happens if the passwords are incorrect.

## **Dynamic Display of Input Fields and Errors**

We need to handle user input dynamically while providing real-time feedback. The following snippet demonstrates an approach for handling email input validation:

```jsx
<div className="control-row">
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
      {emailIsInvalid && <p>Please enter a valid email address</p>}
    </div>
  </div>
</div>
```

### **Detailed Breakdown:**

#### **1. Handling Input Events:**
- **`onBlur={() => handleInputBlur("email")}`**
  - Calls `handleInputBlur("email")` when the user leaves the email input field.
  - This is useful for triggering validation checks after user interaction.
- **`onChange={(event) => handleInputChange("email", event.target.value)}`**
  - Updates the state as the user types.
  - Uses `handleInputChange` to process the entered value.
  
#### **2. Managing Input State:**
- **`value={enteredValues.email}`**
  - Ensures that the input field is controlled, meaning its value is always derived from the component state.

#### **3. Displaying Validation Messages:**
- **Error Handling:**
  - If `emailIsInvalid` is `true`, the error message `<p>Please enter a valid email address</p>` is displayed inside `div.control-error`.
  - This ensures real-time validation feedback to the user.

### **Main Logic Recap:**
- The email input dynamically updates as the user types (`onChange`).
- When the user leaves the field (`onBlur`), additional validation can be triggered.
- If validation fails, an error message is displayed.
- The approach ensures a smooth and responsive user experience.

---

### **Conclusion**
By combining password validation and dynamic input handling, we create a robust form validation system.
- The password fields ensure both values match before allowing submission.
- The email field dynamically tracks user input and provides real-time feedback on validity.

This approach enhances usability, improves error handling, and provides a seamless form experience.


