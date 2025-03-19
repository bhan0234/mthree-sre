# YAML Basics and Best Practices

## 1. Introduction to YAML
YAML (YAML Ain't Markup Language) is a human-readable data serialization format commonly used for configuration files and data exchange.

## 2. Basic YAML Syntax
- Uses indentation (spaces, not tabs) to define structure.
- Key-value pairs are written as `key: value`.
- Lists are denoted with `-`.

### Example:
```yaml
name: John Doe
age: 30
married: true
children:
  - Alice
  - Bob
address:
  street: 123 Main St
  city: New York
  zip: 10001
```

## 3. Lists in YAML
Lists are created using `-` (hyphen) followed by a space.

### Example:
```yaml
fruits:
  - Apple
  - Banana
  - Mango
```

## 4. Dictionaries in YAML
Dictionaries (also called maps or objects) are key-value pairs.

### Example:
```yaml
person:
  name: Alice
  age: 28
  city: London
```

## 5. Nested Structures
YAML allows nesting lists and dictionaries.

### Example:
```yaml
employees:
  - name: John
    age: 24
    department: IT
  - name: Sarah
    age: 30
    department: HR
```

## 6. Multi-Line Strings
Use `|` for block-style multi-line strings and `>` for folded multi-line strings.

### Block Style (`|` keeps line breaks)
```yaml
bio: |
  John is a software engineer.
  He works on Kubernetes deployments.
```

### Folded Style (`>` converts new lines to spaces)
```yaml
description: >
  This is a folded string.
  It will be written as a single line.
```

## 7. YAML Best Practices
✅ **Use spaces, not tabs**
✅ **Maintain consistent indentation (2 or 4 spaces)**
✅ **Use meaningful keys**
✅ **Avoid trailing spaces**
✅ **Enclose special characters in quotes**

### Example of Incorrect YAML ❌
```yaml
name: John
age: 30
  city: New York  # Incorrect indentation ❌
```
### Correct YAML ✅
```yaml
name: John
age: 30
city: New York
```

## 8. YAML vs JSON
YAML is a superset of JSON. The equivalent JSON for the following YAML:

```yaml
person:
  name: John
  age: 30
  city: New York
```

Would be:
```json
{
  "person": {
    "name": "John",
    "age": 30,
    "city": "New York"
  }
}
```

## 9. Common YAML Errors and Fixes
| Issue | Incorrect Example | Correct Example |
|-------|------------------|----------------|
| Indentation error | `name: John`<br>`  age: 30` | `name: John`<br>`age: 30` |
| Tabs instead of spaces | (tabs used) | (use spaces instead) |
| Missing space after `:` | `name:John` | `name: John` |
| Improper list formatting | `-item1` | `- item1` |

## 10. Conclusion
YAML is widely used for configuration files (e.g., Kubernetes, Docker Compose, Ansible). Following best practices ensures readability and error-free parsing.

