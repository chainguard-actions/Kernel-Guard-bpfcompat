<!-- markdownlint-disable -->

# Hardening Report: Kernel-Guard--bpfcompat/research-v1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Kernel-Guard--bpfcompat/research-v1** was hardened automatically. 22 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Rule (a) violation: The 'Run bpfcompat' step's run: block directly interpolates multiple ${{ inputs.* }} expressions into shell code. GitHub Actions template substitution occurs before the shell processes the string, so an attacker-controlled input value containing shell metacharacters (`;`, `|`, `&`, `$(...)`, backticks, etc.) can inject arbitrary shell commands. The following inputs are interpolated directly:
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
- `if [[ -n "${{ inputs.disk-resize }}" && "${{ inputs.disk-resize }}" != "0" ]]`
- `if [[ -n "${{ inputs.validation-mode }}" ]]`
- `if [[ "${{ inputs.keep-vm-on-failure }}" == "true" ]]`

Fix: route all inputs through env: variables (as is already done for inputs.command, inputs.command-binary, and inputs.command-expect-exit) and reference them as quoted shell variables (e.g., `"$INPUT_ARTIFACT"`) inside the run: block.

Locations:

- `action.yml:163`

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

**Fixes applied:** script-injection, static-inline-injection

**Notes:**

Fixed all script injection vulnerabilities in the 'Run bpfcompat' step of hardened/action/action.yml. Added 16 new env: variables (INPUT_ARTIFACT, INPUT_MATRIX, INPUT_OUT, INPUT_MANIFEST, INPUT_MARKDOWN, INPUT_WORKDIR, INPUT_SUITE, INPUT_SUITE_OUT, INPUT_SUITE_MARKDOWN, INPUT_BINARY, INPUT_TIMEOUT, INPUT_CONCURRENCY, INPUT_DISK_RESIZE, INPUT_VALIDATION_MODE, INPUT_KEEP_VM_ON_FAILURE) alongside the 3 already-existing ones (INPUT_COMMAND, INPUT_COMMAND_BINARY, INPUT_COMMAND_EXPECT_EXIT). All ${{ inputs.* }} expressions were removed from the run: block and replaced with their corresponding $INPUT_* environment variable references. The remaining ${{ inputs.* }} expressions in the file are in YAML if: conditions and the env: block, which are safe.

