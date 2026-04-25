
# 🧠 Writing a Simple “Hello World” Kernel Module

---

## 🔹 What Is a Kernel Module?

A **kernel module** (also called a **Loadable Kernel Module – LKM**) is a piece of code that can be **loaded into or removed from the Linux kernel at runtime**.  
It extends the kernel’s functionality **without rebuilding or rebooting** the entire kernel.

> A “Hello World” module is the first step for anyone learning **Linux kernel programming**.

---

## 🧩 Why Write a Kernel Module?

| 🎯 Purpose | 💡 Explanation |
|-------------|----------------|
| **Learn kernel programming basics** | Understand how modules are initialized and cleaned up |
| **Test environment setup** | Verify kernel headers, Makefile, and build system |
| **Foundation for real drivers** | All device drivers start from this base |
| **Dynamic loading** | Practice using `insmod`, `rmmod`, and `dmesg` |

---

## ⚙️ Folder Structure

```

  hello_module/
  ├── Makefile
  └── hello.c

```

---

## 🧱 Step 1 – The Hello World Code

Create a file named **`hello.c`**:

```c
#include <linux/module.h>   // Needed by all modules
#include <linux/init.h>     // Needed for macros like module_init/module_exit

// Function that runs when module is loaded
static int __init hello_init(void)
{
    pr_info("Hello Krish! Kernel module loaded successfully.\n");
    return 0; // 0 = success
}

// Function that runs when module is removed
static void __exit hello_exit(void)
{
    pr_info("Goodbye Krish! Kernel module removed.\n");
}

// Register module entry and exit points
module_init(hello_init);
module_exit(hello_exit);

// Module metadata
MODULE_LICENSE("GPL");
MODULE_AUTHOR("Krish");
MODULE_DESCRIPTION("Simple Hello World Kernel Module Example");
MODULE_VERSION("1.0");
````

---

## 🧱 Step 2 – The Makefile

Create a file named **`Makefile`** in the same folder:

```make
obj-m += hello.o

all:
	make -C /lib/modules/$(shell uname -r)/build M=$(PWD) modules

clean:
	make -C /lib/modules/$(shell uname -r)/build M=$(PWD) clean
```

> This Makefile tells the kernel build system to compile our module (`hello.c`)
> against the currently running kernel version.

---

## ⚙️ Step 3 – Build the Module

Open a terminal in the module’s folder:

```bash
make
```

If successful, you’ll see files like:

```
hello.ko   hello.mod.c   hello.mod.o   hello.o   Module.symvers
```

* `.ko` → Kernel Object (the compiled module file you can load)
* `.o` → Object file
* `.mod.c` → Auto-generated metadata file

---

## 🧩 Step 4 – Insert and Remove the Module

### 🔹 Load the module into the kernel

```bash
sudo insmod hello.ko
```

### 🔹 Check the kernel logs

```bash
dmesg | tail
```

Expected Output:

```
[ 1234.567890] Hello Krish! Kernel module loaded successfully.
```

### 🔹 Verify that module is loaded

```bash
lsmod | grep hello
```

### 🔹 Remove the module

```bash
sudo rmmod hello
```

### 🔹 Confirm removal

```bash
dmesg | tail
```

Expected Output:

```
[ 1236.789012] Goodbye Krish! Kernel module removed.
```

---

## 🧠 What Happens Internally

1. When you run `insmod hello.ko`,
   → the kernel executes the function **`hello_init()`**.
2. When you run `rmmod hello`,
   → the kernel executes the function **`hello_exit()`**.
3. Kernel logs are printed via **`pr_info()`**, visible using `dmesg`.

---

## 🔍 Common Commands Summary

| Command                | Purpose                 |
| ---------------------- | ----------------------- |
| `make`                 | Compile the module      |
| `sudo insmod hello.ko` | Insert the module       |
| `sudo rmmod hello`     | Remove the module       |
| `lsmod`                | List all loaded modules |
| `modinfo hello.ko`     | Show module information |
| `dmesg`                | View kernel logs        |

---

## 🧩 Notes & Tips

* Kernel modules **must** be compiled against the same kernel version that’s currently running.
  Use `uname -r` to check your version.
* The kernel uses `printk()` internally, but `pr_info()` is a safer macro for logging.
* Always include a valid `MODULE_LICENSE()` —
  otherwise, the kernel marks it as “tainted” (non-GPL code warning).
* For debugging, you can use:

  ```bash
  sudo dmesg -w
  ```

  to view logs in real time.

---

## 🧭 Typical Workflow Recap

```text
Write code  →  make  →  insmod  →  dmesg  →  rmmod  →  dmesg
```

---

## 🧠 Key Takeaways

| Concept                         | Meaning                                                    |
| ------------------------------- | ---------------------------------------------------------- |
| **Kernel Module**               | Piece of code dynamically loaded into the kernel           |
| **hello_init() / hello_exit()** | Functions executed during load/unload                      |
| **.ko File**                    | The compiled module                                        |
| **dmesg**                       | Displays kernel logs                                       |
| **MODULE_LICENSE("GPL")**       | Allows use of kernel symbols legally                       |
| **No main() function**          | Kernel modules don’t have main(), they use init/exit hooks |

---

