# Minimum Requirements

The following are the **minimum supported specifications** to install and run ObsidianOS:

- **Architecture:**  
  - x86_64-compatible CPU (64‑bit)  
  - **Note:** ARM, RISC-V, and 32‑bit CPUs are not supported.
- **Memory (RAM):**  
  - **Runtime:** 512 MiB minimum to boot and operate  
  - **Image building with `archiso`:** at least 16 GiB  
  - **Recommended when building:** 32 GiB for faster builds and to avoid swapping
- **Storage:** ~25 GiB total, allocated as:  
  - `/` root: 5 GiB × 2 *(for A/B partition layout)*  
  - `/etc`: 5 GiB  
  - `/var`: 5 GiB
  - `/home`: Rest of available space
  - EFI System Partition (ESP): 512 MiB × 2 *(for A/B ESP layout)*
- **Firmware:**  
  - UEFI (EFI‑based motherboard/firmware) required  
  - Legacy BIOS/CSM mode is **not supported**
- **Connectivity:**  
  - Active internet connection required during system image building

---

## Recommended Requirements

For a smoother installation and better day‑to‑day performance:

- **Architecture:**  
  - Modern multi‑core x86_64 CPU
- **Memory (RAM):**  
  - 2 GiB or more for general use  
  - Higher amounts significantly improve build speeds  
  - *(If using the prebuilt image from the ArchISO and the included /etc/system.sfs, build RAM requirements can be ignored)*
- **Storage:**  
  - 40 GiB or more to allow space for additional packages, logs, and user data
- **Firmware:**  
  - UEFI with Secure Boot disabled  
    - *Secure Boot may be supported in the future, but is currently untested/unsupported*
