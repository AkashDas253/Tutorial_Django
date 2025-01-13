### **Form Fields and Their Parameters**


| **Field Type**         | **Description**                                                                                             | **Parameters**                                                              | **Default Value**   | **Range/Options**                                                                                         |
|-------------------------|---------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------|---------------------|-----------------------------------------------------------------------------------------------------------|
| **`CharField`**         | Used for single-line text input such as names, usernames, or titles.                                      | `max_length`, `min_length`, `required`, `label`, `widget`, `initial`        | `required=True`     | `max_length`: Integer, `min_length`: Integer                                                             |
| **`IntegerField`**      | For numeric inputs; commonly used for age, count, or identifiers.                                         | `min_value`, `max_value`, `required`, `label`, `widget`, `initial`          | `required=True`     | `min_value`: Integer, `max_value`: Integer                                                               |
| **`EmailField`**        | Ensures the input matches an email format. Useful for contact forms or user registration.                 | `required`, `label`, `widget`, `initial`                                   | `required=True`     | Valid email format                                                                                       |
| **`DateField`**         | Used for date inputs. Commonly utilized in forms for birth dates, appointments, etc.                      | `required`, `label`, `widget`, `input_formats`, `initial`                  | `required=True`     | `input_formats`: List of date formats                                                                   |
| **`BooleanField`**      | For checkboxes or boolean selections like "Accept Terms & Conditions."                                    | `required`, `label`, `widget`, `initial`                                   | `required=False`    | `initial`: `True` or `False`                                                                            |
| **`ChoiceField`**       | Provides dropdown options. Frequently used for gender selection, roles, or other categorical choices.     | `choices`, `required`, `label`, `widget`, `initial`                        | `required=True`     | `choices`: List of tuples (e.g., `[('A', 'Option A'), ('B', 'Option B')]`)                              |
| **`MultipleChoiceField`** | Allows multiple selections from a list. Used in forms like surveys or multi-select preferences.          | `choices`, `required`, `label`, `widget`, `initial`                        | `required=True`     | `choices`: List of tuples                                                                               |
| **`FileField`**         | Used for uploading files, such as resumes or documents.                                                  | `required`, `label`, `widget`, `initial`, `max_length`                     | `required=True`     | `max_length`: Integer                                                                                   |
| **`ImageField`**        | Specific to image uploads (e.g., profile pictures, product photos).                                       | `required`, `label`, `widget`, `initial`, `max_length`                     | `required=True`     | Image file formats                                                                                      |
| **`URLField`**          | Ensures valid URL input. Commonly used for website links or API endpoints.                               | `required`, `label`, `widget`, `initial`                                   | `required=True`     | Valid URL format                                                                                        |
| **`SlugField`**         | For storing URL-friendly short strings (e.g., slugs for blog titles).                                     | `max_length`, `allow_unicode`, `required`, `label`, `initial`              | `allow_unicode=False` | `max_length`: Integer                                                                                   |
| **`DecimalField`**      | Accepts decimal values. Useful for prices, ratings, or financial data.                                   | `max_digits`, `decimal_places`, `required`, `label`, `widget`, `initial`   | `required=True`     | `max_digits`: Integer, `decimal_places`: Integer                                                        |
| **`TimeField`**         | Used for inputting time values.                                                                          | `required`, `label`, `widget`, `initial`, `input_formats`                  | `required=True`     | `input_formats`: List of time formats                                                                   |
| **`DurationField`**     | Handles durations (e.g., `timedelta` objects).                                                          | `required`, `label`, `widget`, `initial`                                   | `required=True`     | Input: Duration format                                                                                  |
| **`IPAddressField`**    | Validates IPv4 or IPv6 addresses. Useful for networking-related forms.                                   | `required`, `label`, `initial`, `protocol`                                 | `protocol='both'`   | `protocol`: `'IPv4'`, `'IPv6'`, `'both'`                                                                |
| **`JSONField`**         | Allows JSON input, suitable for configurations or structured data.                                       | `required`, `label`, `widget`, `initial`                                   | `required=True`     | Valid JSON format                                                                                       |

---

### **Description of Key Parameters**
1. **`max_length`**: Specifies the maximum number of characters allowed for text fields.
2. **`min_value` / `max_value`**: Enforces numeric boundaries.
3. **`choices`**: A list of tuples specifying selectable options.
4. **`widget`**: Defines the input type in HTML (e.g., `Textarea`, `NumberInput`).
5. **`initial`**: Prepopulates a default value in the form field.
6. **`input_formats`**: Specifies acceptable formats for `DateField` and `TimeField`.

---

### **Field Usage Examples**

#### **`CharField`**
```python
name = forms.CharField(max_length=50, required=True, label="Full Name")
```

#### **`ChoiceField`**
```python
gender = forms.ChoiceField(
    choices=[('M', 'Male'), ('F', 'Female')],
    required=True,
    label="Gender"
)
```

#### **`FileField`**
```python
resume = forms.FileField(required=True, label="Upload Resume")
```

#### **`DecimalField`**
```python
price = forms.DecimalField(max_digits=10, decimal_places=2, required=True, label="Price")
```

---
