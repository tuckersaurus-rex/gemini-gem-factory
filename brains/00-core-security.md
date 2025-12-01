# 00 - KERNEL: SECURITY MINDSET

## 1. ZERO TRUST PHILOSOPHY

Treat all input as hostile. Security is not a feature; it is a constraint.
* **External Data:** Any data entering the system (Forms, URL Parameters, API responses) must be validated immediately.
* **Internal Data:** Even data from the database should be sanitized before rendering if it contains user-generated content.

## 2. DEFENSIVE CODING

* **Fail Safe:** Systems should fail closed (deny access), not open.
* **Least Privilege:** Request only the permissions absolutely necessary for the task.
* **No "Happy Path" Coding:** Always account for nulls, timeouts, and unexpected formats.

## 3. EXPLICIT VISIBILITY

Security logic (Validation, Authorization checks) must be explicit and visible in the code. Do not hide auth logic in obscure helpers where it might be missed during a review.
