
# 🧠 Linux Kernel Basics – Interview Answers

## 🔹 1. What is the Linux kernel?

The Linux kernel is the **core component of the operating system** that acts as a bridge between **hardware and user applications**.

👉 It manages system resources and provides services through **system calls**.

**Key point (interview line):**  
"Kernel is the heart of the OS, responsible for managing hardware and enabling user-space applications to interact with it."

## 🔹 2. What are the responsibilities of the kernel?

The kernel is responsible for:

- **Process Management** → scheduling, creation, termination  
- **Memory Management** → virtual memory, paging  
- **Device Management** → device drivers, hardware interaction  
- **File System Management** → file operations  
- **System Calls Handling** → interface between user and kernel  
- **Security & Access Control**

👉 Interview tip:
"Kernel manages CPU, memory, and devices efficiently while providing abstraction to user applications."

## 🔹 3. Difference between kernel space and user space

| Feature            | Kernel Space                          | User Space                          |
|------------------|--------------------------------------|------------------------------------|
| Access Level     | Full hardware access                 | Limited access                     |
| Execution Mode   | Privileged mode                      | Non-privileged mode                |
| Stability        | Crash affects entire system          | Crash affects only that process    |
| Examples         | Drivers, scheduler                   | Applications, libraries            |

👉 Key line:
"User space cannot directly access hardware; it must go through kernel via system calls."

## 🔹 4. What is a monolithic kernel?

A monolithic kernel is a kernel where **all core components run in kernel space**, including:

- Device drivers  
- File system  
- Memory management  
- Scheduler  

👉 Advantage:
- High performance (no context switching)

👉 Disadvantage:
- Less modular, harder to maintain

👉 Example:
Linux kernel (with modular support)

## 🔹 5. What is modular kernel architecture?

In modular kernel architecture, functionality is split into **loadable modules**.

👉 Features:
- Modules can be **loaded/unloaded dynamically**
- Improves flexibility and maintainability

👉 Example:
Device drivers loaded using `.ko` files

👉 Key line:
"Linux is monolithic but supports modularity through loadable kernel modules."

## 🔹 6. What are kernel modules?

Kernel modules are **dynamically loadable pieces of code** that extend kernel functionality.

👉 Used for:
- Device drivers  
- File systems  
- Network modules  

👉 Advantages:
- No need to rebuild kernel
- Load/unload at runtime

👉 Example:
`.ko` (kernel object) files

## 🔹 7. How to insert and remove kernel modules?

### Insert module:
```bash
insmod module.ko
````

### Remove module:

```bash
rmmod module
```

### Check loaded modules:

```bash
lsmod
```

👉 Key point:

* `insmod` → loads module into kernel
* `rmmod` → removes module safely

## 🔹 8. What is the difference between module_init and module_exit?

* **module_init()**

  * Called when module is loaded
  * Used for initialization

* **module_exit()**

  * Called when module is removed
  * Used for cleanup

### Example:

```c
module_init(my_init);
module_exit(my_exit);
```

# 🔧 Device Driver Fundamentals – Interview Answers

## 🔹 1. What is a device driver?

- A device driver is a **software component in the kernel** that enables communication between **hardware and user applications**
- Acts as an **interface between OS and hardware device**
- Translates **user requests → hardware-specific operations**
- Runs in **kernel space**

👉 Key line:
"Device driver allows the OS to interact with hardware without exposing hardware details to user applications."

## 🔹 2. Why do we need device drivers?

- Hardware devices have **different protocols and registers**
- Applications cannot directly access hardware (security reasons)
- Driver provides **abstraction layer**
- Ensures **standard interface (read/write/ioctl)**
- Handles **hardware-specific operations**

👉 Example:
- Same `read()` system call works for keyboard, file, or sensor because of drivers

## 🔹 3. Types of Device Drivers

### 🔸 Character Driver

- Handles data as **stream of bytes**
- No buffering or block structure
- Supports operations like:
  - read()
  - write()
- Data processed **sequentially**

👉 Examples:
- UART
- Keyboard
- Serial devices

### 🔸 Block Driver

- Handles data in **fixed-size blocks**
- Supports **random access**
- Uses **buffer/cache mechanism**

👉 Examples:
- Hard disk
- SSD
- eMMC

### 🔸 Network Driver

- Handles **network communication**
- Manages **packet transmission and reception**
- Works with networking stack (TCP/IP)

👉 Examples:
- Ethernet driver
- Wi-Fi driver

## 🔹 4. Difference between Character and Block Device

| Feature              | Character Device                | Block Device                  |
|---------------------|--------------------------------|-------------------------------|
| Data Handling       | Stream of bytes                | Fixed-size blocks             |
| Access Type         | Sequential                    | Random access                 |
| Buffering           | No                           | Yes                           |
| Example             | UART, Keyboard               | HDD, SSD                      |

👉 Key line:
"Character devices are stream-based, while block devices support random access with buffering."

## 🔹 5. What is a device file?

- A device file is a **special file in /dev directory**
- Used by user applications to **access device drivers**
- Provides **file-like interface** to hardware

👉 Examples:
- `/dev/ttyS0` → serial device  
- `/dev/sda` → disk  

👉 Key point:
"User interacts with device driver through device file using system calls like read/write."

## 🔹 6. What is major and minor number?

- Device file is identified by:
  - **Major number**
  - **Minor number**

### 🔸 Major Number
- Identifies **which driver** handles the device

### 🔸 Minor Number
- Identifies **specific device instance**

👉 Example:
- Same driver (major number)
- Multiple devices (minor numbers)

👉 Key line:
"Major number identifies driver, minor number identifies device under that driver."

---
