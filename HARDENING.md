<!-- markdownlint-disable -->

# Hardening Report: Kernel-Guard--bpfcompat/v0.3.6

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Kernel-Guard--bpfcompat/v0.3.6** was hardened automatically. 24 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

The 'Run bpfcompat' step in action.yml directly interpolates multiple ${{ inputs.* }} expressions inside the run: shell block. Affected inputs include: inputs.artifact (e.g. `artifact_input="${{ inputs.artifact }}"`), inputs.matrix (used twice), inputs.out, inputs.manifest, inputs.markdown, inputs.workdir, inputs.suite, inputs.suite-out, inputs.suite-markdown, inputs.binary, inputs.timeout (e.g. `--timeout "${{ inputs.timeout }}"`), inputs.concurrency, inputs.disk-resize (e.g. `if [[ -n "${{ inputs.disk-resize }}" && "${{ inputs.disk-resize }}" != "0" ]]`), inputs.validation-mode, and inputs.keep-vm-on-failure. A caller can supply a crafted input value containing shell metacharacters (`;`, `|`, `$(...)`, etc.) that will be executed by the shell before any quoting takes effect. Sub-rule (a): direct expression interpolation in run: block.

Locations:

- `action.yml:161`
- `action.yml:165`
- `action.yml:169`
- `action.yml:172`
- `action.yml:173`
- `action.yml:174`
- `action.yml:175`
- `action.yml:176`
- `action.yml:177`
- `action.yml:180`
- `action.yml:202`
- `action.yml:204`
- `action.yml:247`
- `action.yml:249`
- `action.yml:264`
- `action.yml:267`
- `action.yml:270`
- `action.yml:273`

### script-injection (severity: high)

In .github/workflows/multiarch-compatibility.yml, three run: blocks directly interpolate ${{ matrix.* }} expressions into shell commands. Specifically: `command -v "${{ matrix.qemu_bin }}"` (Verify runner virtualization step), `make "${{ matrix.examples_target }}"` (Build step), and `make "${{ matrix.image_target }}"` (Fetch image step). Any ${{ ... }} expression directly inside a run: shell string is a script-injection finding per sub-rule (a), regardless of whether the context appears attacker-controlled.

Locations:

- `.github/workflows/multiarch-compatibility.yml:44`
- `.github/workflows/multiarch-compatibility.yml:50`
- `.github/workflows/multiarch-compatibility.yml:55`

### unpinned-uses (severity: high)

In consumer-canary.yml, the step 'Validate via the published action (command mode, source build)' uses `Kernel-Guard/bpfcompat@main`, which is a mutable branch reference rather than a pinned 40-character commit SHA. This means the action can be silently updated to a different (potentially malicious) version without any change to the workflow file.

Locations:

- `.github/workflows/consumer-canary.yml:113`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.artifact }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:212`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.matrix }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:220`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.matrix }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:227`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.out }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:234`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.manifest }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:235`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.markdown }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:236`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.workdir }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:237`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.suite }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:238`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.suite-out }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:239`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.suite-markdown }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:240`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.binary }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:242`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.timeout }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:275`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.concurrency }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:277`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.timeout }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:347`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.concurrency }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:349`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.disk-resize }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:367`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.disk-resize }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:367`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.disk-resize }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:368`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.validation-mode }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:370`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.validation-mode }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:371`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.keep-vm-on-failure }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:376`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, static-inline-injection, unpinned-uses

**Notes:**

Fixed all script injection issues in action.yml by moving all ${{ inputs.* }} expressions from the run: shell block into the env: map of the 'Run bpfcompat' step (added INPUT_ARTIFACT, INPUT_MATRIX, INPUT_OUT, INPUT_MANIFEST, INPUT_MARKDOWN, INPUT_WORKDIR, INPUT_SUITE, INPUT_SUITE_OUT, INPUT_SUITE_MARKDOWN, INPUT_BINARY, INPUT_TIMEOUT, INPUT_CONCURRENCY, INPUT_DISK_RESIZE, INPUT_VALIDATION_MODE, INPUT_KEEP_VM_ON_FAILURE). Fixed script injection in .github/workflows/multiarch-compatibility.yml by moving ${{ matrix.qemu_bin }}, ${{ matrix.examples_target }}, and ${{ matrix.image_target }} into env: blocks (MATRIX_QEMU_BIN, MATRIX_EXAMPLES_TARGET, MATRIX_IMAGE_TARGET) for the three affected run: steps. Pinned the mutable Kernel-Guard/bpfcompat@main reference in consumer-canary.yml to commit SHA 9b557c380dd3327ff0a5883d3526c45b8e73e87e with a # main comment.

