# information-theory

> Information theory in Rust. How much does a bit really tell you?

[![MIT OR Apache-2.0 License](https://img.shields.io/badge/license-MIT%2FApache--2.0-blue.svg)](./LICENSE)

## What This Does

A complete Shannon information theory library implementing the core mathematical toolkit:

- **Entropy**: Discrete Shannon entropy, differential entropy (Gaussian, uniform), joint entropy, conditional entropy
- **Divergences**: KL divergence, Jensen-Shannon divergence, cross-entropy, divergence matrices
- **Mutual information**: MI via entropy, MI via KL divergence, normalized variants, variation of information
- **Channel capacity**: Discrete memoryless channels, Blahut-Arimoto algorithm, BSC, BEC
- **Source coding**: Huffman coding, optimal code lengths, Kraft inequality verification
- **Rate-distortion theory**: Blahut-Arimoto rate-distortion, distortion-rate function
- **Information inequalities**: Fano's inequality, data processing inequality verification

All functions work with probability distributions as `&[f64]` slices and joint distributions as `nalgebra::DMatrix<f64>`. Every type is serializable via serde.

## Install

```toml
[dependencies]
information-theory = "0.1"
```

## Quick Start

### Entropy

```rust
use information_theory::*;

let h = shannon_entropy(&[0.5, 0.5]); // 1.0 bit (fair coin)

let joint = DMatrix::from_row_slice(2, 2, &[0.25, 0.25, 0.25, 0.25]);
let h_xy = joint_entropy(&joint); // 2.0 bits

let h_gauss = differential_entropy_gaussian(1.0); // ½ log₂(2πe)
```

### Mutual Information

```rust
let px = &[0.5, 0.5];
let py = &[0.5, 0.5];
let joint = DMatrix::from_row_slice(2, 2, &[0.5, 0.0, 0.0, 0.5]);
let mi = mutual_information(px, py, &joint); // 1.0 (perfect correlation)
```

### Channel Capacity (Blahut-Arimoto)

```rust
let bsc = DiscreteMemorylessChannel::new(
    DMatrix::from_row_slice(2, 2, &[0.9, 0.1, 0.1, 0.9])
).unwrap();
let (capacity, optimal_input) = bsc.capacity(1000, 1e-10);
```

### Huffman Coding

```rust
let probs = &[0.4, 0.3, 0.2, 0.1];
let code = huffman_code(probs);
assert!(verify_source_coding_bound(probs, &code.lengths));
```

## License

MIT OR Apache-2.0
