# 4. **Using Native Built-In Features for Getting Hold of User Form Values**

## **Introduction**

When handling form submissions in web development, we often need a way to efficiently retrieve user input values. Instead of manually accessing each form element, we can leverage the **FormData** API, a built-in browser feature designed for this purpose.

FormData is a special constructor function that simplifies the process of extracting user input values from a form. This method is particularly useful when dealing with multiple inputs and structured data.

---

## **Step-by-Step Methodology**

### **1. Capturing Form Data with FormData**

To begin using FormData, we need to listen for the form submission event and prevent its default behavior:

```javascript
function handleSubmit(event) {
    event.preventDefault();
    const fd = new FormData(event.target);
}
```

### **2. Understanding `event.target`**

- In the `handleSubmit` function, the **`event.target`** refers to the submitted form element.
- We pass `event.target` into `new FormData()`, allowing us to extract all input values from that form.
- The resulting `fd` (FormData instance) provides access to these values using built-in methods.

### **3. Required Conditions for FormData to Work Properly**

To ensure FormData correctly extracts input values, follow these rules:
- **Each input field must have a `name` attribute**.
- **Even `<select>` fields must have a `name` attribute**.
- Without a `name` attribute, FormData will ignore the input value.

### **4. Extracting Individual Form Values**

Once we have a FormData object, we can retrieve values using its `.get()` method. For example:

```javascript
const enteredEmail = fd.get('email');
const enteredPassword = fd.get('password');
```

This works well for a small number of inputs, but manually extracting values for many fields can become tedious.

---

## **Handling Multiple Inputs Dynamically**

### **1. Converting FormData into an Object**

Instead of manually extracting each field, we can convert the entire FormData instance into an object using `Object.fromEntries()`:

```javascript
const data = Object.fromEntries(fd.entries());
```

This transformation gives us a structured object where:
- Each form field name becomes a property.
- Each corresponding value is stored as a key-value pair.

### **2. Handling Multiple Inputs with the Same Name**

A challenge arises when we have multiple inputs sharing the same `name` attribute, such as checkboxes. Consider the following example:

```html
<fieldset>
    <legend>How did you find us?</legend>
    <div className="control">
        <input type="checkbox" id="google" name="acquisition" value="google" />
        <label htmlFor="google">Google</label>
    </div>
    <div className="control">
        <input type="checkbox" id="friend" name="acquisition" value="friend" />
        <label htmlFor="friend">Referred by friend</label>
    </div>
    <div className="control">
        <input type="checkbox" id="other" name="acquisition" value="other" />
        <label htmlFor="other">Other</label>
    </div>
</fieldset>
```

### **3. Retrieving Multiple Values for the Same Name**

Since checkboxes with the same name attribute may have multiple values, we need to use `fd.getAll()` instead of `fd.get()`. This ensures we retrieve an array of selected values:

```javascript
const acquisitionChannel = fd.getAll('acquisition');
```

This method gathers all selected values under the `acquisition` name and returns an array.

### **4. Merging the Array with the Main Data Object**

Once we have the `acquisitionChannel` array, we need to add it to our `data` object:

```javascript
data.acquisition = acquisitionChannel;
```

---

## **Final Optimized Function**

Bringing everything together, our final function efficiently extracts form values, including multiple selections:

```javascript
function handleSubmit(event) {
    event.preventDefault();
    
    const fd = new FormData(event.target);
    const acquisitionChannel = fd.getAll('acquisition');
    
    const data = Object.fromEntries(fd.entries());
    data.acquisition = acquisitionChannel;
    
    console.log(data);
}
```

---

## **Conclusion**

By leveraging **FormData**, we streamline the process of retrieving user input values:
1. **Automatic extraction** – No need to manually select each input.
2. **Efficient handling** – Supports multiple values (checkbox groups) seamlessly.
3. **Scalability** – Converts the form data into a structured object dynamically.

This approach minimizes repetitive code and ensures a more maintainable, scalable form-handling process in modern web applications.

