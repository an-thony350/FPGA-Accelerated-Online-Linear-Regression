# Fixed-Point and Bus Design

The accelerator uses FPGA-friendly numeric representations and wide input packing to minimise datapath overhead.

## Numeric Representation

The final design converted floating-point feature values in software before passing them to the FPGA.

| Item | Choice |
|---|---|
| Input feature representation | 16-bit integer-style packed values |
| Product width | 32-bit result from multiplying two 16-bit values |
| Accumulator width | 64-bit |
| Reason for 64-bit accumulation | Prevent overflow across approx. 4000 samples |
| Reported quantisation | Worst-case approx. 1.25% relative error |

The 64-bit accumulator is intentionally wider than the single-product width because accumulation over thousands of samples requires substantially more range.

## 512-Bit Sample Packing

Each memory word carries a complete sample. This means the hardware can unpack the label and all features in one cycle through bit-field extraction rather than repeatedly reading narrow words.

```text
512-bit sample word
├── label
├── feature_0
├── feature_1
├── ...
└── feature_n
```

## Design Tradeoff

The design accepts small quantisation error to obtain:

- DSP-efficient multiplication,
- simple hardware unpacking,
- predictable accumulation range,
- lower latency than software matrix accumulation.

