# RTL Linear Regression Accelerator

Public technical dossier for the RTL hardware-acceleration part of a real-time FPGA crypto market-data project.

> This repository documents the FPGA accelerator architecture and validation approach only. The original source, bitstreams, credentials, notebooks, and trading/API code are not published due to university plaigarism rules. 

## Overview

This project accelerated the training step of a streaming linear-regression model on a PYNQ-Z1 FPGA. The trading system computed live market microstructure features in software, then used the FPGA fabric to accelerate the most arithmetic-heavy part of retraining: accumulating the normal-equation matrices.

My work focused on the **RTL hardware acceleration**:

- fixed-point/integer representation for FPGA-friendly arithmetic,
- parallel multiply-accumulate datapath,
- packed sample input format,
- 512-bit AXI-style sample bus,
- 64-bit accumulation to avoid overflow over thousands of samples,
- PYNQ/PS integration for launching the accelerator and reading results,
- hardware/software comparison against NumPy reference implementations.

## Acceleration Target

For each new feature vector `a` and target value `b`, the normal equations can be updated incrementally:

```text
A'T A' = A^T A + a^T a
A'T b' = A^T b + a^T b
```

That means the hardware does not need to store or multiply the entire historical feature matrix each time. It only needs to stream samples and accumulate outer products.

```mermaid
flowchart TD
    A["Packed 512-bit sample"] --> B["Unpack label + features"]
    B --> C["Parallel feature x feature MACs"]
    B --> D["Parallel feature x label MACs"]
    C --> E["A^T A accumulator"]
    D --> F["A^T b accumulator"]
    E --> G["PS reads results"]
    F --> G
```

## Key Results

| Metric | Result |
|---|---:|
| Board | PYNQ-Z1 |
| PL clock | 100 MHz |
| Batch size | Approx. 4000 samples |
| Hardware accumulation | Approx. 3.49 ms |
| Optimised software batch | Approx. 30.4 ms |
| Unoptimised software batch | Approx. 8.8573 s |
| Average multiplication latency difference | 18.240x |
| Maximum multiplication latency difference | 68.191x |
| DSP48 use | 170 / 220 |
| LUT use | 21,197 / 53,200 |
| FF use | 21,292 / 106,400 |
| BRAM use | 5.5 / 140 |
| On-chip power | Approx. 2.135 W |
| Timing estimate / target | 5.84 ns / 8 ns |


## Technology

- SystemVerilog / RTL hardware design
- FPGA fixed-point/integer arithmetic
- Parallel MAC datapaths
- AXI-style register/memory interfaces
- 512-bit sample packing
- PYNQ-Z1 / Zynq-7000
- Python/PYNQ MMIO integration
- NumPy reference models

