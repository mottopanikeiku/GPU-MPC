# Upstream and local changes

Audit date: 2026-10-05. This compares the repository before this PR, not the rewritten README.

## Actual upstream

The suggested `https://github.com/mpc-msri/GPU-MPC` returned repository-not-found from both `git fetch` and `gh api repos/mpc-msri/GPU-MPC`. The public source is the [GPU-MPC directory in mpc-msri/EzPC](https://github.com/mpc-msri/EzPC/tree/f24bf3e022dde3493cf0ffe3ebd92dd73936b734/GPU-MPC), whose default branch is `master`.

`gh repo view mottopanikeiku/GPU-MPC --json isFork,parent,defaultBranchRef,licenseInfo` reported `isFork: false`, `parent: null`, branch `main`, and `licenseInfo: null`. Lack of GitHub license detection did not mean the source was unlicensed: core files contain the MIT permission notice, with Copyright (c) 2024 Microsoft Research.

## Tree comparison

- Local `origin/main`: `301150ac813fc0adccbd45aab373c2c31f526451`.
- Upstream `master`: `f24bf3e022dde3493cf0ffe3ebd92dd73936b734`.
- Latest upstream commit touching GPU-MPC: `aa0f2f860266fb79901405ef5e9965b8c9fe2e88`.
- Local root tree and upstream `GPU-MPC` tree: both `c01a028d5688b29ce7580897cbac5b0e2f51b310`.
- `git diff --exit-code origin/main^{tree} upstream/master:GPU-MPC` exited 0 with no output.

The tracked contents, file modes, and submodule commit pointers are identical. There are no substantive owner changes in the imported tree. This conclusion applies to the public branch; it does not make a claim about uncommitted files in other clones.

## History comparison

`git rev-list --reverse origin/main` and `git rev-list --reverse upstream/master -- GPU-MPC` each yielded six commits. For each pair below, the local root tree equals the upstream GPU-MPC subtree. Author identity/date, committer identity/date, and full commit message also match.

| Local commit | Upstream EzPC commit | Message |
| --- | --- | --- |
| `ce668110c2624f25a58603d63d8918104b3c90c7` | [473eb3414d1d45ed7fc7a14b2f1cfa65dfec105e](https://github.com/mpc-msri/EzPC/commit/473eb3414d1d45ed7fc7a14b2f1cfa65dfec105e) | GPU MPC (#214) |
| `ca8724efec8ab1cdb63e6730592450b5d1e68ed5` | [45c1063bf9bd5fc1fa1a10e9a75962297e32b1cc](https://github.com/mpc-msri/EzPC/commit/45c1063bf9bd5fc1fa1a10e9a75962297e32b1cc) | Update README.md |
| `5e62a8c7aab0cf1564e61d48bdb90a8ea493b5f9` | [abad2c119fea1118651d88e67862d51bf2fd2dc0](https://github.com/mpc-msri/EzPC/commit/abad2c119fea1118651d88e67862d51bf2fd2dc0) | SIGMA (#219) |
| `b0169501f13145045046072fd1db854364553e74` | [426a47723f1f42843ad1f7f4ba773c846542694e](https://github.com/mpc-msri/EzPC/commit/426a47723f1f42843ad1f7f4ba773c846542694e) | Update README.md |
| `f86d251470e8228490bd084ed0e264cff8943028` | [caca73dcaae82582775ab61b10f38b85cc3479f9](https://github.com/mpc-msri/EzPC/commit/caca73dcaae82582775ab61b10f38b85cc3479f9) | SIGMA README Updates (#220) |
| `301150ac813fc0adccbd45aab373c2c31f526451` | [aa0f2f860266fb79901405ef5e9965b8c9fe2e88](https://github.com/mpc-msri/EzPC/commit/aa0f2f860266fb79901405ef5e9965b8c9fe2e88) | added sigma_offline_online |

[INFERENCE] This is a history-preserving directory extraction, rather than independent development. The import mechanism was not observed; matching trees and commit metadata are the evidence. Different commit IDs alone are not evidence of code changes, because the root trees and parent histories differ from the full EzPC repository.

## Changes in this PR

- `README.md`: explicit upstream, code authors, papers, MIT license, local contribution status, and untested-build warning. Long build and Docker instructions moved to `docs/BUILD.md`; experiment READMEs remain unchanged.
- `docs/UPSTREAM_AUDIT.md`: this pinned history and tree comparison.
- `LICENSE`: copied verbatim from [EzPC's root license at the audited commit](https://github.com/mpc-msri/EzPC/blob/f24bf3e022dde3493cf0ffe3ebd92dd73936b734/LICENSE). Its 2020 copyright is retained, alongside the existing 2024 GPU source headers. Vendored licenses and notices are unchanged.
- `.gitmodules`: restores the four GPU-MPC entries from [upstream's root metadata](https://github.com/mpc-msri/EzPC/blob/f24bf3e022dde3493cf0ffe3ebd92dd73936b734/.gitmodules), removing the `GPU-MPC/` prefix from paths and section names. Existing gitlink commit pointers are unchanged.

The missing metadata was a concrete consequence of the standalone import: before this PR, `git submodule status` exited 128 with `fatal: no submodule mapping found in .gitmodules for path 'experiments/orca/datasets/mnist'`. The four tracked gitlinks are MNIST, weights, CUTLASS, and SEAL. Metadata restoration is not a CUDA reproduction result. Submodule downloads, builds, experiments, and post-change tests were not run.

## Attribution and license scope

Neha Jawalkar is named in `backend/orca.h`, `backend/sigma.h`, and `fss/gpu_relu.cu`. Tanmay Rajore is also named in `Dockerfile_Gen`, `experiments/sigma/llama2.h`, and `experiments/sigma/sigma_offline_online.cu`; Kanav Gupta is credited in Sytorch source headers. Other authors and third-party copyright holders retain their file-level credit.

The [Orca paper](https://eprint.iacr.org/2023/206) credits Neha Jawalkar, Kanav Gupta, Arkaprava Basu, Nishanth Chandran, Divya Gupta, and Rahul Sharma. The [SIGMA paper](https://eprint.iacr.org/2023/1269) credits Kanav Gupta, Neha Jawalkar, Ananta Mukherjee, Nishanth Chandran, Divya Gupta, Ashish Panwar, and Rahul Sharma. Paper authorship is not a claim that every paper author wrote every file.

The upstream root license and GPU-MPC source notices use MIT terms. Preserve those notices when redistributing. Embedded dependencies have their own notices, including the cryptoTools dual-license file and the ArgMapping license; the root license does not replace them.

## Owner decision: proper fork

I recommend a proper fork of `mpc-msri/EzPC`, because the public GPU-MPC tree contains no independent code contribution and a fork would make provenance and future updates visible in GitHub. There is no accessible standalone `mpc-msri/GPU-MPC` repository to fork.

The owner must choose between the current standalone directory mirror and a full EzPC fork. Under the delete-and-re-fork route, the owner would need to back up branch/PR state before deleting anything, then re-fork the actual upstream and decide whether to retain the `GPU-MPC` name. This changes the repository layout and may disrupt links; it is not an action taken by this PR. No deletion, settings change, or fork conversion was performed.
