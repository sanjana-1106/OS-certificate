# OS Fundamentals — Design Note

**Project type:** Conceptual design note  
**Topic:** Core responsibilities and high-level design of an operating system  
**Prepared for:** Operating Systems Fundamentals capstone

## 1. Abstract
An operating system (OS) is system software that acts as an interface between applications and computer hardware. It manages shared resources such as CPU time, main memory, storage, and input/output devices. This design note presents a simple, modular view of an operating system and explains how its main components cooperate to run programs safely and efficiently.

## 2. Objectives
- Explain the role of an operating system.
- Identify the main OS components and their responsibilities.
- Describe basic process scheduling and memory management.
- Explain file, device, and security management at a high level.
- Present a simple request flow from an application to hardware.

## 3. Scope and assumptions
This is an educational, conceptual design. It does not implement a bootable operating system or a kernel. The design uses common concepts found in modern general-purpose operating systems. Specific implementation details vary across Linux, Windows, macOS, and other systems.

## 4. High-level architecture
The system is organized into layers:

1. **Users and applications** — people interact with applications such as editors and browsers.
2. **System-call/API interface** — applications request operating-system services through defined interfaces.
3. **Kernel services** — process management, memory management, file systems, device drivers, and protection.
4. **Hardware** — CPU, RAM, disks/SSDs, display, keyboard, network interface, and other devices.

A typical request flows from an application through an OS interface to the relevant kernel service and, when needed, to a device driver and hardware. The result is returned to the application.

## 5. Main components

| Component | Responsibility |
|---|---|
| Process management | Creates, schedules, pauses, resumes, and terminates processes and threads. |
| CPU scheduler | Selects a ready process/thread to run on a CPU. |
| Memory management | Tracks memory use, allocates and releases memory, and supports virtual memory. |
| File-system management | Organizes files/directories and controls file access. |
| I/O and device management | Coordinates devices through drivers, interrupts, and I/O requests. |
| Security and protection | Enforces permissions and separates user activities from privileged kernel operations. |

## 6. Process management and scheduling
A **process** is a program in execution. A process typically moves through states such as **New**, **Ready**, **Running**, **Waiting/Blocked**, and **Terminated**. The OS maintains process information (often represented by a process control block) so execution can be paused and resumed.

A scheduler chooses which ready process receives CPU time. Common algorithms include:
- **FCFS (First-Come, First-Served):** processes run in arrival order.
- **SJF (Shortest Job First):** the process with the shortest estimated CPU burst is selected.
- **Round Robin:** each ready process receives a fixed time quantum in turn.

**Illustrative Round Robin example:** With a 2 ms time quantum and three CPU-bound jobs A=4 ms, B=3 ms, and C=2 ms, the CPU order is A (0–2), B (2–4), C (4–6), A (6–8), B (8–9). This gives each job a fair turn; actual performance depends on context-switch overhead and workload.

## 7. Memory management
The OS gives each process a controlled address space and manages the use of physical RAM. It allocates memory when a process needs it and reclaims memory when it is released or the process exits.

**Virtual memory** provides processes with virtual addresses that the OS and hardware translate to physical memory. Paging divides virtual memory into fixed-size pages and physical memory into frames. If a needed page is not currently in RAM, a page fault may cause the OS to load it from secondary storage. Excessive paging can reduce performance.

## 8. File and device management
The file system provides named files and directories, supports operations such as create/read/write/delete, and applies access permissions. Device management provides a consistent way for applications to use hardware. Device drivers translate OS requests into device-specific operations; interrupts notify the CPU when devices need attention or an operation has completed.

## 9. Protection and reliability
The OS separates ordinary application execution from privileged operations. Access checks help prevent one user or process from modifying resources it is not allowed to access. Process isolation, permissions, input validation, and careful error handling improve system reliability. Backups and recovery mechanisms help protect stored information.

## 10. Example: opening and saving a document
1. The user opens a document editor.
2. The OS creates or schedules the editor process.
3. The memory manager provides memory for the application.
4. The editor requests a file through the OS file-system interface.
5. The file system checks permissions and asks the storage driver to read data.
6. The OS returns the data to the application.
7. When the user saves, the file system coordinates writing the updated data to storage.

## 11. Basic validation checklist
A conceptual OS design can be reviewed using these questions:
- Can multiple processes be tracked and scheduled?
- Are memory allocations associated with the requesting process?
- Are file operations checked against permissions?
- Are device operations mediated by drivers or OS interfaces?
- Are privileged operations restricted?
- Are errors and resource-release paths considered?

## 12. Conclusion
The operating system coordinates hardware and software by managing processes, memory, files, devices, and protection. These components work together to provide applications with convenient abstractions while sharing hardware resources safely. A modular design makes the responsibilities easier to understand, test, and maintain.

## 13. References
1. OpenStax, *Introduction to Computer Science*, “Fundamental OS Concepts”: https://openstax.org/books/introduction-computer-science/pages/6-2-fundamental-os-concepts
2. GeeksforGeeks, “Operating System Tutorial” (overview and topic guide): https://www.geeksforgeeks.org/operating-systems/operating-systems/

---
**Submission note:** This is a conceptual design note, not a working OS implementation. Add your name/course details if your instructor requires them. Upload this README or the PDF to a public GitHub repository, then submit that repository URL in the course form.
