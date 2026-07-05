<!-- markdownlint-disable -->

# Hardening Report: Kernel-Guard--bpfcompat--/v0.3.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **Kernel-Guard--bpfcompat--/v0.3.0** was hardened automatically. 20 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): The 'Run bpfcompat' step in action.yml directly interpolates multiple ${{ inputs.* }} expressions inside run: shell commands. This allows an attacker who controls the calling workflow's inputs to inject arbitrary shell commands. Offending lines include: `artifact_path="$(resolve_path "${{ inputs.artifact }}")"`, `matrix_path="$(resolve_path "${{ inputs.matrix }}")"`, `matrix_input="${{ inputs.matrix }}"`, `out_path="$(resolve_path "${{ inputs.out }}")"`, `manifest_path="$(resolve_path "${{ inputs.manifest }}")"`, `markdown_path="$(resolve_path "${{ inputs.markdown }}")"`, `workdir_path="$(resolve_path "${{ inputs.workdir }}")"`, `suite_path="$(resolve_path "${{ inputs.suite }}")"`, `suite_out_path="$(resolve_path "${{ inputs.suite-out }}")"`, `suite_markdown_path="$(resolve_path "${{ inputs.suite-markdown }}")"`, `binary_input="${{ inputs.binary }}"`, `--timeout "${{ inputs.timeout }}"`, `--concurrency "${{ inputs.concurrency }}"`, `if [[ -n "${{ inputs.validation-mode }}" ]]`, `cmd+=(--validation-mode "${{ inputs.validation-mode }}")`, and `if [[ "${{ inputs.keep-vm-on-failure }}" == "true" ]]`. All inputs should be routed through env: variables and the shell expansions should be double-quoted.

Locations:

- `action.yml:148`
- `action.yml:149`
- `action.yml:155`
- `action.yml:158`
- `action.yml:163`
- `action.yml:164`
- `action.yml:165`
- `action.yml:166`
- `action.yml:167`
- `action.yml:168`
- `action.yml:171`
- `action.yml:200`
- `action.yml:202`
- `action.yml:258`
- `action.yml:260`
- `action.yml:275`
- `action.yml:276`
- `action.yml:280`

### script-injection (severity: high)

Sub-rule (a): The 'Verify runner virtualization', 'Build validator and architecture-specific BPF fixtures', and 'Fetch architecture VM image' steps in multiarch-compatibility.yml directly interpolate ${{ matrix.qemu_bin }}, ${{ matrix.examples_target }}, and ${{ matrix.image_target }} expressions inside run: shell commands. Per the security rules, any ${{ ... }} expression inside a run: block is a script-injection finding regardless of context. Offending lines: `command -v "${{ matrix.qemu_bin }}"`, `make "${{ matrix.examples_target }}"`, `make "${{ matrix.image_target }}"`.

Locations:

- `.github/workflows/multiarch-compatibility.yml:56`
- `.github/workflows/multiarch-compatibility.yml:63`
- `.github/workflows/multiarch-compatibility.yml:69`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.artifact }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:165`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.matrix }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:166`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.matrix }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:173`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.out }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:180`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.manifest }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:181`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.markdown }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:182`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.workdir }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:183`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.suite }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:184`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.suite-out }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:185`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.suite-markdown }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:186`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.binary }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:188`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.timeout }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:221`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.concurrency }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:223`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.timeout }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:293`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.concurrency }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:295`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.validation-mode }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:313`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.validation-mode }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:314`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.keep-vm-on-failure }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:319`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, static-inline-injection

**Notes:**

Fixed all script-injection findings in two files:

1. action.yml ('Run bpfcompat' step): Moved all ${{ inputs.* }} expressions from the run: shell block into the step's env: block. Added 14 new env vars: INPUT_ARTIFACT, INPUT_MATRIX, INPUT_OUT, INPUT_MANIFEST, INPUT_MARKDOWN, INPUT_WORKDIR, INPUT_SUITE, INPUT_SUITE_OUT, INPUT_SUITE_MARKDOWN, INPUT_BINARY, INPUT_TIMEOUT, INPUT_CONCURRENCY, INPUT_VALIDATION_MODE, INPUT_KEEP_VM_ON_FAILURE. All inline ${{ inputs.* }} usages in the shell script were replaced with their corresponding $INPUT_* env var references.

2. .github/workflows/multiarch-compatibility.yml: Added env: blocks to three steps to move ${{ matrix.qemu_bin }}, ${{ matrix.examples_target }}, and ${{ matrix.image_target }} out of run: blocks. Each step now references the value via a plain env var ($QEMU_BIN, $EXAMPLES_TARGET, $IMAGE_TARGET) instead of inline template expressions.

