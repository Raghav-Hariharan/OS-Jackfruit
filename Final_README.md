# Container Memory Monitor (OS Jackfruit)

---

## 1. Overview

This project implements a lightweight container runtime in C along with a Linux kernel module to monitor and control memory usage.

- User-space runtime (engine.c) launches and manages containers
- Kernel module (monitor.c) tracks memory usage
- Enforces soft and hard memory limits

---

## 2. Features

- Run containerized workloads
- Track memory usage (RSS)
- Soft limit warning (logged once)
- Hard limit enforcement (SIGKILL)
- Safe kernel linked-list management
- Periodic monitoring using kernel timer

---

## 3. Build and Run

### Build

cd boilerplate
make

### Load Kernel Module

sudo insmod monitor.ko
ls /dev | grep container_monitor

---

## 4. Running a Container

sudo ./engine run test rootfs-alpha ./memory_hog

---

## 5. Demo (Screenshots)

Build and Module Load → Screenshots/build.png  
Running Workload → Screenshots/run.png  
Soft Limit Trigger → Screenshots/soft_limit.png  
Hard Limit Enforcement → Screenshots/hard_limit.png  
Module Unload → Screenshots/unload.png  

---

## 6. How It Works

1. Engine launches process using fork + exec
2. Registers PID with kernel using ioctl
3. Kernel tracks memory using RSS
4. Timer checks periodically:
   - Soft limit → warning
   - Hard limit → kill

---

## 7. Design Decisions

- Mutex used for safe access
- Linked list for tracking containers
- Kernel enforcement for reliability

---

## 8. Files

engine.c → runtime  
monitor.c → kernel module  
monitor_ioctl.h → ioctl interface  
memory_hog.c → test workload  

---

## 9. Cleanup

sudo rmmod monitor
