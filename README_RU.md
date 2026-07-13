# Reproducible Parallel Reductions

A C++20 research project for studying floating-point reduction algorithms on
CPUs and GPUs.

The project compares several approaches, including naive summation,
higher-precision accumulation, pairwise reduction, Kahan and Neumaier
summation, deterministic reduction trees, reproducible accumulators,
multithreaded CPU implementations, and CUDA reductions.

The main goal is to study the trade-offs between numerical accuracy,
bitwise reproducibility, latency, throughput, memory bandwidth, and parallel
scalability under different input distributions and hardware configurations.

The resulting system will be able to select an appropriate reduction strategy
based on input properties, accuracy requirements, reproducibility guarantees,
and the available CPU or GPU architecture.

The resulting repository will contain a reusable C++ library, high-precision
reference implementations, adversarial dataset generators, correctness and
reproducibility tests, CPU and GPU benchmarks, optimized library baselines,
experiment scripts, performance and error graphs, and a reproducible technical
report.
