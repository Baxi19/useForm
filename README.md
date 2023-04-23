# useForm
#### React Custom hook that allows for easy and efficient handling and validation of forms

```jsx
import { useState } from 'react';

const useForm = (initialValues, validate) => {
  const [values, setValues] = useState(initialValues);
  const [errors, setErrors] = useState({});

  const handleChange = (e) => {
    const { name, value } = e.target;
    setValues({ ...values, [name]: value });
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    const validationErrors = validate(values);
    setErrors(validationErrors);
    if (Object.keys(validationErrors).length === 0) {
      // If there are no validation errors, perform some action (e.g. submit the form)
      console.log('Form submitted', values);
    }
  };

  return { values, errors, handleChange, handleSubmit };
};

export default useForm;
```
In this example, the useForm hook takes two arguments: initialValues, which is an object containing the initial values of the form, and validate, which is a function that validates the form values and returns an object of errors.

The hook uses local state to maintain the form values and errors, and defines two functions: handleChange and handleSubmit. The handleChange function updates the corresponding value in the local state when a form field changes, while the handleSubmit function handles the validation of the form data and performs some action if the data is valid (in this case, it simply logs a message to the console).

To use this hook in a component, you can do the following:
```jsx
import useForm from './useForm';

const MyForm = () => {
  const initialValues = { name: '', email: '', message: '' };

  const validate = (values) => {
    const errors = {};
    if (!values.name) {
      errors.name = 'Name is required';
    }
    if (!values.email) {
      errors.email = 'Email is required';
    }
    if (!values.message) {
      errors.message = 'Message is required';
    }
    return errors;
  };

  const { values, errors, handleChange, handleSubmit } = useForm(initialValues, validate);

  return (
    <form onSubmit={handleSubmit}>
      <label>
        Name:
        <input type="text" name="name" value={values.name} onChange={handleChange} />
        {errors.name && <span>{errors.name}</span>}
      </label>
      <label>
        Email:
        <input type="email" name="email" value={values.email} onChange={handleChange} />
        {errors.email && <span>{errors.email}</span>}
      </label>
      <label>
        Message:
        <textarea name="message" value={values.message} onChange={handleChange} />
        {errors.message && <span>{errors.message}</span>}
      </label>
      <button type="submit">Submit</button>
    </form>
  );
};
```
