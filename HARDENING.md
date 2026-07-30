<!-- markdownlint-disable -->

# Hardening Report: Kernel-Guard--bpfcompat/v0.4.0-rc.3

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Kernel-Guard--bpfcompat/v0.4.0-rc.3** was hardened automatically. 24 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

The 'Run bpfcompat' step in action.yml directly interpolates multiple ${{ inputs.* }} expressions into the shell script body (rule a). An attacker-controlled caller can inject arbitrary shell commands via these inputs. Affected interpolations include: artifact_input="${{ inputs.artifact }}", matrix_path="$(resolve_path "${{ inputs.matrix }}")", matrix_input="${{ inputs.matrix }}", out_path="$(resolve_path "${{ inputs.out }}")", manifest_path="$(resolve_path "${{ inputs.manifest }}")", markdown_path="$(resolve_path "${{ inputs.markdown }}")", workdir_path="$(resolve_path "${{ inputs.workdir }}")", suite_path="$(resolve_path "${{ inputs.suite }}")", suite_out_path="$(resolve_path "${{ inputs.suite-out }}")", suite_markdown_path="$(resolve_path "${{ inputs.suite-markdown }}")", binary_input="${{ inputs.binary }}", --timeout "${{ inputs.timeout }}", --concurrency "${{ inputs.concurrency }}", "${{ inputs.disk-resize }}", "${{ inputs.validation-mode }}", "${{ inputs.keep-vm-on-failure }}". All of these should be routed through env: variables instead.

Locations:

- `action.yml:200`

### script-injection (severity: high)

The 'Verify runner virtualization' and 'Build validator' steps in multiarch-compatibility.yml directly interpolate ${{ matrix.* }} expressions into run: shell blocks (rule a). Specifically: `command -v "${{ matrix.qemu_bin }}"`, `make "${{ matrix.examples_target }}"`, and `make "${{ matrix.image_target }}"`. Per the check rules, matrix.* is an untrusted-input source and must not be interpolated directly into run: blocks.

Locations:

- `.github/workflows/multiarch-compatibility.yml:50`
- `.github/workflows/multiarch-compatibility.yml:60`
- `.github/workflows/multiarch-compatibility.yml:65`

### unpinned-uses (severity: high)

The consumer-canary.yml workflow uses `Kernel-Guard/bpfcompat@main` which is pinned to a branch name rather than a full 40-character commit SHA. This means the action can be silently updated to a different (potentially malicious) version without the workflow noticing. It should be pinned to a specific commit SHA.

Locations:

- `.github/workflows/consumer-canary.yml:120`

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

Fixed all findings across three files:

1. action.yml (Run bpfcompat step): Moved all 18 ${{ inputs.* }} expressions from the run: shell block into the env: block as named environment variables (INPUT_ARTIFACT, INPUT_MATRIX, INPUT_OUT, INPUT_MANIFEST, INPUT_MARKDOWN, INPUT_WORKDIR, INPUT_SUITE, INPUT_SUITE_OUT, INPUT_SUITE_MARKDOWN, INPUT_BINARY, INPUT_TIMEOUT, INPUT_CONCURRENCY, INPUT_DISK_RESIZE, INPUT_VALIDATION_MODE, INPUT_KEEP_VM_ON_FAILURE). All shell references updated to use $VAR_NAME form.

2. .github/workflows/multiarch-compatibility.yml: Moved ${{ matrix.qemu_bin }}, ${{ matrix.examples_target }}, and ${{ matrix.image_target }} from run: blocks into env: blocks as QEMU_BIN, EXAMPLES_TARGET, and IMAGE_TARGET respectively.

3. .github/workflows/consumer-canary.yml: Pinned `Kernel-Guard/bpfcompat@main` to full commit SHA `Kernel-Guard/bpfcompat@7075bde70566334f8c96337f9739d36a6b311b9f # main`.

