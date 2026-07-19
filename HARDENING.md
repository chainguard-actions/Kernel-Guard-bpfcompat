<!-- markdownlint-disable -->

# Hardening Report: Kernel-Guard--bpfcompat/v0.2.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Kernel-Guard--bpfcompat/v0.2.0** was hardened automatically. 20 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

The 'Run bpfcompat' step in action.yml directly interpolates ${{ inputs.* }} expressions into the shell script (sub-rule a). The following inputs are substituted directly into the run: block without being routed through env: variables first: inputs.artifact, inputs.matrix, inputs.out, inputs.manifest, inputs.markdown, inputs.workdir, inputs.suite, inputs.suite-out, inputs.suite-markdown, inputs.binary, inputs.timeout, inputs.concurrency, inputs.validation-mode, and inputs.keep-vm-on-failure. For example: `artifact_path="$(resolve_path "${{ inputs.artifact }}")"`, `--timeout "${{ inputs.timeout }}"`, `if [[ -n "${{ inputs.validation-mode }}" ]]`, and `if [[ "${{ inputs.keep-vm-on-failure }}" == "true" ]]`. An attacker who controls these inputs (e.g. via a calling workflow) can inject arbitrary shell commands.

Locations:

- `action.yml:147`

### script-injection (severity: high)

Three run: blocks in multiarch-compatibility.yml directly interpolate ${{ matrix.* }} expressions into shell commands (sub-rule a). (1) 'Verify runner virtualization' step: `command -v "${{ matrix.qemu_bin }}"` — line ~52. (2) 'Build validator and architecture-specific BPF fixtures' step: `make "${{ matrix.examples_target }}"` — line ~59. (3) 'Fetch architecture VM image' step: `make "${{ matrix.image_target }}"` — line ~65. Any ${{ ... }} expression directly inside a run: block is a script-injection risk regardless of the context it reads from.

Locations:

- `.github/workflows/multiarch-compatibility.yml:52`
- `.github/workflows/multiarch-compatibility.yml:59`
- `.github/workflows/multiarch-compatibility.yml:65`

### github-env-injection (severity: high)

In release-artifacts.yml, the 'Resolve build metadata' step writes a value derived from ${{ github.ref }} to $GITHUB_OUTPUT without sanitization. The env var REF is set to ${{ github.ref }}, then VERSION is derived from it via `${REF#refs/tags/}` or `git describe`, and finally `echo "version=$VERSION" >> "$GITHUB_OUTPUT"` is executed without the required `printf '%s' ... | tr -d '\n\r'` sanitization step. Per the check rules, github.* values are untrusted and any write of a derived value to a special environment file must be sanitized first.

Locations:

- `.github/workflows/release-artifacts.yml:70`

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

**Fixes applied:** script-injection, static-inline-injection, github-env-injection

**Notes:**

Fixed all three categories of findings:

1. action.yml (script-injection / static-inline-injection): Added an env: block to the 'Run bpfcompat' step with 14 INPUT_* variables covering all ${{ inputs.* }} expressions (artifact, matrix, out, manifest, markdown, workdir, suite, suite-out, suite-markdown, binary, timeout, concurrency, validation-mode, keep-vm-on-failure). All run: block references now use plain $INPUT_* shell variables.

2. multiarch-compatibility.yml (script-injection): Added env: blocks to three steps — 'Verify runner virtualization' (QEMU_BIN), 'Build validator and architecture-specific BPF fixtures' (EXAMPLES_TARGET), and 'Fetch architecture VM image' (IMAGE_TARGET) — replacing inline ${{ matrix.* }} expressions with safe env var references.

3. release-artifacts.yml (github-env-injection): The 'Resolve build metadata' step now sanitizes VERSION, COMMIT, and BUILD_DATE through `printf '%s' "$VAR" | tr -d '\n\r'` before writing to $GITHUB_OUTPUT, preventing newline injection.

