# Qwen trajectories

Inference trajectories for Qwen3.8-27B in NVFP4 on public benchmarks. The collection
targets five independent runs for every task in the pinned tau2, Terminal-Bench 2,
and SWE-bench Pro V2 inventories. It is in progress; this repository contains the
finalized attempts shared so far.

## Layout

```text
setup.json
tau2/<task>/run-01/trajectory.json.zst
terminal-bench/<task>/run-01/trajectory.json.zst
swe-bench-pro/<task>/run-01/trajectory.json.zst
```

Archives use Zstandard level 19 and are stored directly in ordinary Git. Uncompressed `trajectory.json` files
are retained for research and ignored by Git. `setup.json` records the model,
public checkpoint revision, benchmark revisions, inference settings, and runners.

## Download

Install Git and Zstandard, then clone the repository and decompress the runs you need:

```bash
git clone https://github.com/jupiterepoch/qwen-trajectories.git
cd qwen-trajectories
zstd -d tau2/<task>/run-01/trajectory.json.zst
```

The clone includes the compressed archives. No large-file extension or separate
large-file download is required. Decompressed JSON files remain ignored by Git.

## Contents

Each decompressed JSON has two sections: `turns` and `tokens`. Turns contain the
effective sampling settings and budgets, explicit user messages, actual model
inputs, full emitted reasoning, raw responses, tool calls and observations, and
the final outcome. Token records preserve the original generated IDs and stream
arrival timestamps. Tokens accepted in the same speculative batch can share a
timestamp; these are HTTP arrival times rather than GPU execution times.

Completed benchmark failures and interrupted/error attempts are retained. Consult
the final turn for status, success, evaluator details, and capture completeness.
Tau2 uses the same Qwen model for its agent, simulated user, and semantic judge.

## Publication

Credentials and private host identifiers are sanitized **in place**, restricted
to metadata. Model-visible inputs, outputs, reasoning, tool observations, token
IDs, and timestamps remain unchanged. An attempt is withheld if those protected
fields contain sensitive content. Each finalized run records the publication
policy and metadata-redaction counts in its final turn.

Compression is lossless relative to the sanitized JSON. The uncompressed JSON is
the format used for studies; the compressed archive is the collaboration format.
The public setup file omits credentials, private paths, and unfiltered host
diagnostics. No model weights or private project source code are included.

Benchmark and upstream source licenses apply to their respective contents. See
[THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md) and `licenses/` for provenance and
license notices; this repository does not relicense third-party source excerpts.
