JSONX (JSON Extended) Enhanced Specification
Version: 1.0

Table of Contents
Introduction
Key Features
Syntax Overview
Data Types
Detailed Specifications
Comments
Unquoted Keys
Optional Commas
Trailing Commas
Multi-line Strings
Variables and Constants
References and Anchors
Inline Schema Definitions
Modularization and Includes
Enhanced Data Types
Examples
Advanced Configuration Example
Compatibility
Conclusion
Introduction
JSONX is designed to be a significant leap forward from JSON, introducing powerful features that enhance readability, maintainability, and expressiveness. It aims to reduce redundancy, enforce data integrity, and improve developer efficiency while maintaining compatibility with existing JSON data.

Key Features
Comments: Single-line and multi-line comments.
Unquoted Keys: Simplifies object notation.
Optional Commas and Trailing Commas: Cleaner syntax.
Multi-line Strings: Easy inclusion of large text blocks.
Variables and Constants: Define reusable values.
References and Anchors: Avoid data duplication.
Inline Schema Definitions: Enforce data structure and types.
Modularization and Includes: Import external files.
Enhanced Data Types: Support for dates, regex, and more.
Syntax Overview
JSONX extends JSON syntax to include:

Comments: //, #, /* ... */
Variables: Defined using let and const
References: Using &anchor and *reference
Includes: Import external JSONX files
Type Annotations: For inline schema definitions
Extended Data Types: Dates, regex, etc.
Data Types
Objects
Arrays
Strings
Numbers: Including hex (0x), binary (0b), and octal (0o)
Booleans
Null
Dates
Regex
Variables and Constants
Detailed Specifications
Comments
Same as previous specification.

Unquoted Keys
Same as previous specification.

Optional Commas and Trailing Commas
Same as previous specification.

Multi-line Strings
Same as previous specification.

Variables and Constants
Definition
Variables: Defined using let.
Constants: Defined using const.
Scope: Variables and constants are available throughout the file after declaration.
Usage
Variables can store any valid data type.
Constants are immutable after declaration.
Example
jsonx
Copy code
const API_URL = "https://api.example.com";
let timeout = 30;  // in seconds

{
  endpoint: API_URL + "/users",
  retryTimeout: timeout,
}
References and Anchors
Anchors (&)
Define reusable data blocks.
Syntax: &anchorName { ... }
References (*)
Reuse data blocks defined by anchors.
Syntax: *anchorName
Example
jsonx
Copy code
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
Inline Schema Definitions
Type Annotations
Use colons to define types: key: type = value
Supported types: string, number, boolean, object, array, date, regex, null
Schema Enforcement
Parsers can validate data against the defined types.
Optional but recommended for data integrity.
Example
jsonx
Copy code
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
Modularization and Includes
Include Files
Use include "path/to/file.jsonx"
Supports relative and absolute paths.
Included files are merged into the current scope.
Example
jsonx
Copy code
// In main.jsonx
include "config/database.jsonx"
include "config/server.jsonx"

{
  appName: "MyApplication",
  version: "1.0.0",
  database: databaseConfig,  // From database.jsonx
  server: serverConfig,      // From server.jsonx
}
Enhanced Data Types
Dates
Recognized using ISO 8601 format.
Type annotated as date.
Regular Expressions
Defined using /pattern/ syntax.
Type annotated as regex.
Example
jsonx
Copy code
{
  passwordPattern: regex = /^(?=.*\d)(?=.*[a-z])(?=.*[A-Z]).{8,}$/,
  eventDate: date = 2023-12-31T23:59:59Z,
}
Examples
Advanced Configuration Example
jsonx
Copy code
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
Compatibility
Backward Compatibility: Valid JSON documents are valid JSONX documents.
Forward Compatibility: JSONX parsers should process standard JSON without issues.
Parsing Requirements: Parsers need to support extended syntax and features.
Conclusion
JSONX offers substantial improvements over JSON by introducing variables, references, inline schemas, modularization, and enhanced data types. These features aim to:

Reduce Redundancy: Variables and references prevent data duplication.
Improve Maintainability: Modularization and includes allow for cleaner project structures.
Enhance Data Integrity: Inline schemas and type annotations enforce data correctness.
Increase Developer Productivity: Advanced features simplify configuration and data management tasks.
By addressing common pain points in JSON, JSONX provides a powerful yet familiar alternative that can significantly boost developer efficiency and satisfaction.

Key Improvements Over JSON
Variables and Constants

Benefit: Reuse values throughout the document, making updates easier and reducing errors.

Example:

jsonx
Copy code
const MAX_RETRIES = 5;

{
  retries: MAX_RETRIES,
  backupRetries: MAX_RETRIES - 1,
}
References and Anchors

Benefit: Avoid data duplication and keep configurations DRY (Don't Repeat Yourself).

Example:

jsonx
Copy code
&defaultSettings {
  theme: "dark",
  notifications: true,
}

userSettings: {
  *defaultSettings,
  notifications: false,  // Override default
}
Inline Schema Definitions

Benefit: Immediate validation of data structures, reducing runtime errors.

Example:

jsonx
Copy code
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
Modularization and Includes

Benefit: Organize configurations into separate files, enhancing readability and manageability.

Example:

jsonx
Copy code
include "users/admin.jsonx"
include "users/guest.jsonx"

users: [adminUser, guestUser]
Enhanced Data Types

Benefit: Direct support for dates and regex simplifies data handling.

Example:

jsonx
Copy code
{
  releaseDate: date = 2023-11-30,
  validationPattern: regex = /^[a-z0-9_-]{3,16}$/,
}
