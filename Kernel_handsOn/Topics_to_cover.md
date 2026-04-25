
# 30-Day Kernel Programming Hands-On Plan

---

## 🗓️ Week 1: Foundations (Days 1–7)
- **Day 1:** Setup environment + Hello Kernel module  
- **Day 2:** Module parameters (`module_param`, `MODULE_PARM_DESC`)  
- **Day 3:** Logging & debugging → `dmesg`, `printk`, `trace_printk`  
- **Day 4:** Procfs (`/proc/hello`)  
- **Day 5:** Sysfs attributes (`/sys/kernel/hello/value`)  
- **Day 6:** Character driver basics (`/dev/hello`)  
- **Day 7:** File operations (`open`, `read`, `write`, `ioctl`)  

---

## 🗓️ Week 2: Memory & Concurrency (Days 8–14)
- **Day 8:** Kernel memory APIs → `kmalloc`, `kfree`, `vmalloc`, `kzalloc`  
- **Day 9:** Slab allocator exploration (`slabinfo`, `slub_debug`)  
- **Day 10:** Workqueues & tasklets (deferred execution)  
- **Day 11:** Kernel threads (`kthread_create`, `wake_up_process`)  
- **Day 12:** Synchronization → spinlocks, semaphores, mutexes  
- **Day 13:** Wait queues + blocking I/O in drivers  
- **Day 14:** Atomic operations, barriers, RCU basics  

---

## 🗓️ Week 3: Subsystems & Networking (Days 15–21)
- **Day 15:** Timer API (`hrtimer`, `timer_list`)  
- **Day 16:** Interrupt handling (`request_irq`, top/bottom halves)  
- **Day 17:** Input subsystem — build a fake keyboard driver  
- **Day 18:** Block device driver (create a RAM disk)  
- **Day 19:** Network stack — Netfilter hook (packet logger)  
- **Day 20:** Packet manipulation (drop/modify packets)  
- **Day 21:** Socket programming in kernel (`sock_create_kern`)  

---

## 🗓️ Week 4: Advanced Topics + Jetson Projects (Days 22–30)
- **Day 22:** Debugging with gdb, kgdb, ftrace, perf  
- **Day 23:** Explore kernel scheduling — write a toy scheduler  
- **Day 24:** Page cache + virtual memory mapping (`mmap`)  
- **Day 25:** Build a toy in-memory filesystem (RAMFS-like)  
- **Day 26:** GPIO driver on Jetson (blink LED, toggle pins)  
- **Day 27:** I2C/SPI mini-driver on Jetson (read sensor data)  
- **Day 28:** V4L2 camera stub driver (fake webcam device)  
- **Day 29:** `/proc/jetson_stats` → GPU/CPU load from kernel space  
- **Day 30:** **Capstone Project** → Custom Jetson driver combining:
  - GPIO  
  - Sysfs  
  - Netfilter hook  

---

