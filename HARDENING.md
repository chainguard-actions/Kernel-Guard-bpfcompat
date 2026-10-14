<!-- markdownlint-disable -->

# Hardening Report: Kernel-Guard--bpfcompat/v0.4.0-rc.4

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Kernel-Guard--bpfcompat/v0.4.0-rc.4** was hardened automatically. 22 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Rule (a) violation: The 'Run bpfcompat' step's run: block directly interpolates ${{ inputs.* }} expressions into shell commands. Despite the env: block safely routing some inputs, many inputs are still directly embedded in the shell script via template substitution before the shell ever sees them. An attacker-controlled input value containing shell metacharacters (`;`, `|`, `&`, `$(...)`, backticks, etc.) would be executed by the shell. Affected interpolations include:
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
- `--timeout "${{ inputs.timeout }}"`
- `--concurrency "${{ inputs.concurrency }}"`
- `${{ inputs.disk-resize }}` (used in conditional and as CLI argument)
- `${{ inputs.validation-mode }}` (used in conditional and as CLI argument)
- `${{ inputs.keep-vm-on-failure }}` (used in conditional)
All of these should be moved to the env: block and referenced as shell variables instead.

Locations:

- `action.yml:260`
- `action.yml:263`
- `action.yml:268`
- `action.yml:275`
- `action.yml:276`
- `action.yml:277`
- `action.yml:278`
- `action.yml:279`
- `action.yml:280`
- `action.yml:281`
- `action.yml:283`
- `action.yml:323`
- `action.yml:325`
- `action.yml:356`
- `action.yml:358`
- `action.yml:361`
- `action.yml:363`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.artifact }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:291`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.matrix }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:299`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.matrix }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:306`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.out }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:313`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.manifest }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:314`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.markdown }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:315`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.workdir }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:316`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.suite }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:317`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.suite-out }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:318`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.suite-markdown }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:319`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.binary }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:321`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.timeout }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:361`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.concurrency }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:363`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.timeout }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:433`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.concurrency }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:435`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.disk-resize }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:453`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.disk-resize }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:453`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.disk-resize }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:454`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.validation-mode }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:456`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.validation-mode }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:457`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.keep-vm-on-failure }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:462`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, static-inline-injection

**Notes:**

Fixed all script injection findings in the 'Run bpfcompat' step of action.yml. Added 14 missing inputs to the env: block (INPUT_MATRIX, INPUT_OUT, INPUT_MANIFEST, INPUT_MARKDOWN, INPUT_WORKDIR, INPUT_SUITE_OUT, INPUT_SUITE_MARKDOWN, INPUT_BINARY, INPUT_TIMEOUT, INPUT_CONCURRENCY, INPUT_DISK_RESIZE, INPUT_VALIDATION_MODE, INPUT_KEEP_VM_ON_FAILURE) alongside the already-present ones. Replaced all direct ${{ inputs.* }} interpolations in the run: shell script with their corresponding $INPUT_* environment variable references. The remaining ${{ inputs.* }} expressions in the file are only in YAML-level if: conditions and env: mappings, which are not shell injection vectors.

