

# 🔐 Authentication & JWT (JSON Web Token)

## 📖 Overview

In this section, I learned how **JWT (JSON Web Tokens)** are used for authentication in a Node.js application. I also understood why passwords are hashed before storing them in the database and how npm version symbols (`^` and `~`) work inside `package.json`.


# 📚 Topics Covered

- Password Hashing
- JWT (JSON Web Token)
- JWT Structure
- JWT Security
- JWT Verification
- npm Versioning
- Caret (`^`) vs Tilde (`~`)

---

# 🔒 Why Hash Passwords?

Passwords should never be stored in plain text.

❌ Bad Example

```text
password123
```

If someone gets access to the database, they can easily see every user's password.

---

## ✅ Hashed Password

Instead of storing the real password, we store a **hashed version**.

Example:

```text
password123

↓

$2b$10$KR0tMiO/l6JH51Bipc3...
```

This hash is created using libraries such as **bcrypt**.

Hashing is **one-way**, which means:

- Password ➜ Hash ✅
- Hash ➜ Password ❌

It is impossible to convert the hash back into the original password.

---

## 🔑 Login Process

When a user logs in:

1. User enters a password.
2. The entered password is hashed again.
3. The new hash is compared with the stored hash.
4. If both hashes match, the login is successful.

---

# 🔐 What is JWT?

JWT stands for **JSON Web Token**.

A JWT is a secure token that is sent to the user after a successful login or registration.

Instead of asking the user to log in on every request, the server checks the JWT token.

---

# 🏗️ JWT Structure

A JWT consists of **three parts** separated by dots.

```text
Header.Payload.Signature
```

Example:

```text
xxxxx.yyyyy.zzzzz
```

---

## 1️⃣ Header

The header stores information about the signing algorithm.

Example:

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

---

## 2️⃣ Payload

The payload stores user information.

Example:

```json
{
  "id": 1,
  "name": "Himanshu",
  "email": "himanshu@gmail.com"
}
```

The server reads this information to identify the logged-in user.

> ⚠️ The payload is **encoded, not encrypted**, so anyone with the token can decode and read it.

**Never store:**

- Passwords
- OTPs
- Bank details
- Secret information

inside the payload.

---

## 3️⃣ Signature

The signature makes sure the token has not been modified.

It is created using:

- Header
- Payload
- Secret Key (`JWT_SECRET`)

Only the server knows the secret key.

---

# 🛡️ How JWT Security Works

Suppose someone changes:

```text
id: 5

↓

id: 1
```

to pretend they are another user.

Although they can edit the payload, they **cannot generate a valid signature** because they do not know the server's `JWT_SECRET`.

When the server runs:

```javascript
jwt.verify(token, process.env.JWT_SECRET);
```

The verification fails, and the request is rejected.

This makes JWT **tamper-proof**.

---

# 🔄 JWT Authentication Flow

```text
User Login

        │

        ▼

Email & Password

        │

        ▼

Verify Password

        │

        ▼

Generate JWT Token

        │

        ▼

Send Token to User

        │

        ▼

User Sends Token with Every Request

        │

        ▼

Server Verifies Token

        │

        ▼

Access Granted ✅
```

---

# 📦 Understanding npm Versions

Every dependency inside **package.json** has a version number.

Example:

```json
"express": "^5.1.0"
```

Version format:

```text
MAJOR.MINOR.PATCH
```

Example:

```text
4.17.21
```

---

# 📌 Version Symbols

## Exact Version

```json
"express": "4.17.21"
```

Only this version will be installed.

No automatic updates.

---

## Tilde (`~`)

```json
"express": "~4.17.21"
```

Allows **Patch updates only**.

Example:

✅ 4.17.22

✅ 4.17.25

❌ 4.18.0

---

## Caret (`^`)

```json
"express": "^4.17.21"
```

Allows:

- Minor updates
- Patch updates

Example:

✅ 4.17.22

✅ 4.18.0

❌ 5.0.0

This is the default behavior in npm.

---

# 📊 Version Comparison

| Symbol | Updates Allowed |
|----------|-----------------|
| `4.17.21` | Exact version only |
| `~4.17.21` | Patch updates only |
| `^4.17.21` | Minor + Patch updates |

---

# 🎯 Key Takeaways

- Passwords should always be hashed before storing them.
- Hashes cannot be converted back into passwords.
- JWT is used for authentication.
- A JWT has three parts: Header, Payload, and Signature.
- The payload is readable but should never contain sensitive information.
- The signature protects the token from tampering.
- `jwt.verify()` checks whether a token is valid.
- npm uses version symbols to control package updates.
- `^` allows Minor + Patch updates, while `~` allows only Patch updates.

---

# 🚀 What I Learned

- Secure password storage using bcrypt.
- How JWT authentication works.
- JWT structure and verification.
- Why JWT payload should never contain sensitive data.
- How token validation protects applications.
- Understanding npm versioning.
- Difference between Exact, `~`, and `^` versions.
