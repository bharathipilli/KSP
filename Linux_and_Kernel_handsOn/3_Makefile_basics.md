
# Makefile Basics (Static & Dynamic Libraries)

---

## 🔹 What is a Makefile?
A **Makefile** is a file used by the `make` utility to:
- Automate compilation and linking of programs.
- Avoid recompiling everything (only rebuild changed parts).
- Define rules for building executables, static libraries, and shared libraries.

⚡ Example of running:
```bash
make
````

---

## 🔹 What is a Library?

A **library** is a collection of precompiled code (functions, classes, etc.) that can be reused by multiple programs.

Two main types:

1. **Static Library (.a)**
2. **Dynamic (Shared) Library (.so)**

---

## 1. Static Libraries (`.a`)

### ✅ Definition:

* A **static library** is linked into the final executable **at compile time**.
* All required functions are copied into the binary.

### ⚡ Example:

Create static library:

```bash
gcc -c math_utils.c -o math_utils.o
ar rcs libmath.a math_utils.o
```

Link with static library:

```bash
gcc main.c -L. -lmath -o app_static
```

### 🔎 Why use:

* No runtime dependencies.
* Binary is self-contained.
* Faster execution (no dynamic linking overhead).

### ❌ Downsides:

* Larger binary size.
* Updating library requires recompilation of the app.

---

## 2. Dynamic Libraries (`.so`)

### ✅ Definition:

* A **dynamic (shared) library** is **not copied** into the executable.
* Instead, the program contains references, and the library is loaded at **runtime**.

### ⚡ Example:

Create shared library:

```bash
gcc -fPIC -c math_utils.c -o math_utils.o
gcc -shared -o libmath.so math_utils.o
```

Link with shared library:

```bash
gcc main.c -L. -lmath -o app_dynamic
```

Run with `LD_LIBRARY_PATH` if needed:

```bash
LD_LIBRARY_PATH=. ./app_dynamic
```

### 🔎 Why use:

* Saves disk space (one `.so` shared by many programs).
* Easy to update library (fix bugs without recompiling app).

### ❌ Downsides:

* Program won’t run if required `.so` is missing or incompatible.
* Slightly slower (symbol resolution at runtime).

---

## 3. Types of Libraries in Makefiles

In Makefiles, you may define rules for:

1. **Object files (`.o`)**
   Compiled source code, intermediate files.

   ```make
   math_utils.o: math_utils.c
       gcc -c math_utils.c -o math_utils.o
   ```

2. **Static library (`.a`)**
   Archiving `.o` files.

   ```make
   libmath.a: math_utils.o
       ar rcs libmath.a math_utils.o
   ```

3. **Shared library (`.so`)**
   Position-independent code + linking.

   ```make
   libmath.so: math_utils.o
       gcc -shared -o libmath.so math_utils.o
   ```

4. **Executable**
   Linking either static or dynamic libs.

   ```make
   app_static: main.o libmath.a
       gcc main.o -L. -lmath -o app_static

   app_dynamic: main.o libmath.so
       gcc main.o -L. -lmath -o app_dynamic
   ```

---

## 4. Why Use Makefiles with Libraries?

* **Automation** → no need to type long gcc commands each time.
* **Dependency management** → only rebuild changed code.
* **Portability** → same Makefile works across systems.
* **Scalability** → essential for large projects with many `.c` files and multiple libraries.

---

## ✅ Summary

* **Static libraries (`.a`)** → linked into program, portable but bigger size.
* **Dynamic libraries (`.so`)** → loaded at runtime, smaller but need correct library installed.
* **Makefile** automates the creation of both and manages dependencies.
---

Would you like me to also **write a sample full Makefile** (with targets for `.a`, `.so`, and executables) so you can test static vs dynamic linking directly?
```
