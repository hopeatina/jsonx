# JSONX (JSON Extended) Specification

**Version:** 1.0

---

## Table of Contents

1. [Introduction](#introduction)
2. [Key Features](#key-features)
3. [Syntax Overview](#syntax-overview)
4. [Data Types](#data-types)
5. [Detailed Specifications](#detailed-specifications)
   - [Comments](#comments)
   - [Unquoted Keys](#unquoted-keys)
   - [Optional Commas and Trailing Commas](#optional-commas-and-trailing-commas)
   - [Multi-line Strings](#multi-line-strings)
   - [Variables and Constants](#variables-and-constants)
   - [References and Anchors](#references-and-anchors)
   - [Inline Schema Definitions](#inline-schema-definitions)
   - [Modularization and Includes](#modularization-and-includes)
   - [Enhanced Data Types](#enhanced-data-types)
6. [Examples](#examples)
   - [Advanced Configuration Example](#advanced-configuration-example)
7. [Compatibility](#compatibility)
8. [Conclusion](#conclusion)
9. [Key Improvements Over JSON](#key-improvements-over-json)
10. [Final Thoughts](#final-thoughts)
11. [Next Steps for Adoption](#next-steps-for-adoption)

---

## Introduction

**JSONX** is an extended version of JSON designed to significantly enhance developer productivity, data integrity, and overall usability. It introduces powerful features like variables, references, inline schemas, modularization, and enhanced data types, all aimed at making JSONX a substantial upgrade over standard JSON.

---

## Key Features

- **Comments**: Single-line (`//`, `#`) and multi-line (`/* ... */`) comments.
- **Unquoted Keys**: Simplifies object notation by allowing keys without quotes.
- **Optional Commas and Trailing Commas**: Cleaner syntax with optional commas.
- **Multi-line Strings**: Use triple quotes `"""..."""` for multi-line strings.
- **Variables and Constants**: Define reusable values with `let` and `const`.
- **References and Anchors**: Avoid data duplication with `&anchor` and `*reference`.
- **Inline Schema Definitions**: Enforce data structure and types within the document.
- **Modularization and Includes**: Import external JSONX files using `include`.
- **Enhanced Data Types**: Support for dates, regular expressions, and more.

---

## Syntax Overview

JSONX extends JSON syntax to include:

- **Comments**: `//`, `#`, `/* ... */`
- **Variables and Constants**: `let variableName = value`, `const constantName = value`
- **References and Anchors**: `&anchorName { ... }`, `*anchorName`
- **Includes**: `include "path/to/file.jsonx"`
- **Type Annotations**: For inline schema definitions, e.g., `key: type = value`
- **Enhanced Data Types**: Dates, regex patterns, etc.

---

## Data Types

- **Objects**
- **Arrays**
- **Strings**
- **Numbers**: Including hexadecimal (`0x`), binary (`0b`), and octal (`0o`) formats
- **Booleans**
- **Null**
- **Dates**
- **Regular Expressions**
- **Variables and Constants**

---

## Detailed Specifications

### Comments

#### Single-line Comments

- Begin with `//` or `#` and continue until the end of the line.

```jsonx
// This is a single-line comment
# This is also a single-line comment
```

#### Multi-line Comments

- Enclosed within `/*` and `*/`.

```jsonx
/*
  This is a multi-line comment.
  It can span multiple lines.
*/
```

### Unquoted Keys

- Object keys do not require quotes if they are valid identifiers (start with a letter, `_`, or `$`, followed by letters, numbers, `_`, or `$`).

```jsonx
{
  name: "John Doe",
  age: 30,
  $meta: "data",
  _private: true
}
```

### Optional Commas and Trailing Commas

- Commas between items are optional.
- Trailing commas after the last item are allowed.

```jsonx
{
  name: "Jane Doe"
  age: 25,
}
```

### Multi-line Strings

- Use triple quotes `"""..."""` for multi-line strings.

```jsonx
{
  bio: """
    Jane is a software developer
    with a passion for open-source projects.
    She loves coding in Python and JavaScript.
  """
}
```

### Variables and Constants

#### Definition

- **Variables**: Defined using `let variableName = value`.
- **Constants**: Defined using `const constantName = value`.
- **Scope**: Available throughout the file after declaration.

#### Usage

- Variables and constants can store any valid data type.

```jsonx
const API_URL = "https://api.example.com";
let timeout = 30;  // in seconds

{
  endpoint: API_URL + "/users",
  retryTimeout: timeout,
}
```

### References and Anchors

#### Anchors (`&`)

- Define reusable data blocks.
- Syntax: `&anchorName { ... }`

#### References (`*`)

- Reuse data blocks defined by anchors.
- Syntax: `*anchorName`

```jsonx
let defaultUser = &user {
  role: "user",
  permissions: ["read"]
};

{
  adminUser: {
    *user,  // Inherits from defaultUser
    role: "admin",
    permissions: ["read", "write", "delete"]
  }
}
```

### Inline Schema Definitions

#### Type Annotations

- Use colons to define types: `key: type = value`
- Supported types: `string`, `number`, `boolean`, `object`, `array`, `date`, `regex`, `null`

#### Schema Enforcement

- Parsers can validate data against the defined types.

```jsonx
{
  id: number = 12345,
  name: string = "John Doe",
  isActive: boolean = true,
  joinedAt: date = 2023-01-15,
  profile: object = {
    bio: string = "Software developer",
    interests: array = ["coding", "music"]
  }
}
```

### Modularization and Includes

#### Include Files

- Use `include "path/to/file.jsonx"` to import external files.
- Included files are merged into the current scope.

```jsonx
// In main.jsonx
include "config/database.jsonx"
include "config/server.jsonx"

{
  appName: "MyApplication",
  version: "1.0.0",
  database: databaseConfig,  // From database.jsonx
  server: serverConfig,      // From server.jsonx
}
```

### Enhanced Data Types

#### Dates

- Recognized using ISO 8601 format.
- Type annotated as `date`.

```jsonx
{
  eventDate: date = 2023-12-31T23:59:59Z,
}
```

#### Regular Expressions

- Defined using `/pattern/` syntax.
- Type annotated as `regex`.

```jsonx
{
  passwordPattern: regex = /^(?=.*\d)(?=.*[a-z])(?=.*[A-Z]).{8,}$/,
}
```

---

## Examples

### Advanced Configuration Example

```jsonx
/* Application Configuration */

const BASE_API_URL = "https://api.example.com";

let defaultHeaders = &headers {
  "Content-Type": "application/json",
  "Accept": "application/json",
};

include "secrets.jsonx"  // Contains API_KEY

schema Config = {
  apiUrl: string,
  apiKey: string,
  timeout: number,
  headers: object,
  features: object,
};

Config = {
  apiUrl: BASE_API_URL + "/v1",
  apiKey: API_KEY,  // From secrets.jsonx
  timeout: number = 60,  // in seconds
  headers: *headers,
  features: {
    logging: boolean = true,
    caching: boolean = false,
    experimental: boolean = ENV == "development",  // Computed value
  },
};
```

---

## Compatibility

- **Backward Compatibility**: Any valid JSON document is also a valid JSONX document.
- **Forward Compatibility**: JSONX parsers should process standard JSON without issues.
- **Parsing Requirements**: Parsers need to support extended syntax and features to utilize JSONX fully.

---

## Conclusion

**JSONX** offers substantial improvements over JSON by introducing:

- **Variables and Constants**: For reusability and easier maintenance.
- **References and Anchors**: To avoid data duplication.
- **Inline Schema Definitions**: Enforce data structure and types for integrity.
- **Modularization and Includes**: Organize configurations for better manageability.
- **Enhanced Data Types**: Direct support for dates and regex simplifies data handling.

These features aim to reduce redundancy, enforce data integrity, and improve developer efficiency, making JSONX a powerful yet familiar alternative to JSON.

---

## Key Improvements Over JSON

1. **Variables and Constants**

   - **Benefit**: Reuse values throughout the document, simplifying updates.

   ```jsonx
   const MAX_RETRIES = 5;

   {
     retries: MAX_RETRIES,
     backupRetries: MAX_RETRIES - 1,
   }
   ```

2. **References and Anchors**

   - **Benefit**: Avoid data duplication and keep configurations DRY.

   ```jsonx
   &defaultSettings {
     theme: "dark",
     notifications: true,
   }

   userSettings: {
     *defaultSettings,
     notifications: false,  // Override default
   }
   ```

3. **Inline Schema Definitions**

   - **Benefit**: Immediate validation of data structures, reducing runtime errors.

   ```jsonx
   schema User = {
     id: number,
     name: string,
     email: string,
     isActive: boolean,
   };

   User = {
     id: 101,
     name: "Alice",
     email: "alice@example.com",
     isActive: true,
   };
   ```

4. **Modularization and Includes**

   - **Benefit**: Organize configurations into separate files.

   ```jsonx
   include "users/admin.jsonx"
   include "users/guest.jsonx"

   users: [adminUser, guestUser]
   ```

5. **Enhanced Data Types**

   - **Benefit**: Direct support for dates and regex simplifies data handling.

   ```jsonx
   {
     releaseDate: date = 2023-11-30,
     validationPattern: regex = /^[a-z0-9_-]{3,16}$/,
   }
   ```


*This specification is open-source and available under the MIT License.*
