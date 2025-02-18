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
    
    setDidEdit(prevEdit => ({
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

## **Next Step: Validating on Form Submission**
While this method works great for live validation, sometimes we only want to validate **when the user submits the form**. In the next section, we’ll explore how to handle form submission validation efficiently.