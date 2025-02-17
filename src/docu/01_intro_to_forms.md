# Forms Are a Little Tricky in React.js

## Why Are Forms Tricky in React?

### Definition
A collection of input fields, typically used in conjunction with labels, all wrapped by the built-in HTML `<form>` element.

### Forms Have Two Main Functionalities:
1. **Submit & Extract Values**
2. **Validate Data Provided by the User and Handle Errors**

## Extraction and Submission of Forms
Extraction and submission of forms are relatively easy and straightforward:
- Entered values can be managed via **state**.
- Values can be extracted via **refs**.
- Built-in features via the **browser** can be used, e.g., `FormData`.

Extracting values isn't that difficult. The challenge comes with validation.

## Input Validation: The Tricky Part
### Common Validation Approaches & Their Drawbacks
1. **Validate on every keystroke** → Risk of showing errors too early.
2. **Validate when focus on an input field is lost** → Errors could remain visible for too long.
3. **Validate on form submission** → Might be too late to show validation.

As we can see, input validation approaches in React all have potential drawbacks, making forms a bit tricky to handle properly.

