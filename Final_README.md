# OS Jackfruit – Container Memory Monitor

## Overview
This project implements a **Linux kernel module** and a **user-space runtime (engine)** to monitor and control memory usage of container-like processes.

The system tracks processes, enforces memory limits, and takes action when limits are exceeded.

---

## Architecture

### User Space (engine)
- Handles CLI commands (`run`, `start`, `ps`, etc.)
- Spawns container processes using `fork + exec`
- Communicates with kernel module using `ioctl`

### Kernel Space (monitor module)
- Tracks registered processes
- Periodically checks memory usage (RSS)
- Enforces limits

---

## Features

- Register/unregister containers
- Track memory usage using RSS
- Soft limit warning (logged once)
- Hard limit enforcement (process killed)
- Safe concurrent access using mutex
- Periodic monitoring using kernel timer

---

## Data Structures

```c
struct container_entry {
    pid_t pid;
    char container_id[MONITOR_NAME_LEN];
    unsigned long soft_limit;
    unsigned long hard_limit;
    struct list_head list;
};
