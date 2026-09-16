# dreaOS

> **A system built from its own code, architecture and execution model.**

**dreaOS** is an operating-system project built around **CNU (Code is Not Unix)** and the **ARD50 architecture**. It is being developed from the foundations upward: language, instruction architecture, kernel, boot, filesystem, process model, system interfaces and, later, the graphical environment.

## What is dreaOS?

dreaOS is not intended to be a Unix-like system with a different interface. Its execution model is being designed around its own language, instruction architecture, boot flow, filesystem model and system processes.

```text
                         dreaOS
                            │
              ┌─────────────┴─────────────┐
              │                           │
             CNU                        ARD50
       system code layer          instruction architecture
              │                           │
       ┌──────┴──────┐            ┌───────┴────────┐
       │             │            │                │
     AMD65       system code   devices/files    processes
       │             │            │                │
       └─────────────┴────────────┴────────────────┘
                            │
                          Kernel
                            │
                 ┌──────────┴──────────┐
                 │                     │
                Boot               Filesystem
                 │                     │
                 └──────────┬──────────┘
                            │
                         dreaOS
```

The graphical environment is intentionally later in the stack. The current work is the system underneath it.

## CNU: Code is Not Unix

**CNU** means **Code is Not Unix**.

CNU is its own programming language and execution model. It is **not C with renamed keywords**, and it is not intended as a Unix/POSIX dialect.

CNU is the code layer used to describe dreaOS components, system processes, dependencies, boot operations and interactions with ARD50.

For example, `GET` has a specific CNU meaning: **collect data**. Its semantics come from CNU itself rather than from an identically named operation in another language.

### CNU architecture

CNU is organised around several kinds of system code:

- **CNU code** — the main system language.
- **Headers** — structural entry and organisation information used throughout the codebase. The project is built around the Magic Up Header execution model.
- **Processes** — operations and subprocesses that form system behaviour.
- **Dependencies** — components that can be requested and loaded by the system.
- **ARD50 calls** — references from CNU into the instruction architecture.
- **AMD65** — a CNU subcode used for specialised system operations.
- **PLIST data** — structured startup/execution information used by the boot path.

The architectural distinction is simple: **CNU describes system code and behaviour, while ARD50 provides architecture-level instructions that CNU can invoke.**

## ARD50

**ARD50** is dreaOS's instruction architecture. It is not a package manager or conventional library. CNU can invoke ARD50 instructions using their architecture identifiers.

The numeric identifiers are deliberately structured according to the ARD50 numbering system. They are part of the architecture and are not arbitrary labels.

### Current ARD50 instruction set

The current `ARD50/ard50.exe` definition contains these instructions and operations:

| Instruction | Operation |
|---|---|
| `UN` | JIT |
| `DFR` | Drawer |
| `DFX` | Painter |
| `TRO` | Virtualitation |
| `TRP` | OpenApp |
| `TRE` | LoadArchive |
| `TRA` | UploadImage |
| `TRW` | CableDetection |
| `FGPD` | DataTransfear |
| `FPG` | DataParser |
| `FGP` | DataZIP |
| `FPSE` | DataUNZIP |
| `PSE` | DataReader |
| `FPS` | DataWriter |
| `DET` | Imput |
| `TED` | DisplayPortManager |
| `GET` | Get* / collect data |
| `JITT` | VMInteraction |
| `VOR` | Permissums |
| `DAT` | AudioPlayer |
| `WRT` | VideoPlayer |
| `WET` | ImageViewer |
| `NED` | TypeArchive |
| `NET` | ArchiveViewer |
| `NAD` | ArchiveIdentifier |
| `HTF` | ImageBitInfo |
| `NFFO` | ImageColors |
| `NID` | VideoFrame |
| `IDO` | FrameBuffer |
| `LAK` | FrameByFrame |
| `LRZ` | PiP / Picture-in-Picture |
| `LNK` | AudioManager |
| `SSO` | AudioBuffer |
| `DAOS` | AudioDriver |
| `DOS` | VMAction |
| `DOX` | DocEditor |
| `DOCX` | Graphics |
| `ARCX` | GraphicsV2 |
| `RDD` | TurnOff |
| `DRR` | TurnOn |
| `FDO` | ReadWi-FiSignal |
| `FDE` | Wi-FiDriver |
| `IDE` | Wi-FiConnect |
| `NEW` | ReadBluetoothDevices |
| `BOL` | BluetoothDriver |
| `BAL` | BluetoothConnect |
| `REST` | ReadBatteryCapacity |
| `API` | Read"%"Baterry |
| `IPA` | ReadChargeCiclesBattery |
| `INT` | ReadBatteryHealth |
| `UNT` | ScreenDriver |
| `UND` | ScreenViewer |
| `YED` | ScreenInteracts |
| `SIS` | SystemStart |
| `SES` | SystemShutDown |
| `SAS` | SystemRestart |
| `SOS` | SystemKillProcess |
| `SUS` | SystemProcess |

The spellings above intentionally follow the current ARD50 source, including names such as `Virtualitation`, `DataTransfear`, `Imput` and `Permissums`.

### ARD50 instruction families

**System and process control:** `SIS`, `SES`, `SAS`, `SOS`, `SUS`, `RDD`, `DRR`, `UN`, `JITT`, `DOS`.

**Filesystem, archives and data:** `TRE`, `FGPD`, `FPG`, `FPSE`, `PSE`, `FPS`, `NED`, `NET`, `NAD`, `DOX`.

**Graphics, display and media:** `DFR`, `DFX`, `TRA`, `TED`, `WET`, `DOCX`, `ARCX`, `HTF`, `NFFO`, `NID`, `IDO`, `LAK`, `LRZ`, `DET`.

**Audio:** `DAT`, `LNK`, `SSO`, `DAOS`.

**Video:** `WRT`, `NID`, `IDO`, `LAK`, `LRZ`.

**Connectivity and device access:** `FDO`, `FDE`, `IDE`, `NEW`, `BOL`, `BAL`, `REST`, `API`, `IPA`, `INT`.

**Virtualisation and applications:** `TRO`, `TRP`, `UN`, `JITT`, `DOS`.

**Screen interaction:** `UNT`, `UND`, `YED`.

These categories describe roles, not application-level substitutes. dreaOS deliberately exposes differentiated capabilities such as screen driving, screen viewing and screen interaction.

## Files dreaOS recognises, reads and processes

The filesystem recogniser defines a broad set of formats and semantic categories. **Recognition is not automatically execution**: knowing what a file is does not mean that dreaOS already has a complete native runtime for it.

### Recognised file formats

```text
.psd  .ipa  .apk  .aab  .ubn  .deb  .pdf  .png  .jpg  .pin
.mp4  .mp3  .hex  .css  .midi  .exe  .msi  .msix  .yml
.p12  .mobileprovision  .zip  .unzip  .dmg  .app
.mobileconfig  .winrar  .html  .txt  .js  .jsx  .tsx  .ts
.node  .cpp  .csharp  .py  .swift  .kt  .obj-c  .NET  .flutter
.tar.gz  .tar  .appar  .data  .sys  .syslog  .sysdiagnostic
.sys.tpd.td
```

The recogniser also assigns semantic categories including extensions, archive content, archives, archive metadata, archive information, archive interfaces, images, video, buffers, audio and code archives.

### Reading and filesystem operations

The filesystem architecture includes operations for:

- identifying an archive
- opening/viewing archive data
- identifying archive type/content
- reading data
- writing data
- registering/inputting data
- editing archive data
- displaying data on screen
- collecting data through the ARD50 `GET` operation
- handling image and media information

`filespace.cnu` connects these operations to ARD50 instructions for archive identification, archive viewing, archive editing, data reading, data writing, input/registration and screen output.

### Parsing and execution

`ARD50/ard.parser` defines the current ARD50 parsing entry as:

```text
PARSE*ARD50INSTRUCTIONS
GET*./ard50.exe
```

The current model therefore parses the ARD50 instruction definition from `ard50.exe`. CNU code can reference the architecture's instruction identifiers, while the future ARD50 VM/runtime is intended to provide portable execution across different host platforms.

The recognised application/code formats are therefore **known filesystem formats**, not a claim that every listed format already has a complete native runtime. Native execution support will expand as the kernel, process system and ARD50 VM/runtime are implemented.

## Kernel architecture

The kernel is being built from the bottom up rather than starting with a desktop shell.

```text
Magic Up Header / execution entry
                │
                ▼
          kernelinit.cnu
                │
        ┌───────┴────────┐
        │                │
   ARD50 dependency    PLIST
        loading          │
        │                ▼
        │        mainStart.plist
        │                │
        │             SIS 751
        │                │
        └────────┬───────┘
                 ▼
              Boot
                 │
       ┌─────────┴─────────┐
       │                   │
 device registration   memory/storage
       │                   │
       └─────────┬─────────┘
                 ▼
          system processes
                 │
             scheduler
                 │
          system interfaces
```

The scheduler and additional process-management layers are still under development. The graphical environment comes after these foundations.

## Boot system

The boot layer currently includes device registration, memory/storage checks and thermal handling.

The startup path is connected through the kernel and PLIST system:

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
SIS 751 — SystemStart
      │
      ▼
start.sh / boot
      │
      ▼
dreaOS startup
```

The boot subsystem also contains separate components for the bootloader, boot registration, bootstrap checks and thermal camps.

## AMD65

**AMD65** is a subcode of CNU, not a separate operating-system architecture.

Its current system-level use includes restart/shutdown/startup control through ARD50 instructions.

```text
CNU
├── main CNU code
├── AMD65
│   └── specialised CNU subcode
└── ARD50 access
    └── architecture instructions
```

## PLIST and system startup

`pls/plist/plistmainstart.plist` provides startup information consumed by `kernelinit.cnu`.

It invokes ARD50 instruction `751`, **SIS / SystemStart**, and connects that operation to the startup sequence. This bridges kernel initialisation, the PLIST execution layer and system boot.

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

dreaOS currently has **no complete graphical desktop**. The project is building the system underneath it first: language, instruction architecture, kernel, boot, filesystem, process model and system interfaces.

Planned development areas include:

1. Complete the kernel foundation.
2. Build scheduler and process-management layers.
3. Expand the boot system.
4. Establish the complete Magic Up Header execution system.
5. Add system APIs and device capabilities.
6. Build navigation and the graphical environment.
7. Develop the ARD50 virtual machine/runtime for multiple host platforms.

## Philosophy

> **Build the system underneath the interface, not the interface around an unfinished system.**

The graphical environment comes later. The architecture comes first.

## Status

🚧 **Early development**

The project is experimental and actively changing. File formats, language semantics, instruction definitions and internal architecture may evolve as dreaOS develops.

---

**dreaOS**  
**CNU — Code is Not Unix**  
**ARD50 — dreaOS instruction architecture**
