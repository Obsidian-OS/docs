---
title: System Requirements
nav_order: 2
---

## Minimum system requirements

The following are the minimum requirements to install and run ObsidianOS:

- **Architecture:** x86_64-compatible processor  
- **Memory:** 512 MiB RAM  
  - 16 GiB required to build an image with `archiso`  
  - 32 GiB recommended for building  
- **Storage:** 25 GiB total, allocated as:  
  - `/` root: 5 GiB × 2  
  - `/etc`: 5 GiB  
  - `/var`: 5 GiB  
  - EFI System Partition (ESP): 512 MiB × 2  
- **Firmware:** EFI-based motherboard (UEFI)

> **Note:** An active internet connection is required during installation.

---

## Recommended system requirements

For a smoother experience, the following specifications are recommended:

- **Architecture:** x86_64-compatible processor with multiple cores
- **Memory:** 2 GiB RAM or more for general use (more needed during building of image, you can just use the default one provided in the archiso)
- **Storage:** 40 GiB or more for additional packages, logs, and user data
- **Firmware:** EFI-based motherboard (UEFI) with Secure Boot disabled
