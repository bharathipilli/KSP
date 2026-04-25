# 🧠 The `printk()` Function – Explained

---

## 🔹 What is `printk()`?

In user-space, we use `printf()` to print output to the terminal.  
In **kernel-space**, you **cannot use `printf()`** — because there is **no standard I/O** (no stdout or stderr).

So, the kernel provides its own logging function:

```c
printk();
````

> `printk()` is the **kernel’s version of printf()**, used to print messages to the **kernel log buffer**, which can be viewed using the `dmesg` command.

---

## ⚙️ Basic Syntax

```c
printk("<log_level>Message string\n", args...);
```

Example:

```c
printk(KERN_INFO "Hello Krish! Kernel module loaded.\n");
```

---

## 🧩 Key Points

| Concept                   | Description                                                          |
| ------------------------- | -------------------------------------------------------------------- |
| **Header File**           | `<linux/kernel.h>`                                                   |
| **Output Destination**    | Kernel ring buffer (view using `dmesg`)                              |
| **Purpose**               | Debugging and status messages                                        |
| **Safe in Kernel Space?** | ✅ Yes — `printk()` is designed for kernel context                    |
| **Not Real-time Safe**    | ⚠️ Should not be used in time-critical interrupt contexts repeatedly |

---

## 🧠 Log Levels in printk()

`printk()` supports **log levels** to classify messages based on importance.
Each level defines how urgent or informative the message is.

| Log Level | Macro          | Purpose                                        |
| --------- | -------------- | ---------------------------------------------- |
| **0**     | `KERN_EMERG`   | System is unusable (highest priority)          |
| **1**     | `KERN_ALERT`   | Immediate action needed                        |
| **2**     | `KERN_CRIT`    | Critical condition (hardware/software failure) |
| **3**     | `KERN_ERR`     | Error condition                                |
| **4**     | `KERN_WARNING` | Warning (non-critical issues)                  |
| **5**     | `KERN_NOTICE`  | Normal but significant condition               |
| **6**     | `KERN_INFO`    | Informational messages (general purpose)       |
| **7**     | `KERN_DEBUG`   | Debugging messages (lowest priority)           |

---

## ⚙️ Example: Using Different Log Levels

```c
#include <linux/module.h>
#include <linux/kernel.h>

static int __init printk_demo_init(void)
{
    printk(KERN_EMERG  "EMERG: System is going down!\n");
    printk(KERN_ALERT  "ALERT: Immediate action required!\n");
    printk(KERN_CRIT   "CRIT: Critical condition.\n");
    printk(KERN_ERR    "ERR: Something failed!\n");
    printk(KERN_WARNING"WARNING: Possible issue detected.\n");
    printk(KERN_NOTICE "NOTICE: Normal but significant info.\n");
    printk(KERN_INFO   "INFO: Hello Krish! Module initialized.\n");
    printk(KERN_DEBUG  "DEBUG: Detailed debug information.\n");
    return 0;
}

static void __exit printk_demo_exit(void)
{
    printk(KERN_INFO "INFO: Module exiting, goodbye!\n");
}

module_init(printk_demo_init);
module_exit(printk_demo_exit);

MODULE_LICENSE("GPL");
MODULE_AUTHOR("Krish");
MODULE_DESCRIPTION("Demo of printk() function and log levels");
```

---

## 🧾 Sample Output (in `dmesg`)

After loading the module:

```bash
sudo insmod printk_demo.ko
dmesg | tail -n 10
```

Output:

```
[1234.111111] EMERG: System is going down!
[1234.111112] ALERT: Immediate action required!
[1234.111113] CRIT: Critical condition.
[1234.111114] ERR: Something failed!
[1234.111115] WARNING: Possible issue detected.
[1234.111116] NOTICE: Normal but significant info.
[1234.111117] INFO: Hello Krish! Module initialized.
[1234.111118] DEBUG: Detailed debug information.
```

---

## 🧠 Using `pr_*()` Macros (Modern Method)

The Linux kernel also provides simplified macros that automatically set log levels:

| Macro         | Equivalent to              |
| ------------- | -------------------------- |
| `pr_emerg()`  | `printk(KERN_EMERG ...)`   |
| `pr_alert()`  | `printk(KERN_ALERT ...)`   |
| `pr_crit()`   | `printk(KERN_CRIT ...)`    |
| `pr_err()`    | `printk(KERN_ERR ...)`     |
| `pr_warn()`   | `printk(KERN_WARNING ...)` |
| `pr_notice()` | `printk(KERN_NOTICE ...)`  |
| `pr_info()`   | `printk(KERN_INFO ...)`    |
| `pr_debug()`  | `printk(KERN_DEBUG ...)`   |

### ✅ Example

```c
#include <linux/module.h>
#include <linux/kernel.h>

static int __init hello_init(void)
{
    pr_info("Hello Krish! Module loaded using pr_info().\n");
    pr_warn("Be careful with kernel debugging!\n");
    return 0;
}

static void __exit hello_exit(void)
{
    pr_info("Goodbye Krish! Module unloaded.\n");
}

module_init(hello_init);
module_exit(hello_exit);

MODULE_LICENSE("GPL");
MODULE_AUTHOR("Krish");
MODULE_DESCRIPTION("Example using pr_info() macros");
```

> These macros are **preferred** in new kernel code — cleaner and consistent.

---

## 🧩 Checking Kernel Logs

```bash
# Show the last few kernel messages
dmesg | tail

# Follow kernel logs live
sudo dmesg -w
```

> `dmesg` reads from the kernel’s **ring buffer**, where all printk outputs are stored.

---

## 🧰 Controlling Log Verbosity

You can adjust how much detail appears in `dmesg` using the **kernel log level**.

### 🔹 View Current Log Level

```bash
cat /proc/sys/kernel/printk
```

Output example:

```
4   4   1   7
```

* First number = current console log level
  (messages with lower or equal level are printed to console)

### 🔹 Change Log Level Temporarily

```bash
sudo sysctl -w kernel.printk=7
```

> Level `7` means all messages (including debug) are shown.

---

## 🧱 Kernel Log Buffer Files

| File                      | Description                        |
| ------------------------- | ---------------------------------- |
| `/var/log/kern.log`       | Kernel logs (if syslog is enabled) |
| `/proc/kmsg`              | Interface to kernel message buffer |
| `/proc/sys/kernel/printk` | Controls logging levels            |

---

## 🧠 Key Takeaways

| Concept                 | Explanation                                          |
| ----------------------- | ---------------------------------------------------- |
| `printk()`              | Kernel-space print function (like printf for kernel) |
| **Output Location**     | Kernel ring buffer → view with `dmesg`               |
| **Log Levels**          | Control message importance (0–7)                     |
| **`pr_info()` Macros**  | Modern, cleaner way to log                           |
| **Cannot use printf()** | No user-space I/O in kernel                          |
| **Debugging Tip**       | Use `sudo dmesg -w` to live-watch kernel logs        |

---

## 🧭 Summary

| Task                | Command                          | Description        |                 |
| ------------------- | -------------------------------- | ------------------ | --------------- |
| Print inside kernel | `printk()` / `pr_info()`         | Kernel log message |                 |
| View logs           | `dmesg`                          | Show ring buffer   |                 |
| Filter logs         | `dmesg                           | grep "Krish"`      | Search your tag |
| Set log level       | `sudo sysctl -w kernel.printk=7` | Enable all logs    |                 |

Would you like the **next section** to be  
👉 *“module_init() and module_exit() – how kernel calls them internally with examples”*  
in the same formatted Markdown style?
```

