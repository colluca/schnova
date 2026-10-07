![CI](https://github.com/pulp-platform/snitch_cluster/actions/workflows/ci.yml/badge.svg)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

# Schnova

This repository hosts the hardware and software for Schnova, an open-source, streamlined superscalar out-of-order (OoO) RISC-V core for data-intensive workloads. Schnova builds upon the simple blocks of the scalar in-order [Snitch](https://github.com/pulp-platform/snitch_cluster) core. It targets compact and energy-efficient exploitation of instruction-level parallelism (ILP) and is developed as part of the PULP project, a joint effort between ETH Zurich and the University of Bologna.

## Key features

- **Hardware loops.** A hardware-loop instruction (`frep`) delimits a region of superscalar execution and communicates its boundaries and iteration count. Zero-overhead-loop hardware redirects instruction fetch across iterations, so independent instructions from different iterations can coexist in the instruction window without branch prediction or speculative state checkpointing.
- **Dynamic scheduling.** Instructions are fetched, renamed and dynamically scheduled, preserving runtime dependency discovery.
- **Functional-unit-specific reservation stations/issue queues.** Instructions execute out of order across reservation stations but in order within each station, avoiding associative wakeup and selection across the whole instruction window.
- **Physical register reclamation.** Free-list and reference-counting based strategies are supported, as an alternative to reorder-buffer based reclamation.
- **Configurability.** The number of ALUs, LSUs and FPUs, their reservation stations, dispatch buffers and the number of physical registers are parameters of the core (see [`hw/schnova/src/schnova.sv`](hw/schnova/src/schnova.sv)).

Schnova implements IMAFD (ignoring the `aq` and `rl` flags for A), Zicsr and Zicntr, and retains the Snitch `Xssr` and `Xdma` extensions. Precise exceptions are not supported while the core executes in superscalar out-of-order mode.

## Content

What can you expect to find in this repository?

- The Schnova core RTL in [`hw/schnova`](hw/schnova), including frontend, rename, dispatch, reservation stations, functional units, physical register file and tracer, together with synthesis wrappers.
- A Schnova cluster generator and the supporting hardware in `hw/`, used to integrate Schnova cores in a cluster. Select the core with `CORE=schnova` when invoking the Makefile flow.
- A runtime and benchmark kernels (BLAS, DNN, miscellaneous) in `sw/`, including Schnova-specific implementations.
- RTL simulation environments for Verilator, Questa and VCS, and a Schnova-aware trace generator in `util/sz_trace`.
- Performance and area experiments in [`experiments/schnova`](experiments/schnova); see its [README](experiments/schnova/README.md) for instructions.

Schnova is derived from the [Snitch cluster](https://github.com/pulp-platform/snitch_cluster) repository, whose [documentation pages](https://pulp-platform.github.io/snitch_cluster) describe the underlying cluster, runtime and simulation infrastructure.

## License

Schnova is being made available under permissive open source licenses.

The following files are released under Solderpad v0.51 (`SHL-0.51`) see `hw/LICENSE`:

- `hw/`

The `sw/deps` directory references submodules that come with their own
licenses. See the respective folder for the licenses used.

- `sw/deps/`

All other files are released under Apache License 2.0 (`Apache-2.0`) see `LICENSE`.

## Contributing

If you would like to contribute to this project, please check our [contribution guidelines](CONTRIBUTING.md).

## Roadmap

In the near future, Schnova will be merged with [Schnizo](https://github.com/colluca/schnizo). Both will then be merged into the [snitch_cluster](https://github.com/pulp-platform/snitch_cluster) repository, which will be renamed to **PULP-S**. The result will be a single cluster for data-intensive computing, supporting these different core flavours.
