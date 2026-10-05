> **Upstream attribution:** This is a standalone copy of [GPU-MPC from mpc-msri/EzPC](https://github.com/mpc-msri/EzPC/tree/master/GPU-MPC), not original work by Alp Cetin. Source headers credit Neha Jawalkar, Tanmay Rajore, and, in Sytorch, Kanav Gupta; Microsoft Research holds the GPU code copyright. It implements [Orca: FSS-based Secure Training and Inference with GPUs](https://eprint.iacr.org/2023/206) and [SIGMA: Secure GPT Inference with Function Secret Sharing](https://eprint.iacr.org/2023/1269). The upstream code uses the [MIT license](LICENSE); existing source and dependency notices remain in force.

# GPU-MPC

GPU-MPC is research code for GPU-accelerated secure two-party machine-learning training and inference using function secret sharing.

The papers ask whether GPU acceleration and smaller preprocessing keys can make secure training and transformer inference practical.

## What was built

The upstream Orca implementation is in [backend/orca.h](backend/orca.h), the SIGMA implementation is in [backend/sigma.h](backend/sigma.h), and GPU cryptographic primitives include [fss/gpu_relu.cu](fss/gpu_relu.cu). Experiment drivers and configurations are under `experiments/orca/` and `experiments/sigma/`.

## Result and local status

This copy has no independently reproduced performance result. CUDA builds and experiments were **not run** for this attribution audit. The papers report the research results; those are not measurements made by Alp Cetin.

## What I changed

Before this PR, I had made no substantive changes to the imported code: the tree at [`301150ac`](https://github.com/mottopanikeiku/GPU-MPC/commit/301150ac813fc0adccbd45aab373c2c31f526451) exactly matches upstream's `GPU-MPC` directory at [`f24bf3e0`](https://github.com/mpc-msri/EzPC/tree/f24bf3e022dde3493cf0ffe3ebd92dd73936b734/GPU-MPC). All imported commits match upstream directory snapshots and author metadata.

This PR adds attribution, the upstream `LICENSE`, and a factual change audit, and moves the long build instructions to `docs/BUILD.md`. It also restores the missing [`.gitmodules`](.gitmodules) mappings needed to initialize the imported dependencies; no protocol implementation or dependency commit changed. See the [audit, license scope, and fork recommendation](docs/UPSTREAM_AUDIT.md).

## Reproduction

Use NVIDIA GPUs and the upstream environment documented in [the build guide](docs/BUILD.md). After reviewing `setup.sh` and installing GPU drivers and CUDA, the upstream build sequence is:

```sh
export CUDA_VERSION=11.7 GPU_ARCH=86
sh setup.sh
make orca sigma
```

These are build commands, not a complete experiment. Configure both parties and follow the [Orca](experiments/orca/README.md#run-orca) or [SIGMA](experiments/sigma/README.md#run-sigma) run instructions. The setup script installs system packages and downloads dependencies and datasets. GPU compute and storage costs depend on the machines used; none were incurred here. The restored submodule mappings have not been exercised by downloading dependencies.

## Limitations

- This is an academic proof of concept, not production cryptography; the upstream warning says it has not received careful code review.
- No local correctness, security, or performance validation is claimed.
- Orca requires substantial key storage; SIGMA keeps large preprocessing keys in CPU memory. See their experiment READMEs before allocating hardware.
- The inherited build targets an older CUDA/toolchain environment; compatibility with newer environments is untested here.
- This standalone repository is not a GitHub fork, so GitHub does not display its upstream relationship.

## Prior work

The implementation and experiment instructions come from [mpc-msri/EzPC](https://github.com/mpc-msri/EzPC/tree/master/GPU-MPC). The [Orca](https://eprint.iacr.org/2023/206) and [SIGMA](https://eprint.iacr.org/2023/1269) paper pages list their full author teams; file-level author and copyright notices are preserved.
