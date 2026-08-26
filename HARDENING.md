<!-- markdownlint-disable -->

# Hardening Report: Kernel-Guard--bpfcompat/v0.3.7

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Kernel-Guard--bpfcompat/v0.3.7** was hardened automatically. 22 finding(s) were identified and resolved across 3 iteration(s).

## Findings Fixed

### script-injection (severity: high)

The 'Run bpfcompat' step in action.yml directly interpolates ${{ inputs.* }} expressions inside the run: shell script body (rule a). While some inputs (command, command-binary, command-expect-exit) are correctly routed through env: variables, many others are interpolated directly into the shell script before the shell ever sees them, allowing an attacker-controlled value to inject arbitrary shell syntax. Offending lines include:
- `artifact_input="${{ inputs.artifact }}"`
- `matrix_path="$(resolve_path "${{ inputs.matrix }}")"`
- `matrix_input="${{ inputs.matrix }}"`
- `out_path="$(resolve_path "${{ inputs.out }}")"`
- `manifest_path="$(resolve_path "${{ inputs.manifest }}")"`
- `markdown_path="$(resolve_path "${{ inputs.markdown }}")"`
- `workdir_path="$(resolve_path "${{ inputs.workdir }}")"`
- `suite_path="$(resolve_path "${{ inputs.suite }}")"`
- `suite_out_path="$(resolve_path "${{ inputs.suite-out }}")"`
- `suite_markdown_path="$(resolve_path "${{ inputs.suite-markdown }}")"`
- `binary_input="${{ inputs.binary }}"`
- `--timeout "${{ inputs.timeout }}"` (appears twice)
- `--concurrency "${{ inputs.concurrency }}"` (appears twice)
- `${{ inputs.disk-resize }}` (appears twice in conditions and array)
- `${{ inputs.validation-mode }}` (appears twice)
- `${{ inputs.keep-vm-on-failure }}` (appears twice)
All these inputs should be routed through env: variables and referenced as shell variables instead.

Locations:

- `action.yml:182`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.artifact }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:254`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.matrix }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:262`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.matrix }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:269`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.out }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:276`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.manifest }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:277`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.markdown }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:278`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.workdir }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:279`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.suite }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:280`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.suite-out }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:281`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.suite-markdown }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:282`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.binary }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:284`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.timeout }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:317`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.concurrency }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:319`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.timeout }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:389`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.concurrency }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:391`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.disk-resize }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:409`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.disk-resize }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:409`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.disk-resize }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:410`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.validation-mode }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:412`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.validation-mode }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:413`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.keep-vm-on-failure }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:418`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, static-inline-injection

**Notes:**

Added 16 new env: variables to the 'Run bpfcompat' step (INPUT_ARTIFACT, INPUT_MATRIX, INPUT_OUT, INPUT_MANIFEST, INPUT_MARKDOWN, INPUT_WORKDIR, INPUT_SUITE, INPUT_SUITE_OUT, INPUT_SUITE_MARKDOWN, INPUT_BINARY, INPUT_TIMEOUT, INPUT_CONCURRENCY, INPUT_DISK_RESIZE, INPUT_VALIDATION_MODE, INPUT_KEEP_VM_ON_FAILURE) and replaced all inline ${{ inputs.* }} expressions in the run: shell script with references to these environment variables. All 22 findings (script-injection and static-inline-injection) are resolved. The remaining ${{ inputs.* }} occurrences in the file are in YAML if: conditions and the env: block, which are safe.

### Iteration 2

**Fixes applied:** script-injection, unpinned-uses

**Notes:**

Fixed script-injection in .github/workflows/multiarch-compatibility.yml: moved ${{ matrix.qemu_bin }}, ${{ matrix.examples_target }}, and ${{ matrix.image_target }} out of run: blocks into step-level env: blocks (as QEMU_BIN, EXAMPLES_TARGET, IMAGE_TARGET respectively), then referenced them as plain shell variables. Fixed unpinned-uses in .github/workflows/consumer-canary.yml: pinned Kernel-Guard/bpfcompat@main to the resolved commit SHA 94b8db8a9110610c69c5e231d8eb89da5789b64e with a # main comment.

### Iteration 3

**Fixes applied:** github-env-injection

**Notes:**

Fixed two github-env-injection findings:
1. release-artifacts.yml: Added sanitization (`printf '%s' ... | tr -d '\n\r'`) for VERSION (derived from github.ref), COMMIT, and BUILD_DATE before writing to $GITHUB_OUTPUT in the 'Resolve build metadata' step.
2. ghcr-image.yml: Added sanitization (`printf '%s' ... | tr -d '\n\r'`) for ver (derived from GITHUB_REF_NAME/GITHUB_REF_TYPE), GITHUB_SHA, and the date value before writing to $GITHUB_OUTPUT in the 'Resolve build metadata' step.
Both fixes prevent crafted ref/tag names containing newlines from injecting additional key=value pairs into GITHUB_OUTPUT.

