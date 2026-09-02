# OSFS — Open Source Forensic Scanner

<p align="center">
  <strong>OSFS</strong> is a modular, open-source digital forensic scanner built for detecting unauthorized software, anti-forensic techniques, cheating tools, macros, and suspicious system modifications. Designed for system administrators, game server staff, security researchers, and digital forensic investigators.
</p>

---

## Table of Contents

- [About](#about)
- [Vision](#vision)
- [Architecture](#architecture)
- [Scanner Modules](#scanner-modules)
- [Roadmap](#roadmap)
- [Coding Standards](#coding-standards)
- [Build System](#build-system)
- [Project Structure](#project-structure)
- [Contributing](#contributing)
- [TODO — Help Wanted](#todo--help-wanted)
- [Community](#community)

---

## About

OSFS is a next-generation forensic scanning framework. It scans a system for evidence of:

- **Cheats and injectors** — DLL injection, process hollowing, memory manipulation, fileless malware
- **Macros** — software macros, onboard hardware memory macros, autoclickers
- **Anti-forensic bypasses** — event log tampering, prefetch deletion, USN journal clearing, registry modification, disk replacement, time manipulation
- **File execution artifacts** — prefetch, BAM registry, jump lists, WER crash dumps, recent file shortcuts, task scheduler logs
- **Environment anomalies** — virtual machines, unplugged devices, multiple mice, network shared drives, unsigned processes, injected DLLs

OSFS is **not** a cheat detection tool for a single game. It is a **general-purpose forensic scanner** that can be extended with modules for any platform, any artifact type, and any detection technique.

---

## Vision

OSFS aims to become the industry-standard open-source forensic scanner. Here is the long-term vision:

### Web GUI (Planned)

A browser-based dashboard for running scans, viewing results, and generating reports.

- **Frontend:** React or Svelte with a component library (shadcn/ui or similar)
- **Backend:** Python (FastAPI) or Rust (Actix/Axum) REST API
- **Real-time updates:** WebSocket connections for live scan progress
- **Report generation:** PDF, JSON, and HTML export formats
- **Multi-scan management:** Queue scans, compare results across time, track system changes
- **Authentication:** Role-based access for teams (admin, scanner, viewer)
- **Deployment:** Docker container for self-hosting, or single-binary desktop app via Tauri

### Cross-Platform Support

- **Phase 1:** Windows (current foundation)
- **Phase 2:** Linux (artifact paths, systemd journals, /proc analysis)
- **Phase 3:** macOS (Unified Logs, LaunchAgents, Spotlight metadata)

### Plugin System

- Dynamic module loading at runtime
- Community-contributed scanner plugins
- Plugin manifest format (YAML/JSON) for metadata, dependencies, and supported OS
- Sandboxed plugin execution for third-party modules

### Machine Learning Integration

- Behavioral analysis of process execution patterns
- Anomaly detection across historical scan data
- Trained models for identifying suspicious file modifications
- Local inference (no cloud dependency) for privacy-sensitive environments

### API & Integration

- RESTful API for programmatic scan execution
- Webhook support for real-time alerting (Discord, Slack, email)
- SIEM integration (Splunk, Elastic, Graylog) via structured log output
- CI/CD pipeline integration for automated compliance scanning

---

## Architecture

OSFS is designed around a **modular scanner architecture**:

```
┌─────────────────────────────────────────────────┐
│                   OSFS Core                      │
│  ┌───────────┐  ┌──────────┐  ┌──────────────┐  │
│  │  Scanner   │  │  Result  │  │   Plugin     │  │
│  │  Engine    │  │  Engine  │  │   Loader     │  │
│  └─────┬─────┘  └────┬─────┘  └──────┬───────┘  │
│        │              │               │          │
│  ┌─────▼──────────────▼───────────────▼───────┐  │
│  │            Module Registry                 │  │
│  └─────┬──────────────┬───────────────┬───────┘  │
│        │              │               │          │
│  ┌─────▼─────┐  ┌─────▼─────┐  ┌─────▼─────┐   │
│  │ Artifact  │  │ Process   │  │ Network   │   │
│  │ Scanners  │  │ Scanners  │  │ Scanners  │   │
│  └───────────┘  └───────────┘  └───────────┘   │
└─────────────────────────────────────────────────┘
```

### Core Principles

1. **Modularity** — Every scanner is a self-contained module with a defined interface
2. **No external dependencies** — Core scanner compiles with zero third-party packages
3. **Deterministic results** — Same input always produces the same output
4. **Minimal footprint** — Scanner should not modify the system being scanned
5. **Offline-first** — No internet connection required for any scan
6. **Extensibility** — Adding a new scanner should require only implementing one interface

---

## Scanner Modules

### Artifact Scanners

| Module | Description | Status |
|--------|-------------|--------|
| Prefetch Analysis | Parse and analyze Windows prefetch files | TODO |
| BAM Registry | Browser Activity Monitor registry analysis | TODO |
| Jump Lists | Automatic and Custom Destination parsing | TODO |
| USN Journal | NTFS journal analysis for file modifications | TODO |
| Event Log | Windows event log integrity checks | TODO |
| WER Crash Dumps | Windows Error Reporting analysis | TODO |
| Recent Files | Recently accessed file shortcuts | TODO |
| Registry Artifacts | Suspicious registry key analysis | TODO |
| Task Scheduler | Scheduled task execution analysis | TODO |
| LNK Files | Shortcut file forensics | TODO |
| Shellbags | Explorer folder history analysis | TODO |
| Amcache | Application compatibility cache | TODO |
| SRUM | System Resource Usage Monitor data | TODO |
| Windows Timeline | Activity history analysis | TODO |
| MFT Analysis | Master File Table artifact extraction | TODO |

### Process Scanners

| Module | Description | Status |
|--------|-------------|--------|
| Unsigned DLLs | Detect unsigned injected DLLs | TODO |
| Unsigned Executables | Detect unsigned executed files | TODO |
| Process Hollowing | Detect hollowed processes | TODO |
| Memory Scanning | Scan process memory for suspicious strings | TODO |
| Injected Code | Detect injected code in running processes | TODO |
| Restarted Processes | Detect suspiciously restarted processes | TODO |
| Thread Manipulation | Detect modified thread behavior | TODO |
| Token Tampering | Detect process token modifications | TODO |

### Macro Scanners

| Module | Description | Status |
|--------|-------------|--------|
| Macro File Analysis | Detect 30+ macro file modifications | TODO |
| Deleted Macros | Detect deleted/renamed macro files | TODO |
| Onboard Memory Macros | Hardware mouse/keyboard macro detection | TODO |
| Macro Process Memory | Regex filtering of macro software memory | TODO |
| MouseKeys Detection | Detect MouseKeys autoclicker usage | TODO |

### Anti-Forensic Bypass Detection

| Module | Description | Status |
|--------|-------------|--------|
| Event Log Bypass | Detect .evtx log tampering | TODO |
| Time Change Bypass | Detect system time manipulation | TODO |
| Prefetch Bypass | Detect prefetch folder/file tampering | TODO |
| USN Journal Clearing | Detect $J datastream clearing | TODO |
| Disk Replacement | Detect formatted/replaced drives | TODO |
| Unplugged Devices | Detect recently unplugged devices | TODO |
| String Cleaners | Detect Process Hacker/System Informer | TODO |
| Code Import Detection | Detect base64/base32 anti-forensic code | TODO |
| Shadow Copy Deletion | Detect volume shadow copy removal | TODO |
| AutoRun Bypass | Detect AutoRun-based persistence | TODO |

### Environment Checks

| Module | Description | Status |
|--------|-------------|--------|
| Virtual Machine Detection | Detect VM environments | TODO |
| Multiple Mice | Detect multiple mouse devices (VID/PID) | TODO |
| Network Shares | Detect network shared drive execution | TODO |
| Digital Signature Verification | Verify file signatures | TODO |
| XRay Texture Packs | Detect XRay texture packs | TODO |

---

## Roadmap

### Phase 1 — Core Framework (Current)
- [ ] Define scanner module interface (trait/class)
- [ ] Implement plugin loader and module registry
- [ ] Build result engine with structured output (JSON)
- [ ] Create CLI entry point with argument parsing
- [ ] Implement logging infrastructure
- [ ] Set up CI/CD pipeline (GitHub Actions)
- [ ] Write unit test framework
- [ ] Document coding standards

### Phase 2 — Windows Artifact Scanners
- [ ] Prefetch parser
- [ ] BAM registry analyzer
- [ ] USN journal scanner
- [ ] Event log integrity checker
- [ ] Task scheduler analyzer
- [ ] Registry artifact scanner
- [ ] WER crash dump parser
- [ ] Jump list parser
- [ ] Recent files analyzer
- [ ] Amcache parser

### Phase 3 — Process & Memory Scanners
- [ ] Unsigned process/DLL detection
- [ ] Process hollowing detection
- [ ] Memory string scanning
- [ ] Thread manipulation detection
- [ ] Token tampering detection

### Phase 4 — Anti-Forensic Detection
- [ ] Event log bypass detection
- [ ] Time manipulation detection
- [ ] Disk replacement detection
- [ ] Shadow copy deletion detection
- [ ] Code import detection

### Phase 5 — Web GUI
- [ ] Backend API server
- [ ] Frontend dashboard
- [ ] Real-time scan monitoring via WebSocket
- [ ] Report generation (PDF/JSON/HTML)
- [ ] User authentication & role management
- [ ] Docker deployment

### Phase 6 — Cross-Platform
- [ ] Linux artifact scanners
- [ ] macOS artifact scanners
- [ ] Platform abstraction layer

### Phase 7 — Advanced Features
- [ ] Plugin marketplace
- [ ] ML-based anomaly detection
- [ ] SIEM integration
- [ ] API for third-party tools
- [ ] Automated compliance scanning

---

## Coding Standards

### Language

- **Primary:** C++20 (core scanner, modules, CLI)
- **Secondary:** C17 (low-level system interfaces where needed)
- **Web GUI Backend:** Python 3.12+ (FastAPI) or Rust
- **Web GUI Frontend:** TypeScript (React or Svelte)

### C++ Style Guide

```cpp
// File naming: lowercase with underscores
// Example: prefetch_parser.cpp, usn_journal_scanner.hpp

// Class naming: PascalCase
class PrefetchScanner { };

// Function naming: camelCase
void scanPrefetchFiles();

// Variable naming: snake_case
int artifact_count;

// Constants: UPPER_SNAKE_CASE
const int MAX_SCAN_DEPTH = 10;

// Namespaces: lowercase
namespace osfs::scanners::artifact { }

// Header guards: #pragma once
// Prefer std::optional over null pointers
// Prefer std::string_view over const std::string& for read-only params
// Use RAII for all resource management
// Use std::format (C++20) over printf/snprintf
```

### File Organization

- One class per file (with rare exceptions for tightly coupled classes)
- Headers use `.hpp`, sources use `.cpp`
- C files use `.c` / `.h`
- Each scanner module lives in its own subdirectory under `scanners/`
- Shared utilities live in `core/` or `platform/`

### Error Handling

- Use `std::expected` (C++23) or `std::optional` for fallible operations
- Never use exceptions for control flow
- Log errors with severity levels (TRACE, DEBUG, INFO, WARN, ERROR, FATAL)
- Scanner modules must never crash the application — catch and report

### Testing

- Unit tests for every scanner module
- Integration tests with known-good artifact samples
- Fuzz testing for parsers (USN journal, prefetch, registry)
- Test framework: Google Test or Catch2
- Minimum 80% code coverage for scanner modules

### Documentation

- Every public function has a doc comment (`///` style)
- Every scanner module has a README explaining what it detects and how
- Architecture Decision Records (ADRs) for significant design choices
- CHANGELOG.md updated with every merge

---

## Build System

### Planned: CMake + vcpkg

OSFS will use CMake as its build system with vcpkg for dependency management.

```bash
# Configure
cmake -B build -DCMAKE_TOOLCHAIN_FILE=[vcpkg-root]/scripts/buildsystems/vcpkg.cmake

# Build
cmake --build build --config Release

# Test
ctest --test-dir build
```

### CI/CD

- **GitHub Actions** for automated builds and tests
- **Matrix builds:** Windows (MSVC), Linux (GCC/Clang), macOS (Clang)
- **Release automation:** Tagged releases produce signed binaries
- **Code quality:** clang-format, clang-tidy, cppcheck in CI

---

## Project Structure

```
osfs/
├── CMakeLists.txt
├── README.md
├── CHANGELOG.md
├── CONTRIBUTING.md
├── CODE_OF_CONDUCT.md
├── .github/
│   ├── workflows/
│   │   ├── ci.yml
│   │   └── release.yml
│   └── ISSUE_TEMPLATE/
│       ├── bug_report.md
│       └── feature_request.md
├── core/
│   ├── scanner_engine.hpp/cpp      # Main scan orchestrator
│   ├── result_engine.hpp/cpp       # Result collection & output
│   ├── plugin_loader.hpp/cpp       # Dynamic module loading
│   ├── logger.hpp/cpp              # Structured logging
│   └── config.hpp/cpp              # Configuration management
├── platform/
│   ├── windows/
│   │   ├── registry.hpp/cpp        # Windows registry access
│   │   ├── wmi.hpp/cpp             # WMI query interface
│   │   ├── usn_journal.hpp/cpp     # USN journal access
│   │   └── process.hpp/cpp         # Process enumeration
│   ├── linux/
│   │   └── ...
│   └── macos/
│       └── ...
├── scanners/
│   ├── artifact/
│   │   ├── prefetch/
│   │   │   ├── prefetch_scanner.hpp
│   │   │   ├── prefetch_scanner.cpp
│   │   │   └── README.md
│   │   ├── bam/
│   │   ├── usn_journal/
│   │   ├── event_log/
│   │   ├── jump_list/
│   │   ├── registry/
│   │   ├── task_scheduler/
│   │   ├── recent_files/
│   │   ├── amcache/
│   │   └── srum/
│   ├── process/
│   │   ├── unsigned_dll/
│   │   ├── unsigned_executable/
│   │   ├── memory_scanner/
│   │   └── process_hollowing/
│   ├── macro/
│   │   ├── macro_file/
│   │   ├── onboard_memory/
│   │   └── mousekeys/
│   └── antiforensic/
│       ├── event_log_tamper/
│       ├── time_manipulation/
│       ├── disk_replacement/
│       └── shadow_copy/
├── utils/
│   ├── string_utils.hpp/cpp
│   ├── file_utils.hpp/cpp
│   ├── crypto_utils.hpp/cpp        # Signature verification
│   └── json.hpp                    # JSON library (single header)
├── tests/
│   ├── unit/
│   ├── integration/
│   └── fixtures/                   # Sample artifacts for testing
├── docs/
│   ├── architecture/
│   ├── adr/
│   └── scanners/                   # Per-scanner documentation
└── gui/                            # Web GUI (Phase 5)
    ├── backend/
    └── frontend/
```

---

## Contributing

OSFS is an open-source project and welcomes contributions from everyone. Whether you are fixing a bug, adding a scanner module, improving documentation, or building the web GUI — your help is appreciated.

### How to Contribute

1. **Fork** the repository
2. **Create** a feature branch (`git checkout -b feature/my-scanner`)
3. **Commit** your changes with clear messages
4. **Push** to your fork
5. **Open** a Pull Request

### Good First Issues

Look for issues tagged `good-first-issue` or `help-wanted` in the issue tracker.

### Code Review

All pull requests require at least one review before merging. Be open to feedback and iterate quickly.

---

## TODO — Help Wanted

These are areas where OSFS needs community contributions. Pick something, claim it in an issue, and start building.

### High Priority

- [ ] **Scanner module interface** — Design and implement the base `Scanner` trait/class that all modules implement. Define result types, severity levels, and metadata.
- [ ] **CLI argument parser** — Build the command-line interface with subcommands (scan, list-modules, export, config).
- [ ] **Result engine** — Implement structured result output (JSON, colored terminal, plain text).
- [ ] **Logging infrastructure** — Set up spdlog or similar with file + console sinks, log levels, and rotation.
- [ ] **Unit test framework** — Set up Google Test or Catch2, write first tests for core modules.
- [ ] **CI/CD pipeline** — GitHub Actions for build + test on Windows/Linux/macOS.

### Scanner Modules (Windows)

- [ ] **Prefetch parser** — Parse `.pf` files, extract executed file names, run counts, timestamps, and detect prefetch bypasses (deleted folder, renamed folder, read-only attributes).
- [ ] **BAM registry scanner** — Read `HKLM\SYSTEM\CurrentControlSet\Services\bam\State\UserSettings` to find executed files since last logon, flag unsigned and deleted executables.
- [ ] **USN journal analyzer** — Use `fsutil usn` to parse journal entries, detect suspicious file modifications, special character files, replaced files, and journal clearing.
- [ ] **Event log integrity** — Scan `.evtx` files for corruption, deletion, renaming, and system time change indicators.
- [ ] **Task scheduler scanner** — Enumerate scheduled tasks, check for files with special characters, no name, no extension, and verify digital signatures.
- [ ] **Jump list parser** — Parse both Automatic and Custom Destination jump lists to identify executed files.
- [ ] **Registry artifact scanner** — Detect suspicious registry modifications (AutoRun, persistence keys, bypass keys).
- [ ] **WER crash dump analyzer** — Parse `C:\ProgramData\Microsoft\Windows\WER` for application crash dumps that reveal executed files.
- [ ] **Amcache parser** — Extract file execution artifacts from `Amcache.hve`.
- [ ] **SRUM parser** — Extract application execution data from System Resource Usage Monitor database.
- [ ] **Recent files analyzer** — Parse `Recent` folder shortcuts for recently accessed files.
- [ ] **Shellbag parser** — Analyze Explorer folder history for accessed directories.
- [ ] **MFT parser** — Extract artifacts from the Master File Table for file creation/modification timestamps.

### Process & Memory Scanners

- [ ] **Unsigned DLL scanner** — Enumerate loaded DLLs in all processes, verify digital signatures, flag unsigned modules.
- [ ] **Unsigned executable scanner** — Check all running executables against Windows trust verification.
- [ ] **Memory string scanner** — Scan process memory for known cheat signatures, injected code patterns, and suspicious strings.
- [ ] **Process hollowing detector** — Compare on-disk vs in-memory PE headers to detect hollowed processes.
- [ ] **Thread manipulation detector** — Detect suspicious thread creation patterns, remote thread injection, APC injection.
- [ ] **Token tampering detector** — Detect process token privilege escalation and impersonation.
- [ ] **Restarted process detector** — Track process lifetimes and flag suspiciously restarted processes.

### Macro & Input Scanners

- [ ] **Macro file analyzer** — Detect 30+ macro file modifications across common macro software paths.
- [ ] **Deleted macro detector** — Find evidence of deleted, renamed, or overwritten macro files.
- [ ] **Onboard memory macro scanner** — Use low-level hardware hooks to detect macros stored in mouse/keyboard onboard memory.
- [ ] **Macro process memory scanner** — Regex and pattern filtering of macro software process memory for cheat traces.
- [ ] **MouseKeys detector** — Detect Windows MouseKeys autoclicker usage.

### Anti-Forensic Detection

- [ ] **Event log bypass detector** — Detect .evtx file corruption, deletion, and renaming.
- [ ] **Time manipulation detector** — Cross-reference system time across multiple artifacts to detect time changes.
- [ ] **Disk replacement detector** — Detect formatted or replaced drives by comparing modification times against boot time.
- [ ] **Unplugged device detector** — Track USB device connect/disconnect events and flag suspicious removals.
- [ ] **String cleaner detector** — Detect execution of string cleaning tools (Process Hacker, System Informer).
- [ ] **Code import detector** — Scan console process memory for base64/base32 encoded anti-forensic code.
- [ ] **Shadow copy detector** — Detect volume shadow copy deletion.
- [ ] **AutoRun detector** — Detect AutoRun/autorun.inf based persistence mechanisms.
- [ ] **Network share detector** — Detect execution from network shared locations.
- [ ] **VM detection** — Detect virtual machine environments and host-to-guest input injection.

### Environment & Device Checks

- [ ] **Digital signature verifier** — Windows TrustVerify wrapper for file and catalog signature verification.
- [ ] **Multiple mouse detector** — Detect multiple mouse devices by VID/PID.
- [ ] **XRay texture pack detector** — Detect XRay texture pack files.
- [ ] **Device path mapper** — Map `\Device\HarddiskVolumeN` paths to drive letters.

### Infrastructure & Tooling

- [ ] **JSON result output** — Structured JSON output for all scan results.
- [ ] **HTML report generator** — Generate styled HTML reports from scan results.
- [ ] **PDF report generator** — Generate PDF reports for printing/sharing.
- [ ] **Configuration file support** — YAML/TOML config files for scan profiles (quick scan, full scan, custom).
- [ ] **Plugin manifest format** — Define YAML/JSON schema for scanner plugin metadata.
- [ ] **Plugin sandbox** — Sandboxed execution environment for third-party plugins.
- [ ] **File tracker utility** — Deduplicate scanned files across modules.
- [ ] **Device path utility** — Shared utility for converting Windows device paths to DOS paths.
- [ ] **String utilities** — UTF-8/UTF-16 conversion, string formatting helpers.
- [ ] **Benchmarking suite** — Performance benchmarks for scanner modules.

### Web GUI (Phase 5)

- [ ] **Backend API server** — FastAPI or Actix-web REST API with scan management endpoints.
- [ ] **WebSocket handler** — Real-time scan progress streaming to the frontend.
- [ ] **Frontend dashboard** — React or Svelte SPA with scan history, results, and configuration.
- [ ] **Authentication system** — JWT-based auth with role-based access control.
- [ ] **Report viewer** — In-browser report viewing with export options.
- [ ] **Docker setup** — Dockerfile and docker-compose for self-hosted deployment.
- [ ] **Database layer** — PostgreSQL or SQLite for scan history and user data.

### Documentation

- [ ] **Architecture overview** — Detailed architecture documentation with diagrams.
- [ ] **Scanner developer guide** — Step-by-step guide for writing a new scanner module.
- [ ] **API reference** — Auto-generated API documentation from code comments.
- [ ] **User guide** — End-user documentation for running scans and interpreting results.
- [ ] **Security policy** — Responsible disclosure policy and security contact information.
- [ ] **Changelog** — Maintain a structured changelog following Keep a Changelog format.

### Testing

- [ ] **Unit tests** — Tests for every scanner module with known-good samples.
- [ ] **Integration tests** — End-to-end tests with complete system snapshots.
- [ ] **Fuzz tests** — Fuzzing for all parsers (prefetch, USN journal, registry, jump lists).
- [ ] **Regression tests** — Regression test suite with previously-fixed bugs.
- [ ] **Test fixtures** — Curated sample artifacts for each scanner module.

---

## Community

- **Issues:** Report bugs and request features via GitHub Issues
- **Discussions:** Join the conversation in GitHub Discussions
- **Pull Requests:** Contributions are always welcome

---

## License

TBD
