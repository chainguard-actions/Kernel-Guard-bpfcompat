<!-- markdownlint-disable -->

# Hardening Report: Kernel-Guard--bpfcompat--/v0.2.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **Kernel-Guard--bpfcompat--/v0.2.0** was hardened automatically. 19 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): Multiple ${{ inputs.* }} expressions are directly interpolated into the 'Run bpfcompat' run: shell block in action.yml. This allows any caller of the composite action to inject arbitrary shell commands via crafted input values. Offending lines include: `artifact_path="$(resolve_path "${{ inputs.artifact }}")"`, `matrix_path="$(resolve_path "${{ inputs.matrix }}")"`, `out_path="$(resolve_path "${{ inputs.out }}")"`, `manifest_path="$(resolve_path "${{ inputs.manifest }}")"`, `markdown_path="$(resolve_path "${{ inputs.markdown }}")"`, `workdir_path="$(resolve_path "${{ inputs.workdir }}")"`, `suite_path="$(resolve_path "${{ inputs.suite }}")"`, `suite_out_path="$(resolve_path "${{ inputs.suite-out }}")"`, `suite_markdown_path="$(resolve_path "${{ inputs.suite-markdown }}")"`, `binary_input="${{ inputs.binary }}"`, `--timeout "${{ inputs.timeout }}"`, `--concurrency "${{ inputs.concurrency }}"`, `${{ inputs.validation-mode }}`, and `${{ inputs.keep-vm-on-failure }}`. All inputs should be bound to env: variables and referenced as double-quoted shell variables instead.

Locations:

- `action.yml:146`
- `action.yml:147`
- `action.yml:148`
- `action.yml:149`
- `action.yml:150`
- `action.yml:151`
- `action.yml:152`
- `action.yml:153`
- `action.yml:154`
- `action.yml:156`

### script-injection (severity: high)

Sub-rule (a): ${{ matrix.qemu_bin }}, ${{ matrix.examples_target }}, and ${{ matrix.image_target }} are directly interpolated into run: shell blocks in multiarch-compatibility.yml. The matrix.* context is classified as an untrusted-input expression and must not appear directly inside run: scripts. Offending lines: `command -v "${{ matrix.qemu_bin }}"` (line 52), `make "${{ matrix.examples_target }}"` (line 60), `make "${{ matrix.image_target }}"` (line 66). These should be bound to env: variables and referenced as double-quoted shell variables.

Locations:

- `.github/workflows/multiarch-compatibility.yml:52`
- `.github/workflows/multiarch-compatibility.yml:60`
- `.github/workflows/multiarch-compatibility.yml:66`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.artifact }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:147`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.matrix }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:148`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.out }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:149`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.manifest }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:150`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.markdown }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:151`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.workdir }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:152`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.suite }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:153`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.suite-out }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:154`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.suite-markdown }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:155`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.binary }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:157`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.timeout }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:190`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.concurrency }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:192`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.timeout }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:255`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.concurrency }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:257`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.validation-mode }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:263`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.validation-mode }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:264`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.keep-vm-on-failure }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:269`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, static-inline-injection

**Notes:**

Fixed all script injection findings in two files:

1. action.yml (Run bpfcompat step): Added an env: block with 14 variables (INPUT_ARTIFACT, INPUT_MATRIX, INPUT_OUT, INPUT_MANIFEST, INPUT_MARKDOWN, INPUT_WORKDIR, INPUT_SUITE, INPUT_SUITE_OUT, INPUT_SUITE_MARKDOWN, INPUT_BINARY, INPUT_TIMEOUT, INPUT_CONCURRENCY, INPUT_VALIDATION_MODE, INPUT_KEEP_VM_ON_FAILURE) mapping all ${{ inputs.* }} expressions. All references in the run: shell block now use the corresponding $INPUT_* shell variables.

2. .github/workflows/multiarch-compatibility.yml: Fixed three steps by adding env: blocks — QEMU_BIN for the 'Verify runner virtualization' step, EXAMPLES_TARGET for the 'Build validator' step, and IMAGE_TARGET for the 'Fetch architecture VM image' step. All ${{ matrix.* }} expressions in run: blocks are now accessed via environment variables.

### Iteration 2

**Fixes applied:** github-env-injection

**Notes:**

Fixed the 'Resolve build metadata' step in .github/workflows/release-artifacts.yml. Added sanitization of VERSION, COMMIT, and BUILD_DATE values using `printf '%s' "$VAR" | tr -d '\n\r'` before writing them to $GITHUB_OUTPUT. The sanitized values (SAFE_VERSION, SAFE_COMMIT, SAFE_BUILD_DATE) are now used in the echo statements that write to $GITHUB_OUTPUT, preventing newline injection attacks via specially crafted tag names or other ref values.

