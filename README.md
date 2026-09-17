# dreaOS

> **A system built from its own code, architecture and execution model.**

**dreaOS** is an operating-system project built around **CNU (Code is Not Unix)** and the **ARD50 architecture**. It is being developed from the foundations upward: language, instruction architecture, kernel, boot, filesystem, process model, system interfaces and, later, the graphical environment.

## What is dreaOS?

dreaOS is not intended to be a Unix-like system with a different interface. Its execution model is being designed around its own language, instruction architecture, boot flow, filesystem model, system processes and system codes.

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

The numeric identifiers are part of the architecture and are defined in `ARD50/ard50.exe`.

### Current ARD50 instruction set

The following table reflects the current instruction definitions in `ARD50/ard50.exe`.

| ID | Instruction | Operation |
|---:|---|---|
| `407` | `UN` | `JIT` |
| `708` | `DFR` | `Drawer` |
| `709` | `DFX` | `Painter` |
| `710` | `TRO` | `Virtualitation` |
| `711` | `TRP` | `OpenApp` |
| `712` | `TRE` | `LoadArchive` |
| `713` | `TRA` | `UploadImage` |
| `714` | `TRW` | `CableDetection` |
| `715` | `FPG` | `DataParser` |
| `716` | `FGP` | `DataZIP` |
| `717` | `PSE` | `DataReader` |
| `718` | `FPS` | `DataWriter` |
| `719` | `DET` | `Imput` |
| `720` | `TED` | `DisplayPortManager` |
| `721` | `GET` | `Get* / collect data` |
| `722` | `VOR` | `Permissums` |
| `723` | `DAT` | `AudioPlayer` |
| `724` | `WRT` | `VideoPlayer` |
| `725` | `WET` | `ImageViewer` |
| `726` | `NED` | `TypeArchive` |
| `727` | `NET` | `ArchiveViewer` |
| `728` | `NAD` | `ArchiveIdentifier` |
| `729` | `HTF` | `ImageBitInfo` |
| `730` | `NID` | `VideoFrame` |
| `731` | `IDO` | `FrameBuffer` |
| `732` | `LAK` | `FrameByFrame` |
| `732` | `LRZ` | `PiP/PictureinPicture` |
| `733` | `LNK` | `AudioManager` |
| `734` | `SSO` | `AudioBuffer` |
| `735` | `DOS` | `VMAction` |
| `736` | `DOX` | `DocEditor` |
| `737` | `EDD` | `TurnOff` |
| `738` | `DRR` | `TurnOn` |
| `739` | `FDO` | `ReadWi-FiSignal` |
| `740` | `FDE` | `Wi-FiDriver` |
| `741` | `IDE` | `Wi-FiConnect` |
| `742` | `NEW` | `ReadBluetoothDevices` |
| `743` | `BOL` | `BluetoothDriver` |
| `744` | `BAL` | `BluetoothConnect` |
| `745` | `API` | `Read"%"Baterry` |
| `746` | `IPA` | `ReadChargeCiclesBattery` |
| `747` | `INT` | `ReadBatteryHealth` |
| `748` | `UNT` | `ScreenDriver` |
| `749` | `UND` | `ScreenViewer` |
| `750` | `YED` | `ScreenInteracts` |
| `751` | `SIS` | `SystemStart` |
| `752` | `SES` | `SystemShutDown` |
| `753` | `SAS` | `SystemRestart` |
| `754` | `SOS` | `SystemKillProcess` |
| `755` | `SUS` | `SystemProcess` |
| `756` | `MEM` | `MemoryReader` |
| `757` | `MAM` | `MemoryCapacityReader` |
| `758` | `MIM` | `MemoryAlibableReader` |
| `759` | `MOM` | `MemoryDataReader` |
| `760` | `MUM` | `MemoryWriter` |
| `761` | `MEN` | `MemoryAllWriter` |
| `762` | `MAN` | `MemoryVerification` |
| `763` | `MIN` | `MemorySecurity` |
| `764` | `MON` | `MemoryEncode` |
| `765` | `MUN` | `MemoryDataEncode256UNF` |
| `766` | `RAM` | `RAMReader` |
| `767` | `REM` | `RAMAllReader` |
| `768` | `RIM` | `RAMWriter` |
| `769` | `ROM` | `RAMProtect` |
| `770` | `RUM` | `RAMRestart` |
| `771` | `RAN` | `RAMReadCapacity` |
| `772` | `REN` | `RAMReadType` |
| `773` | `RIN` | `RAMReset` |
| `774` | `RON` | `RAMOfused` |
| `775` | `RUN` | `RUNProcess` |
| `776` | `MER` | `MemoryStart` |
| `777` | `MOR` | `MemoryShutDown` |
| `778` | `MIR` | `MemoryCrash` |
| `779` | `RAR` | `RAMStart` |
| `780` | `RER` | `RAMShutSown` |
| `781` | `RIR` | `RAMCrash` |
| `782` | `ARU` | `UploadArchiveReader` |
| `783` | `AED` | `UploadArchiveWriter` |
| `784` | `AID` | `UploadArchive` |
| `785` | `VGP` | `VirtualGPU` |
| `786` | `VCP` | `VirtualCPU` |
| `787` | `ISK` | `UseDisk` |
| `788` | `GSI` | `GPUStateInspect` |
| `789` | `CSI` | `CPUStateInspect` |
| `790` | `GSD` | `GPUStateDetachState` |
| `791` | `CSD` | `CPUStateDetachState` |
| `792` | `GCR` | `GPUCorruptionRecovery` |
| `793` | `CCR` | `CPUCorruptionRecovery` |
| `794` | `GRS` | `GPUResetState` |
| `795` | `CRS` | `CPUResetState` |
| `796` | `GCK` | `GPUStateCheck` |
| `797` | `CCK` | `CPUStateCheck` |
| `798` | `WIF` | `Wi-FiGetPassword` |
| `799` | `WAF` | `Wi-FiGetIP` |
| `800` | `WEF` | `Wi-FiComprober` |
| `915` | `FGPD` | `DataTransfear` |
| `916` | `FPSE` | `DataUNZIP` |
| `917` | `JITT` | `VMInteraction` |
| `918` | `NFFO` | `ImageColors` |
| `919` | `DAOS` | `AudioDriver` |
| `920` | `DOCX` | `Graphics` |
| `921` | `ARCX` | `GraphicsV2` |
| `922` | `SEST` | `ReadBatteryCapacity` |

The spellings above intentionally follow the current ARD50 source, including names such as `Virtualitation`, `DataTransfear`, `Imput`, `Permissums`, `RAMShutSown` and `ReadChargeCiclesBattery`.

### ARD50 instruction families

**System and process control:** `SIS`, `SES`, `SAS`, `SOS`, `SUS`, `EDD`, `DRR`, `RUN`, `UN`, `JITT`, `DOS`.

**Memory and RAM:** `MEM`, `MAM`, `MIM`, `MOM`, `MUM`, `MEN`, `MAN`, `MIN`, `MON`, `MUN`, `RAM`, `REM`, `RIM`, `ROM`, `RUM`, `RAN`, `REN`, `RIN`, `RON`, `MER`, `MOR`, `MIR`, `RAR`, `RER`, `RIR`.

**Filesystem, archives and data:** `TRE`, `FGPD`, `FPG`, `FGP`, `FPSE`, `PSE`, `FPS`, `NED`, `NET`, `NAD`, `DOX`, `ARU`, `AED`, `AID`, `GET`.

**Graphics, display and media:** `DFR`, `DFX`, `TRA`, `TED`, `WET`, `DOCX`, `ARCX`, `HTF`, `NFFO`, `NID`, `IDO`, `LAK`, `LRZ`, `DET`.

**Audio:** `DAT`, `LNK`, `SSO`, `DAOS`.

**Video:** `WRT`, `NID`, `IDO`, `LAK`, `LRZ`.

**Connectivity and device access:** `TRW`, `FDO`, `FDE`, `IDE`, `NEW`, `BOL`, `BAL`, `WIF`, `WAF`, `WEF`, `SEST`, `API`, `IPA`, `INT`.

**Virtualisation and applications:** `TRO`, `TRP`, `VOR`, `UN`, `JITT`, `DOS`, `VGP`, `VCP`, `ISK`.

**CPU/GPU state and recovery:** `GSI`, `CSI`, `GSD`, `CSD`, `GCR`, `CCR`, `GRS`, `CRS`, `GCK`, `CCK`.

**Screen interaction:** `UNT`, `UND`, `YED`.

These categories describe roles, not application-level substitutes. dreaOS deliberately exposes differentiated capabilities such as screen driving, screen viewing and screen interaction.

## JUT system codes

JUT has its own internal system-code vocabulary. These values are **not HTTP status codes**. They are semantic codes defined by dreaOS in `kernel/codes.cnu`.

| Code | Meaning |
|---:|---|
| `140` | `UndeterminatedObject` |
| `200` | `OKOperationSuccess` |
| `350` | `ComprobeThing` |
| `400` | `ERROROperationError` |
| `405` | `Invalid` |
| `500` | `UnalivableServer` |
| `503` | `UnexistantMethod` |
| `600` | `FAIL` |
| `769` | `UndeterminatedInterrupt` |
| `800` | `InterruptedOperation` |
| `806` | `LostenObject` |
| `850` | `LostenData` |
| `900` | `LostenArchive` |
| `905` | `UnactiveProcess` |
| `920` | `ComprobeProcess` |
| `940` | `ComprobeProcessStatus` |
| `1000` | `Unsuccess` |
| `1109` | `InvalidCommand` |

The JUT codes are used as semantic results/states inside the dreaOS execution model. They should not be interpreted as HTTP codes.

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
├── codes.cnu
└── kernelinit.cnu

memory/
├── memoryalivable.cnu
├── memorycapacity.cnu
├── memorydatareader.cnu
├── memorydatawriter.cnu
├── memorysecurity.cnu
├── memoryslot.cnu
└── memoryverification.cnu

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
