# Data Model Examples

This folder contains JSON examples of the **six top-level objects** in the IdentityNow data model. These objects are fundamental to understanding how IdentityNow organizes and searches for data.

Each object is represented as a JSON file. You can use these examples to better understand the structure of these objects and how to construct effective search queries.

---

## **Top-Level Objects**

### 1. Identities
Identities represent users or entities in your system. They have:
- **Direct Attributes**: Top-level fields like `id`, `name`, and `email`.
- **Nested Objects**: Attributes such as `country` or `department` are stored in a nested `attributes` object.
- **Arrays**: Collections like `@accounts`, `@access`, and `@apps`.

[View Example JSON](./identities.json)

---

### 2. Accounts
Accounts are the individual profiles linked to sources for a user. They typically include:
- **Direct Attributes**: Fields like `id` and `nativeIdentity`.
- **Attributes**: Metadata such as `status` or `email`.

[View Example JSON](./accounts.json)

---

### 3. Access Profiles
Access Profiles group entitlements and provide bundled access. These include:
- **Direct Fields**: Such as `id` and `name`.
- **Arrays**: List of entitlements included in the profile.

[View Example JSON](./accessProfiles.json)

---

### 4. Roles
Roles define sets of permissions or responsibilities and are often assigned to groups of users:
- **Direct Attributes**: Fields like `id` and `name`.
- **Arrays**: Such as `entitlements` and `members`.

[View Example JSON](./roles.json)

---

### 5. Entitlements
Entitlements represent granular permissions for systems or applications. They include:
- **Direct Attributes**: Such as `id` and `name`.
- **Attributes**: Metadata like `application` and `description`.

[View Example JSON](./entitlements.json)

---

### 6. Sources
Sources represent systems of record, such as Active Directory or Salesforce. They include:
- **Direct Attributes**: Such as `id` and `name`.
- **Arrays**: Such as linked accounts for identities.

[View Example JSON](./sources.json)

---

## **Key Concepts**

### **1. Direct Fields**
Direct fields are attributes at the top level of an object. These fields can be searched directly in queries.  
**Example**:  
To find an Identity named "Bob Smith":  
`name:Bob.Smith`

---

### **2. Nested Objects**
Nested objects group related fields inside an object. They are represented using curly braces `{}` in JSON.  
**Example**:  
For an Identity's country, which is stored in the `attributes` object, the search syntax is:  
`attributes.country:USA`

---

### **3. Arrays**
Arrays represent collections of similar objects related to a parent object. These are denoted using square brackets `[]` in JSON.  
**Example**:  
The `@access` array in an Identity object contains all the roles, entitlements, and access profiles assigned to the user.

**Search Tip**: Arrays are searched using the `@` prefix.  
To find identities with a specific entitlement:  
`@access(type:ENTITLEMENT AND displayName:Employee)`

---

### **4. The `@` Prefix**
The `@` prefix in IdentityNow indicates a **collection or array**. When searching within an array, use the `@` prefix to specify the array name.

- **Example 1**:  
  Search for an entitlement within the `@access` array:  
  `@access(type:ROLE AND displayName:Manager)`

- **Example 2**:  
  Search for a specific account in the `@accounts` array:  
  `@accounts(nativeIdentity:bsmith)`

---

## **How to Use These Examples**
1. Open any of the JSON files to see how the object is structured.
2. Cross-reference the fields and arrays in the JSON with the queries provided in this `README.md`.
3. Use this knowledge to construct and test your own search queries in IdentityNow.

---

## **Further Reading**
For more details, refer to the official IdentityNow documentation on the data model and search syntax.
