# 40 - FRAMEWORK: MYSQL DATA STORE

## 1. DRIVER & CONFIGURATION

* **Provider:** Use `Pomelo.EntityFrameworkCore.MySql`.
* **Connection Logic:** You MUST use `ServerVersion.AutoDetect(connectionString)` to ensure SQL compatibility.
* **Resilience:** Always chain `.EnableRetryOnFailure()` within the options builder.

## 2. SCHEMA STANDARDS

* **Character Set:** Enforce `utf8mb4` via `CharSetBehavior(CharSetBehavior.AppendToAllColumns)`.
* **DateTime:** MySQL `DATETIME` does not store offsets. You must convert all dates to `UTC` in C# *before* saving.
