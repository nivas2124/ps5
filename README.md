# ChileServer Slopkit

A customized fork of **Slopkit** with additional PS5 payload-loading functionality.

This fork is maintained by **Not-Yitan** and is based on the original Slopkit project by **jordyidk**.

> This repository is a fork/modification of the original project.  
> Full credit for Slopkit and its original implementation belongs to its respective author and contributors.

---

## What's New

The main addition in this fork is **PS5 AUTOLOAD**.

PS5 AUTOLOAD has been integrated directly into Slopkit's existing payload menu and uses the same ELF Loader workflow as the other payloads.

The payload menu is now organized as:

1. **PS5 AUTOLOAD**
2. FTP SERVER
3. GDB SERVER
4. SHELL SERVER
5. WEB SERVER
6. KLOG SERVER

KLOG SERVER is still included and has simply been moved to the bottom of the menu.

---

## PS5 AUTOLOAD

A new payload has been added:

```text
payloads/ps5_autoload.elf
```

Once Slopkit successfully reaches:

```text
ELF LOADER READY
```

**PS5 AUTOLOAD** can be selected manually from the payload menu.

Slopkit sends:

```text
ps5_autoload.elf
```

through the existing ELF Loader connection on:

```text
127.0.0.1:9021
```

The autoloader can then process payloads stored locally on the console.

---

## Local Autoload Directory

PS5 AUTOLOAD is designed to work with:

```text
/data/ps5_autoloader/
```

For example:

```text
/data/ps5_autoloader/
├── autoload.txt
├── ftpsrv-ps5-0.19.elf
├── shadowmountplus.elf
├── kstuff.elf
├── elf-arsenal.elf
├── lapy_jb_daemon.elf
├── pegasus_dl.elf
├── pldmgr_v0.3.6.elf
└── ps5shopappkg-dpi.elf
```

The payloads themselves therefore don't necessarily need to be downloaded from the web interface.

They can remain locally inside:

```text
/data/ps5_autoloader/
```

---

## autoload.txt

`autoload.txt` controls the payload execution sequence.

Example:

```text
!4000
ftpsrv-ps5-0.19.elf
!4000
shadowmountplus.elf
!4000
kstuff.elf
!4000
elf-arsenal.elf
```

Entries beginning with `!` represent a delay before continuing with the next entry.

For example:

```text
!4000
```

means a delay of approximately:

```text
4000 ms
```

The following line specifies the ELF that should be loaded.

---

## Why PS5 AUTOLOAD Is Manual

An earlier experimental version attempted to execute `ps5_autoload.elf` automatically as soon as the ELF Loader reported that it was ready.

That approach could result in instability or a console crash because the payload could be dispatched before the environment had completely stabilized.

For this reason, the current implementation intentionally uses:

```text
ELF LOADER READY
        ↓
Payload menu
        ↓
PS5 AUTOLOAD
        ↓
ps5_autoload.elf
```

instead of immediately executing the autoloader.

---

## New UI Assets

PS5 AUTOLOAD includes dedicated menu graphics matching Slopkit's existing payload interface:

```text
ui/payload-autoload-default.png
ui/payload-autoload-sending.png
ui/payload-autoload-sent.png
ui/payload-autoload-failed.png
```

Using image-based labels also improves compatibility with the PS5 browser, where custom HTML text inside the payload tile did not render consistently during testing.

The states represent:

| State | Description |
|---|---|
| `default` | Payload ready to be selected |
| `sending` | ELF is being sent |
| `sent` | ELF was successfully sent |
| `failed` | Payload transfer failed |

---

## Modified Files

The main Slopkit file modified by this fork is:

```text
slopkit/poops.html
```

New files:

```text
payloads/ps5_autoload.elf

ui/payload-autoload-default.png
ui/payload-autoload-sending.png
ui/payload-autoload-sent.png
ui/payload-autoload-failed.png
```

The existing Slopkit payloads and ELF Loader workflow remain available.

---

## Payload Menu

Current menu layout:

```text
PS5 AUTOLOAD
FTP SERVER
GDB SERVER
SHELL SERVER
WEB SERVER
KLOG SERVER
```

---

## Hosting

Because Slopkit is web-based, this fork can be hosted on a normal HTTP/HTTPS web server.

The ChileServer development instance is designed around:

```text
https://libre.chileserver.cl/
```

The project can also be uploaded to another compatible web host while preserving the repository directory structure.

---

## Important

Payload compatibility depends on the PS5 environment and firmware being used.

Loading multiple ELF payloads in sequence can cause instability when:

- a payload is incompatible with the current firmware;
- two payloads modify conflicting parts of the system;
- a payload is executed before another payload has finished initializing;
- the delay configured in `autoload.txt` is insufficient.

When troubleshooting an autoload configuration, test payloads individually before creating a long sequence.

---

## Credits

### Slopkit

Original project:

**jordyidk / Slopkit**

This repository is based on the original Slopkit project.

### ChileServer modifications

Maintained by:

**Not-Yitan**

Changes introduced by this fork include:

- PS5 AUTOLOAD integration
- `ps5_autoload.elf` payload option
- PS5 AUTOLOAD menu entry
- PS5-browser-compatible AUTOLOAD UI assets
- manual autoloader execution workflow
- payload menu reorganization
- KLOG SERVER moved to the final menu position
- documentation for `/data/ps5_autoloader/`
- `autoload.txt` workflow documentation

---

## Third-Party Components

Some ELF payloads, exploits, libraries, binaries, or other components used with this project may originate from separate projects and authors.

Their inclusion or use does not imply ownership by **Not-Yitan**.

Please refer to the respective upstream projects for their licenses, source code, documentation, and credits.

---

> ### 🐱 One Last Change...
>
> I also replaced the original cat image. No technical reason, no compatibility issue, no performance improvement... **I just don't like cats that much XD.**
> Consider it the most important visual optimization in the ChileServer fork. 😎
>
> — **Not-Yitan**

---

## Disclaimer

This project is intended for research, development, experimentation, and homebrew use on compatible systems.

Use it at your own risk.

Running experimental payloads or incompatible ELF files may cause application crashes, system instability, or console reboots.

---

## Contributors

Original Slopkit development:

**jordyidk**

ChileServer fork / PS5 AUTOLOAD integration:

**Not-Yitan**

https://github.com/user-attachments/assets/09578b30-3741-4d06-9845-2415e31ca2b7
