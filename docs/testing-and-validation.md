# Testing and Validation

The accelerator was validated by comparing hardware output against two software implementations.

## Three-Way Comparison

| Implementation | Purpose |
|---|---|
| Unoptimised software | Stores full historical matrices and recomputes reference result. |
| Optimised software | Incrementally accumulates `A^T A` and `A^T b` in software. |
| Hardware accelerator | Computes the same accumulations in the FPGA fabric. |

Agreement between the two software paths validates the modelling pipeline. Agreement between optimised software and hardware validates the fixed-point/RTL datapath.

## Bugs This Caught

The report notes that continuous comparison helped detect:

- accumulator overflow,
- early normalisation mismatch,
- fixed-point precision issues.

## Performance Validation

| Metric | Achieved |
|---|---:|
| Hardware accumulation | Approx. 3.49 ms |
| Optimised software accumulation | Approx. 30.4 ms |
| Unoptimised software path | Approx. 8.8573 s |

The final system found that kernel compute time was no longer the main bottleneck; DMA/read/write overhead became more important after acceleration.

## Scope

The paper-trading result belongs to the wider application and is not used here as proof of trading performance. For this RTL-focused dossier, the important validation is numerical agreement and latency improvement for the accumulation kernel.

