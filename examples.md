# **JSON Object Template Examples**

The following examples include a `Template`, an `Input`, and an `Output`

```
Output = TemplateEngine.run(Template, Input)
```
---

### **1. JSONPath Expressions**

**Template:**
```json
{
  "userName": "$.user.name",
  "firstBookTitle": "$.books[0].title"
}
```

**Input:**
```json
{
  "user": {
    "name": "Alice",
    "age": 30
  },
  "books": [
    { "title": "Book One", "author": "Author A" },
    { "title": "Book Two", "author": "Author B" }
  ]
}
```

**Output:**
```json
{
  "userName": "Alice",
  "firstBookTitle": "Book One"
}
```

---

### **2. Destructuring**

#### **2.1 Object Destructuring**

**Template:**
```json
{
  "userInfo": {
    "{name, age: yearsOld, city = 'Unknown', ...extra}": "$.user"
  }
}
```

**Input:**
```json
{
  "user": {
    "name": "Alice",
    "age": 30,
    "email": "alice@example.com"
  }
}
```

**Output:**
```json
{
  "userInfo": {
    "name": "Alice",
    "yearsOld": 30,
    "city": "Unknown",
    "extra": {
      "email": "alice@example.com"
    }
  }
}
```

#### **2.2 Array Destructuring**

**Template:**
```json
{
  "coordinates": {
    "[latitude, longitude]": "$.location.coordinates"
  }
}
```

**Input:**
```json
{
  "location": {
    "coordinates": [37.7749, -122.4194]
  }
}
```

**Output:**
```json
{
  "coordinates": {
    "latitude": 37.7749,
    "longitude": -122.4194
  }
}
```

---

### **3. Spread Operators**

#### **3.1 `@merge` (Merging Objects)**

**Template:**
```json
{
  "fullUserData": {
    "@merge": [
      "$.userDetails",
      "$.accountInfo",
      {
        "signupDate": "2023-01-01"
      }
    ]
  }
}
```

**Input:**
```json
{
  "userDetails": {
    "name": "Alice",
    "email": "alice@example.com"
  },
  "accountInfo": {
    "accountType": "premium",
    "status": "active"
  }
}
```

**Output:**
```json
{
  "fullUserData": {
    "name": "Alice",
    "email": "alice@example.com",
    "accountType": "premium",
    "status": "active",
    "signupDate": "2023-01-01"
  }
}
```

---

#### **3.2 `@concat` (Concatenating Arrays)**

**Template:**
```json
{
  "allItems": {
    "@concat": [
      "$.cartItems",
      "$.wishlistItems",
      [
        {
          "item": "Gift Card",
          "value": 50
        }
      ]
    ]
  }
}
```

**Input:**
```json
{
  "cartItems": [
    { "item": "Book", "price": 15.00 },
    { "item": "Pen", "price": 1.50 }
  ],
  "wishlistItems": [
    { "item": "Notebook", "price": 5.00 }
  ]
}
```

**Output:**
```json
{
  "allItems": [
    { "item": "Book", "price": 15.00 },
    { "item": "Pen", "price": 1.50 },
    { "item": "Notebook", "price": 5.00 },
    { "item": "Gift Card", "value": 50 }
  ]
}
```

---

#### **3.3 `"..."` Shorthand for Spread Operators, can be used in place of @merge and @concat and detects if objects or arrays**

**Template:**
```json
{
  "fullUserData": {
    "...": [
      "$.userDetails",
      "$.accountInfo",
      {
        "signupDate": "2023-01-01"
      }
    ]
  }
}
```

**Input:**

```json
{
  "userDetails": {
    "name": "Alice",
    "email": "alice@example.com"
  },
  "accountInfo": {
    "accountType": "premium",
    "status": "active"
  }
}
```

**Output:**
```json
{
  "fullUserData": {
    "name": "Alice",
    "email": "alice@example.com",
    "accountType": "premium",
    "status": "active",
    "signupDate": "2023-01-01"
  }
}
```

---

### **4. Escape Characters**

**Template:**
```json
{
  "escapedExpression": "\\$.user.name"
}
```

**Input:**
```json
{}
```

**Output:**
```json
{
  "escapedExpression": "$.user.name"
}
```

---

### **5. `@literal` Operator**

**Template:**
```json
{
  "literalContent": {
    "@literal": {
      "expression": "$.user.name"
    }
  }
}
```

**Input:**
```json
{}
```

**Output:**
```json
{
  "literalContent": {
    "expression": "$.user.name"
  }
}
```

---

### **6. String Manipulation Operators**

#### **6.1 `@regexReplace`**

**Template:**
```json
{
  "sanitizedEmail": {
    "@regexReplace": {
      "path": "$.email",
      "pattern": "@example.com",
      "replacement": "@newdomain.com"
    }
  }
}
```

**Input:**
```json
{
  "email": "alice@example.com"
}
```

**Output:**
```json
{
  "sanitizedEmail": "alice@newdomain.com"
}
```

---

#### **6.2 `@stringSplit`**

**Template:**
```json
{
  "words": {
    "@stringSplit": {
      "path": "$.sentence",
      "delimiter": " "
    }
  }
}
```

**Input:**
```json
{
  "sentence": "This is an example"
}
```

**Output:**
```json
{
  "words": ["This", "is", "an", "example"]
}
```

---

#### **6.3 `@arrayJoin`**

**Template:**
```json
{
  "joinedWords": {
    "@arrayJoin": {
      "path": "$.words",
      "delimiter": ", "
    }
  }
}
```

**Input:**
```json
{
  "words": ["This", "is", "an", "example"]
}
```

**Output:**
```json
{
  "joinedWords": "This, is, an, example"
}
```

---

### **7. Parsing and Serializing Operators**

#### **7.1 `@jsonParse`**

**Template:**
```json
{
  "parsedData": {
    "@jsonParse": {
      "path": "$.jsonString"
    }
  }
}
```

**Input:**
```json
{
  "jsonString": "{\"name\": \"Alice\", \"age\": 30}"
}
```

**Output:**
```json
{
  "parsedData": {
    "name": "Alice",
    "age": 30
  }
}
```

---

#### **7.2 `@jsonSerialize`**

**Template:**
```json
{
  "serializedUser": {
    "@jsonSerialize": {
      "path": "$.user"
    }
  }
}
```

**Input:**
```json
{
  "user": {
    "name": "Alice",
    "age": 30
  }
}
```

**Output:**
```json
{
  "serializedUser": "{\"name\":\"Alice\",\"age\":30}"
}
```

---

#### **7.3 `@csvParse`**

**Template:**
```json
{
  "parsedCSV": {
    "@csvParse": {
      "path": "$.csvString",
      "delimiter": ","
    }
  }
}
```

**Input:**
```json
{
  "csvString": "name,age\nAlice,30\nBob,25"
}
```

**Output:**
```json
{
  "parsedCSV": [
    { "name": "Alice", "age": "30" },
    { "name": "Bob", "age": "25" }
  ]
}
```

---

#### **7.4 `@csvSerialize`**

**Template:**
```json
{
  "csvString": {
    "@csvSerialize": {
      "path": "$.dataArray",
      "delimiter": ","
    }
  }
}
```

**Input:**
```json
{
  "dataArray": [
    { "name": "Alice", "age": 30 },
    { "name": "Bob", "age": 25 }
  ]
}
```

**Output:**
```json
{
  "csvString": "name,age\nAlice,30\nBob,25"
}
```

---

#### **7.

5 `@iniParse`**

**Template:**
```json
{
  "parsedINI": {
    "@iniParse": {
      "path": "$.iniString"
    }
  }
}
```

**Input:**
```json
{
  "iniString": "[user]\nname=Alice\nage=30\n[settings]\ntheme=dark"
}
```

**Output:**
```json
{
  "parsedINI": {
    "user": {
      "name": "Alice",
      "age": "30"
    },
    "settings": {
      "theme": "dark"
    }
  }
}
```

---

#### **7.6 `@iniSerialize`**

**Template:**
```json
{
  "iniString": {
    "@iniSerialize": {
      "path": "$.data"
    }
  }
}
```

**Input:**
```json
{
  "data": {
    "user": {
      "name": "Alice",
      "age": 30
    },
    "settings": {
      "theme": "dark"
    }
  }
}
```

**Output:**
```json
{
  "iniString": "[user]\nname=Alice\nage=30\n[settings]\ntheme=dark"
}
```

---

### **8. HTTP Fetch Operator**

#### **8.1 `@httpFetch`**

**Template:**
```json
{
  "apiData": {
    "@httpFetch": {
      "url": "https://api.example.com/users/1",
      "method": "GET",
      "headers": {
        "Authorization": "Bearer your_token_here"
      }
    }
  }
}
```

**Output:**

_(fetched from API)_:
```json
{
  "apiData": {
    "id": 1,
    "name": "Alice",
    "age": 30
  }
}
```

---

### **9. No Intermediate Values (Sequential Execution Only)**

#### **Phase 1: JSON Parsing**

**Template 1:**
```json
{
  "transformedData": {
    "@merge": [
      "$.inputData",
      {
        "status": "active"
      }
    ]
  }
}
```

**Input:**
```json
{
  "inputData": {
    "name": "Dana",
    "email": "dana@example.com"
  }
}
```

**Output:**
```json
{
  "transformedData": {
    "name": "Dana",
    "email": "dana@example.com",
    "status": "active"
  }
}
```

---

#### **Phase 2: Validation of Transformed Data**

**Template 2:**
```json
{
  "validatedData": {
    "@jsonSchemaValidate": {
      "path": "$.transformedData",
      "schema": {
        "type": "object",
        "properties": {
          "name": { "type": "string" },
          "email": { "type": "string", "format": "email" },
          "status": { "type": "string", "enum": ["active", "inactive"] }
        },
        "required": ["name", "email", "status"]
      },
      "onError": {
        "message": "Validation failed",
        "errors": "$errors"
      }
    }
  }
}
```

**Input (from Phase 1 Output):**
```json
{
  "transformedData": {
    "name": "Dana",
    "email": "dana@example.com",
    "status": "active"
  }
}
```

**Output:**
```json
{
  "validatedData": {
    "name": "Dana",
    "email": "dana@example.com",
    "status": "active"
  }
}
```

---

### **10. JSON Schema Validation Operator**

#### **10.1 Example 1: Validating User Data**

**Template:**
```json
{
  "validatedUser": {
    "@jsonSchemaValidate": {
      "path": "$.userData",
      "schema": {
        "type": "object",
        "properties": {
          "name": { "type": "string" },
          "email": { "type": "string", "format": "email" },
          "age": { "type": "integer", "minimum": 0 }
        },
        "required": ["name", "email"]
      },
      "onError": null
    }
  }
}
```

**Input:**
```json
{
  "userData": {
    "name": "Alice",
    "email": "alice@example.com",
    "age": 30
  }
}
```

**Output:**
```json
{
  "validatedUser": {
    "name": "Alice",
    "email": "alice@example.com",
    "age": 30
  }
}
```

---

#### **10.2 Example 2: Handling Validation Errors**

**Template:**
```json
{
  "validatedUser": {
    "@jsonSchemaValidate": {
      "path": "$.userData",
      "schema": {
        "type": "object",
        "properties": {
          "name": { "type": "string" },
          "email": { "type": "string", "format": "email" },
          "age": { "type": "integer", "minimum": 0 }
        },
        "required": ["name", "email"]
      },
      "onError": {
        "message": "User data validation failed",
        "errors": "$errors"
      }
    }
  }
}
```

**Input:**
```json
{
  "userData": {
    "name": "Bob",
    "email": "bob_at_example.com",
    "age": -5
  }
}
```

**Output:**
```json
{
  "validatedUser": {
    "message": "User data validation failed",
    "errors": [
      {
        "field": "email",
        "message": "Invalid email format"
      },
      {
        "field": "age",
        "message": "Value must be greater than or equal to 0"
      }
    ]
  }
}
```

---

#### **10.3 Example 3: Using a Schema Reference**

**Template:**
```json
{
  "validatedUser": {
    "@jsonSchemaValidate": {
      "path": "$.userData",
      "schema": "$.schemas.userSchema",
      "onError": null
    }
  }
}
```

**Input:**
```json
{
  "userData": {
    "name": "Charlie",
    "email": "charlie@example.com"
  },
  "schemas": {
    "userSchema": {
      "type": "object",
      "properties": {
        "name": { "type": "string" },
        "email": { "type": "string", "format": "email" },
        "age": { "type": "integer", "minimum": 0 }
      },
      "required": ["name", "email", "age"]
    }
  }
}
```

**Output:**
```json
{
  "validatedUser": null
}
```

---

#### **10.4 Example 4: Validating Nested Data**

**Template:**
```json
{
  "validatedOrder": {
    "@jsonSchemaValidate": {
      "path": "$.order",
      "schema": {
        "type": "object",
        "properties": {
          "orderId": { "type": "string" },
          "customer": {
            "type": "object",
            "properties": {
              "name": { "type": "string" },
              "email": { "type": "string", "format": "email" }
            },
            "required": ["name", "email"]
          },
          "items": {
            "type": "array",
            "items": {
              "type": "object",
              "properties": {
                "productId": { "type": "string" },
                "quantity": { "type": "integer", "minimum": 1 }
              },
              "required": ["productId", "quantity"]
            },
            "minItems": 1
          }
        },
        "required": ["orderId", "customer", "items"]
      },
      "onError": {
        "message": "Order validation failed",
        "errors": "$errors"
      }
    }
  }
}
```

**Input:**
```json
{
  "order": {
    "orderId": "ORD12345",
    "customer": {
      "name": "Eve",
      "email": "eve@example.com"
    },
    "items": [
      {
        "productId": "PROD1",
        "quantity": 2
      },
      {
        "productId": "PROD2",
        "quantity": 0
      }
    ]
  }
}
```

**Output:**
```json
{
  "validatedOrder": {
    "message": "Order validation failed",
    "errors": [
      {
        "field": "items[1].quantity",
        "message": "Value must be greater than or equal to 1"
      }
    ]
  }
}
```

---

### **11. Complex Template**

#### **11.1 Example 1: Concat, JSON parse, Destructure, Merge, Regex Replace**

**Template:**
```json
{
  "userInfo": {
    "{name, age, accountType, signupDate, email: sanitizedEmail, ...extra}": {
      "...": [
        "$.userDetails",
        "$.accountInfo",
        { "@jsonParse": { "path": "$.additionalData" }},
        {
          "email": {
            "@regexReplace": {
              "path": "$.userDetails.email",
              "pattern": "@example.com",
              "replacement": "@testdomain.com"
            }
          },
          "signupDate": "2023-01-01"
        }
      ]
    }
  },
  "allItems": {
    "...": [
      "$.cartItems",
      "$.wishlistItems"
    ]
  },
}
```

**Input:**
```json
{
  "userDetails": {
    "name": "Alice",
    "age": 30,
    "email": "alice@example.com"
  },
  "accountInfo": {
    "accountType": "premium",
    "status": "active"
  },
  "cartItems": [
    { "item": "Book", "price": 15.00 },
    { "item": "Pen", "price": 1.50 }
  ],
  "wishlistItems": [
    { "item": "Notebook", "price": 5.00 }
  ],
  "additionalData": "{\"preferences\": {\"theme\": \"dark\", \"notifications\": true}}"
}
```

**Output:**
```json
{
  "userInfo": {
    "name": "Alice",
    "age": 30,
    "accountType": "premium",
    "signupDate": "2023-01-01",
    "sanitizedEmail": "alice@testdomain.com",
    "extra": {
      "status": "active",
      "preferences": {
        "theme": "dark",
        "notifications": true
      },
    }
  },
  "allItems": [
    { "item": "Book", "price": 15.00 },
    { "item": "Pen", "price": 1.50 },
    { "item": "Notebook", "price": 5.00 }
  ]
}
```

---

### **12. Template Control Flow**

#### **12.1 Example 1: Template Sequence for multiple API calls, Define Custom API Operators, Disallow @httpFetch **

**Template:**
```json
{"@seq": [
// Begin Operator Setup
  //Define custom operators to make specific API calls and merge the response into the input data
  {"@def": {
    "@callAuthenticate": {
      "...": ["$", {
        "authentication": {
          "@httpFetch": {
            "url": "https://api.example.com/authenticate",
            "method": "POST",
            "headers": {
              "Content-Type": "application/json"
            },
            "body": {
              "username": "$.username",
              "password": "$.password"
            },
            "fullResponse": true
          }
        }
      }]
    },
    "@callUserInfo": {
      "...": ["$", {
        "userInfo": {
          "{email, fullName, preferences}": {
            "@httpFetch": {
              "url": "https://api.example.com/userinfo",
              "method": "GET",
              "headers": {
                "Authorization": "Bearer $.authToken"
              }
            }
          }
        }
      }]
    }}},
    // Undefine the @httpFetch operator to disallow any other http requests
    {"@undef": "@httpFetch"},
// End Operator setup

// Begin Template Operations
    {"@callAuthenticate": "$"},
    // Extract authToken and userId, merge with current input data
    {"...": ["$",
      {
        "authToken": "$.authentication.headers['Auth-Token']",
        "userId": "$.authentication.body.userId"
      }
    ]},
    // Check auth result then fetch user info and re-structure final result to simple flat map
    {"@if": "$.authToken",
      "@then": {"{username, userInfo: {email, fullName, preferences}}": {"@callUserInfo": "$"}},
      "@else": {"error": "Not Authorized"}
    }
// End Template Operations
  ]
}
```

**Input:**
```json
{
  username: "username123",
  password: "password123",
}
```

**Output:**
```json
{
  "username": "username123",
  "email": "email",
  "fullName": "full name",
  "preferences": []
}
```