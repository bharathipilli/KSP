# 🧠 Module Compilation & Insertion (insmod, rmmod, lsmod, modinfo)

---

## 🔹 Overview

After writing your kernel module (e.g., `hello.c`), the next step is to **compile**, **insert**, **inspect**, and **remove** it safely.

Linux provides several utilities to manage **Loadable Kernel Modules (LKMs)** dynamically, without rebooting or rebuilding the kernel.

---

## 🧱 Key Module Management Commands

| 🧩 Command | 💡 Purpose |
|-------------|-------------|
| `make` | Compiles the module to produce `.ko` (Kernel Object) file |
| `sudo insmod <module>.ko` | Inserts a module into the kernel |
| `sudo rmmod <module>` | Removes a module from the kernel |
| `lsmod` | Lists all currently loaded modules |
| `modinfo <module>.ko` | Displays detailed information about a module |
| `modprobe <module>` | Inserts module along with its dependencies (recommended) |

---

## ⚙️ Step 1 – Compiling a Module

Every kernel module needs a **Makefile** to tell the kernel build system how to compile it.

### 🧩 Example: Makefile

```make
obj-m += hello.o

all:
	make -C /lib/modules/$(shell uname -r)/build M=$(PWD) modules

clean:
	make -C /lib/modules/$(shell uname -r)/build M=$(PWD) clean
````

### 🧠 Explanation

| Line                        | Meaning                                                 |
| --------------------------- | ------------------------------------------------------- |
| `obj-m += hello.o`          | Declares a loadable module named `hello.ko`             |
| `-C /lib/modules/.../build` | Points to the kernel source/build directory             |
| `M=$(PWD)`                  | Current working directory — where your module source is |
| `modules`                   | Target in the kernel build system to compile LKMs       |

### 🧰 Run Compilation

```bash
make
```

If successful, you’ll get a `hello.ko` file — this is your **compiled kernel module**.

---

## ⚙️ Step 2 – Inserting a Module (`insmod`)

To load your compiled `.ko` file into the kernel:

```bash
sudo insmod hello.ko
```

### ✅ Verify insertion

```bash
dmesg | tail
```

Expected Output:

```
[1234.567890] Hello Krish! Kernel module loaded successfully.
```

---

## ⚙️ Step 3 – Viewing Loaded Modules (`lsmod`)

List all currently loaded modules:

```bash
lsmod
```

Sample Output:

```
Module                  Size  Used by
hello                  16384  0
uvcvideo               98304  1
videobuf2_vmalloc      20480  1 uvcvideo
```

| Column      | Meaning                          |
| ----------- | -------------------------------- |
| **Module**  | Name of the loaded module        |
| **Size**    | Memory footprint (in bytes)      |
| **Used by** | Number of other modules using it |

> If your module doesn’t appear, insertion failed — check `dmesg` for error messages.

---

## ⚙️ Step 4 – Viewing Module Information (`modinfo`)

You can see module metadata embedded using macros like `MODULE_LICENSE()`, `MODULE_AUTHOR()`, etc.

```bash
modinfo hello.ko
```

Sample Output:

```
filename:       /home/krish/hello_module/hello.ko
license:        GPL
description:    Simple Hello World Kernel Module Example
author:         Krish
version:        1.0
srcversion:     1A2B3C4D5E6F7890ABCDE
depends:
retpoline:      Y
name:           hello
vermagic:       5.15.0-91-generic SMP mod_unload
```

---

## ⚙️ Step 5 – Removing a Module (`rmmod`)

To safely unload a module from the kernel:

```bash
sudo rmmod hello
```

Then verify removal:

```bash
dmesg | tail
```

Expected Output:

```
[1236.789012] Goodbye Krish! Kernel module removed.
```

> Always unload modules **before recompiling** or modifying them.
> A loaded `.ko` file cannot be recompiled or replaced.

---

## ⚙️ Step 6 – Using `modprobe` (Recommended)

Instead of `insmod`, you can use:

```bash
sudo modprobe hello
```

* Automatically resolves dependencies
* Searches module paths under `/lib/modules/$(uname -r)/`
* Automatically calls `depmod` if needed

To remove:

```bash
sudo modprobe -r hello
```

---

## 🧠 Typical Module Management Flow

```text
Write code  →  make  →  insmod  →  lsmod  →  modinfo  →  rmmod
```

---

## 🧩 Real Example Walkthrough

```bash
cd ~/hello_module/
make

sudo insmod hello.ko
dmesg | tail

lsmod | grep hello
modinfo hello.ko

sudo rmmod hello
dmesg | tail
```

---

## 🧱 Troubleshooting Common Issues

| ⚠️ Problem                                   | 🔍 Possible Cause / Fix                                                           |
| -------------------------------------------- | --------------------------------------------------------------------------------- |
| `invalid module format`                      | Mismatch between running kernel and build headers (`uname -r`)                    |
| `unknown symbol`                             | Missing dependency or exported symbol                                             |
| `permission denied`                          | Forgot `sudo`                                                                     |
| `rmmod: module is in use`                    | Module still being used by another process or driver                              |
| `make: *** No rule to make target 'modules'` | Missing kernel headers — install via `sudo apt install linux-headers-$(uname -r)` |

---

## 🧭 Summary Table

| Task                 | Command                                          | Description                       |
| -------------------- | ------------------------------------------------ | --------------------------------- |
| **Compile module**   | `make`                                           | Build `.ko` file                  |
| **Insert module**    | `sudo insmod hello.ko`                           | Load into kernel                  |
| **List modules**     | `lsmod`                                          | Show loaded modules               |
| **Inspect module**   | `modinfo hello.ko`                               | Display metadata                  |
| **Remove module**    | `sudo rmmod hello`                               | Unload from kernel                |
| **Auto load/unload** | `sudo modprobe hello` / `sudo modprobe -r hello` | Handle dependencies automatically |

---

## 🧠 Key Takeaways

* `.ko` = compiled kernel object (your driver/module)
* Use **`insmod`** and **`rmmod`** for manual testing
* Use **`modprobe`** for production or dependency handling
* Always check logs using **`dmesg`**
* Kernel modules run in **kernel space** — so debugging must be done carefully

---

