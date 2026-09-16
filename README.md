# dreaOS

> **A system built from its own code, architecture and execution model.**

**dreaOS** is an operating-system project built around **CNU (Code is Not Unix)** and the **ARD50 architecture**.

It is currently in an early development stage, focused on building the foundations of the system before the graphical environment is implemented.

## What is dreaOS?

dreaOS is not intended to be a Unix-like system with a different interface. Its core is being designed around its own language, instruction architecture, boot flow, filesystem model and system processes.

The project currently develops several layers together:

- **CNU** — the system programming language used by dreaOS.
- **ARD50** — the instruction architecture used by the system.
- **AMD65** — a CNU subcode used for specific system operations.
- **Kernel** — initialization, dependencies and the path toward process and system management.
- **Boot system** — device registration, memory/storage checks, thermal handling and system startup.
- **Filesystem** — archive identification, file recognition, data access and storage operations.
- **PLIST execution** — system startup information and execution entry points.

## CNU

### Code is Not Unix

The name **CNU** means **Code is Not Unix**.

CNU is its own language. It is not C with renamed keywords and is not designed as a Unix/POSIX dialect.

CNU provides the code layer used to describe dreaOS components and interact with ARD50 instructions and system processes.

For example, `GET` has a specific CNU meaning: **collect data**. Its behavior should therefore be understood according to CNU's own semantics rather than by mapping it automatically to a function from another language or operating system.

## ARD50

**ARD50** is dreaOS's instruction architecture.

Its instruction set contains operations for system and device capabilities such as:

- filesystem and archive operations
- data reading and writing
- graphics and display handling
- audio and video handling
- Wi-Fi and Bluetooth
- battery information
- screen interaction
- virtualization
- system startup and shutdown
- process management

The instruction identifiers are structured according to the ARD50 naming and numbering system. They are part of the architecture rather than arbitrary labels.

Some current system lifecycle instructions include:

| Instruction | Operation |
|---|---|
| `SIS` | SystemStart |
| `SES` | SystemShutDown |
| `SAS` | SystemRestart |
| `SOS` | SystemKillProcess |
| `SUS` | SystemProcess |

## Boot flow

The current startup architecture is being connected through the kernel and PLIST system.

A simplified view is:

```text
kernelinit.cnu
      │
      ▼
GET /pls/plist/plistmainstart.plist
      │
      ▼
mainStart.plist
      │
      ▼
ARD50 SIS
(SystemStart)
      │
      ▼
start.sh / boot
      │
      ▼
dreaOS startup
```

The purpose of this layer is to establish a defined execution path before the graphical environment exists.

## Kernel

The kernel is currently being built from the bottom up.

Current foundations include:

- kernel initialization
- ARD50 dependency loading
- startup entry handling
- boot integration
- system lifecycle operations
- preparation for process and scheduler management

The scheduler and additional kernel subsystems are part of the next stages of development.

## Filesystem

The filesystem layer is responsible for understanding the data dreaOS can encounter and providing operations around it.

Current components include:

- file recognition
- archive identification
- archive metadata/type handling
- data reading
- data writing
- archive editing
- image and other media handling
- filesystem structures

The project intentionally treats file types as system-level information rather than leaving everything to applications.

## AMD65

**AMD65** is a subcode of CNU.

It is currently used for system-level operations such as restart, shutdown and startup control through ARD50 instructions.

Conceptually:

```text
CNU
├── CNU code
├── AMD65
│   └── system subcode
└── ARD50 access
    └── architecture instructions
```

## Project structure

```text
ARD50/
├── ard.parser
├── ard50.exe
└── ard50.fad

boot/
├── boot.cnu
├── bootloader.cnu
├── bootregister.cnu
├── bootstrap.cnu
└── thermalcamps.cnu

camps/
├── campestructure.json.cnu
└── campregistry.cnu

filesystem/
├── filerecogniser.cnu
├── filespace.cnu
├── filesystem.ard50
└── fileunion.amd65

kernel/
├── ard50.dpd.cnu
└── kernelinit.cnu

pls/
└── plist/
    └── plistmainstart.plist
```

## Current development stage

**Beta 1 · Version 1.5 · 16 September 2026**

dreaOS is currently focused on its system foundations. There is **no complete graphical desktop yet**. The priority is to establish the architecture, language, kernel, boot sequence, filesystem and system interfaces first.

Planned development areas include:

1. Complete the kernel foundation.
2. Build scheduler and process-management layers.
3. Expand the boot system.
4. Establish the main execution/header system.
5. Add system APIs and device capabilities.
6. Build the navigation and graphical environment.
7. Develop an ARD50 virtual machine/runtime for multiple host platforms.

## Philosophy

dreaOS is being built around a simple idea:

> **Build the system underneath the interface, not the interface around an unfinished system.**

The graphical environment comes later. The architecture comes first.

## Status

🚧 **Early development**

The project is experimental and actively changing. File formats, language semantics, instruction definitions and internal architecture may evolve as dreaOS develops.

---

**dreaOS**  
**CNU — Code is Not Unix**  
**ARD50 — dreaOS instruction architecture**
