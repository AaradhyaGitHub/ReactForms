# Default Browser Behavior for Buttons Inside a `<form>` Element

## Default Behavior
- Buttons will **submit** the form.
- An **HTTP request** is sent to the server serving the website.
- This is a **built-in feature** of browsers.

### Why Does This Matter?
Even if we have a `handleSubmit()` function that logs "submit" and bind this to an `onClick` on a button, clicking this button will refresh the page due to this default behavior. The entire page reloads because buttons inside form elements generate and send requests.

This can be problematic because, in our case, the server serving the website is a **pure development server**. It is not prepared to handle form submissions or incoming requests.

## How to Handle This Default Form Submission Behavior

### 1. **Add `type="button"` to the Button**
By default, buttons inside a form act as submit buttons. Explicitly setting `type="button"` prevents form submission and stops the page from reloading.

### 2. **Use `onSubmit` on the `<form>` Element**
```jsx
<form onSubmit={handleSubmit}>
```
- `handleSubmit` now receives an event object.
- This event object has a `preventDefault()` method.
- Calling `preventDefault()` **prevents the default browser behavior** (which is to send the HTTP request).

### 3. **Using React's Form Action Approach (To Be Discussed Later)**
React provides another way to handle forms through the **Form Action approach**, which will be covered in a later discussion.

