# Fixed-Point and Bus Design

The accelerator uses FPGA-friendly numeric representations and wide input packing to minimise datapath overhead.

## Numeric Representation

The final design converted floating-point feature values in software before passing them to the FPGA. The report describes:

| Item | Choice |
|---|---|
| Input feature representation | 16-bit integer-style packed values |
| Product width | 32-bit result from multiplying two 16-bit values |
| Accumulator width | 64-bit |
| Reason for 64-bit accumulation | Prevent overflow across approx. 4000 samples |
| Reported quantisation | Worst-case approx. 1.25% relative error |

The 64-bit accumulator is intentionally wider than the single-product width because accumulation over thousands of samples requires substantially more range.

## Earlier Fixed-Point Prototype

The project also explored an HLS-style fixed-point design with:

- `ap_fixed<18,13>` feature values,
- `ap_fixed<64,40>` accumulators.

The final public claim is kept broader because the report says the later design moved toward a Vivado/SystemVerilog implementation with software-side conversion and 64-bit accumulation.

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

The report attributes approximately 90 us of DMA/register setup overhead to this packed transfer path.

## Design Tradeoff

The design accepts small quantisation error to obtain:

- DSP-efficient multiplication,
- simple hardware unpacking,
- predictable accumulation range,
- lower latency than software matrix accumulation.

