<!-- markdownlint-disable -->

# Hardening Report: Kernel-Guard--bpfcompat/v0.4.0-rc.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Kernel-Guard--bpfcompat/v0.4.0-rc.2** was hardened automatically. 23 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### script-injection (severity: high)

The 'Run bpfcompat' step in action.yml directly interpolates multiple ${{ inputs.* }} expressions into shell code (rule a). Examples include: `artifact_input="${{ inputs.artifact }}"`, `matrix_path="$(resolve_path "${{ inputs.matrix }}")"`, `matrix_input="${{ inputs.matrix }}"`, `out_path="$(resolve_path "${{ inputs.out }}")"`, `manifest_path="$(resolve_path "${{ inputs.manifest }}")"`, `markdown_path="$(resolve_path "${{ inputs.markdown }}")"`, `workdir_path="$(resolve_path "${{ inputs.workdir }}")"`, `suite_path="$(resolve_path "${{ inputs.suite }}")"`, `suite_out_path="$(resolve_path "${{ inputs.suite-out }}")"`, `suite_markdown_path="$(resolve_path "${{ inputs.suite-markdown }}")"`, `binary_input="${{ inputs.binary }}"`, `--timeout "${{ inputs.timeout }}"`, `--concurrency "${{ inputs.concurrency }}"`, `--disk-resize "${{ inputs.disk-resize }}"`, `--validation-mode "${{ inputs.validation-mode }}"`, and `"${{ inputs.keep-vm-on-failure }}"`. All of these are caller-controlled values that are substituted into the shell script before the shell parses it, allowing an attacker to inject arbitrary shell commands via crafted input values. The action correctly routes `inputs.command`, `inputs.command-binary`, and `inputs.command-expect-exit` through env vars, but fails to do so for the remaining inputs.

Locations:

- `action.yml:170`

### unpinned-uses (severity: high)

The workflow consumer-canary.yml uses `Kernel-Guard/bpfcompat@main` which is a mutable branch reference, not a pinned commit SHA. This means the action can be silently updated to a different (potentially malicious) version without any change to the workflow file. All other `uses:` references in this repository are correctly pinned to full 40-character SHA hashes.

Locations:

- `.github/workflows/consumer-canary.yml:148`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.artifact }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:221`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.matrix }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:229`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.matrix }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:236`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.out }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:243`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.manifest }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:244`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.markdown }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:245`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.workdir }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:246`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.suite }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:247`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.suite-out }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:248`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.suite-markdown }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:249`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.binary }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:251`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.timeout }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:284`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.concurrency }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:286`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.timeout }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:356`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.concurrency }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:358`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.disk-resize }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:376`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.disk-resize }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:376`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.disk-resize }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:377`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.validation-mode }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:379`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.validation-mode }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:380`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.keep-vm-on-failure }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:385`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, static-inline-injection, unpinned-uses

**Notes:**

Fixed all 23 findings: (1) Moved all ${{ inputs.* }} expressions from the 'Run bpfcompat' step's run: block into the env: block in action.yml, adding INPUT_ARTIFACT, INPUT_MATRIX, INPUT_OUT, INPUT_MANIFEST, INPUT_MARKDOWN, INPUT_WORKDIR, INPUT_SUITE, INPUT_SUITE_OUT, INPUT_SUITE_MARKDOWN, INPUT_BINARY, INPUT_TIMEOUT, INPUT_CONCURRENCY, INPUT_DISK_RESIZE, INPUT_VALIDATION_MODE, and INPUT_KEEP_VM_ON_FAILURE environment variables. All shell script references were updated to use these env vars instead of inline ${{ }} interpolation. (2) Replaced the mutable Kernel-Guard/bpfcompat@main reference in .github/workflows/consumer-canary.yml with the pinned commit SHA ceddee960e997d0499edbe74c688f035dfa805f8 (with # main comment for readability).

### Iteration 2

**Fixes applied:** script-injection, github-env-injection

**Notes:**

1. script-injection (multiarch-compatibility.yml lines 52, 60, 66): Moved ${{ matrix.qemu_bin }}, ${{ matrix.examples_target }}, and ${{ matrix.image_target }} out of run: shell strings into env: blocks as QEMU_BIN, EXAMPLES_TARGET, and IMAGE_TARGET respectively. Shell scripts now reference these as quoted environment variables ($QEMU_BIN, $EXAMPLES_TARGET, $IMAGE_TARGET). 2. github-env-injection (release-artifacts.yml line 93): Added sanitization of the version and channel variables (derived from github.ref via the REF env var) using `printf '%s' "$var" | tr -d '\n\r'` before writing to $GITHUB_OUTPUT, preventing newline-based injection of arbitrary key=value pairs.

