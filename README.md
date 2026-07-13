# Reproducible Parallel Reductions

**Accuracy, reproducibility, and performance trade-offs in parallel floating-point reductions on CPUs and GPUs.**

[Русская версия](README_RU.md)

`reproducible-reductions` is a research-oriented C++20 project for the comparative study of multiple floating-point reduction approaches.

This repository is **not** intended to demonstrate one preferred summation algorithm. It is designed as an experimental framework for implementing, validating, benchmarking, and comparing several reduction strategies under different numerical workloads and hardware configurations.

## Problem

Floating-point addition is not associative:

```text
(a + b) + c != a + (b + c)
```

Parallel execution changes the order in which partial results are combined. Therefore, the final result may depend on the reduction tree, thread count, scheduling, compiler transformations, floating-point format, and hardware architecture.

The project studies the trade-off between three goals:

- **accuracy** — closeness to a high-precision reference result;
- **reproducibility** — stability of the bit pattern across repeated runs, thread counts, and architectures;
- **performance** — latency, throughput, scalability, and effective memory bandwidth.

## Research question

> What performance cost is required to achieve a given level of numerical accuracy and reproducibility in parallel floating-point reductions?

A longer-term question is whether a workload-aware policy can select an appropriate reduction approach based on input properties, accuracy requirements, reproducibility requirements, and the available CPU or GPU architecture.

## Approaches under study

The project will incrementally implement and evaluate several algorithm families:

| Family | Planned approaches | Main purpose |
|---|---|---|
| Baselines | naive `float`, higher-precision accumulation | establish speed and error baselines |
| Structured summation | pairwise and tree reductions | improve numerical behavior and expose parallel structure |
| Compensated summation | Kahan and Neumaier | reduce rounding error |
| Deterministic reductions | fixed reduction trees | provide run-to-run and cross-thread reproducibility |
| Reproducible accumulators | binned or exponent-aware methods | target stronger reproducibility guarantees |
| Parallel CPU | OpenMP and explicit tree implementations | study multicore scalability and scheduling effects |
| GPU | CUDA block and hierarchical reductions | study GPU throughput and reduction-order effects |
| Library baselines | optimized CPU/GPU libraries | compare custom implementations with established systems |

No method is assumed to be universally best. The goal is to identify the conditions under which each approach belongs to the accuracy–reproducibility–performance Pareto frontier.

## Experimental workloads

The benchmark suite will include controlled datasets with different numerical properties:

- random values;
- wide exponent ranges;
- alternating signs;
- strong cancellation;
- values sorted by magnitude;
- adversarial orderings;
- dot products and vector norms;
- inputs derived from numerical algorithms.

High-precision arithmetic will be used to construct reference results.

## Evaluation metrics

### Numerical quality

- absolute error;
- relative error;
- ULP error;
- agreement with a high-precision reference.

### Reproducibility

- bitwise equality across repeated runs;
- reproducibility across thread counts;
- reproducibility across scheduling configurations;
- later, reproducibility across CPU and GPU implementations.

### Performance

- latency;
- elements processed per second;
- effective memory bandwidth;
- parallel speedup and efficiency;
- algorithmic overhead relative to the fastest baseline.

## Experimental methodology

Each result should be reproducible from repository code and configuration files. Experiments will separate:

- input generation;
- reduction execution;
- reference-result computation;
- numerical-error analysis;
- timing and hardware counters;
- CSV export and plotting.

The benchmark methodology will report compiler, flags, CPU/GPU model, thread count, dataset parameters, warm-up policy, number of repetitions, and summary statistics.

## Roadmap

- [ ] C++20 project infrastructure
- [ ] naive `float` and higher-precision baselines
- [ ] pairwise, Kahan, and Neumaier implementations
- [ ] high-precision reference computation
- [ ] adversarial workload generator
- [ ] correctness and numerical-error tests
- [ ] reproducible CPU benchmark suite
- [ ] multicore reductions
- [ ] fixed deterministic reduction trees
- [ ] SIMD and memory-bandwidth analysis
- [ ] CUDA reduction kernels
- [ ] optimized library baselines
- [ ] workload-aware algorithm selection
- [ ] downstream experiments with dot products and iterative numerical methods

## Planned repository structure

```text
reproducible-reductions/
├── include/                 # public C++ interfaces
├── src/                     # sequential and parallel implementations
├── tests/                   # correctness and reproducibility tests
├── benchmarks/              # performance benchmarks
├── apps/                    # experiment runners and CLI tools
├── experiments/
│   ├── configs/             # reproducible experiment definitions
│   ├── results/             # generated CSV files, excluded when appropriate
│   └── scripts/             # Python analysis and plotting
└── reports/                 # figures, tables, and technical notes
```

## Current status

**Planned / research design stage.**

The first implementation phase will be CPU-only and deliberately narrow: sequential baselines, compensated summation, a high-precision reference, correctness tests, and reproducible benchmarks.

CUDA, distributed reductions, and adaptive selection will be added only after the CPU methodology is reliable.

## Technology direction

- C++20;
- CMake;
- GoogleTest;
- Google Benchmark;
- sanitizers;
- Python for result analysis and plotting;
- OpenMP later;
- CUDA later.

## License

This project is licensed under the MIT License.