<!-- markdownlint-disable -->

# Hardening Report: Kernel-Guard--bpfcompat/v0.3.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Kernel-Guard--bpfcompat/v0.3.0** was hardened automatically. 19 finding(s) were identified and resolved across 3 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): The 'Run bpfcompat' step directly interpolates multiple ${{ inputs.* }} expressions inside the run: shell script, allowing an attacker-controlled caller to inject arbitrary shell commands. Offending lines include:
- Line 165: artifact_path="$(resolve_path "${{ inputs.artifact }}")"
- Line 166: matrix_path="$(resolve_path "${{ inputs.matrix }}")"
- Line 172: matrix_input="${{ inputs.matrix }}"
- Line 179: out_path="$(resolve_path "${{ inputs.out }}")"
- Line 180: manifest_path="$(resolve_path "${{ inputs.manifest }}")"
- Line 181: markdown_path="$(resolve_path "${{ inputs.markdown }}")"
- Line 182: workdir_path="$(resolve_path "${{ inputs.workdir }}")"
- Line 183: suite_path="$(resolve_path "${{ inputs.suite }}")"
- Line 184: suite_out_path="$(resolve_path "${{ inputs.suite-out }}")"
- Line 185: suite_markdown_path="$(resolve_path "${{ inputs.suite-markdown }}")"
- Line 187: binary_input="${{ inputs.binary }}"
- (suite cmd) --timeout "${{ inputs.timeout }}" and --concurrency "${{ inputs.concurrency }}"
- (test cmd) --timeout "${{ inputs.timeout }}" and --concurrency "${{ inputs.concurrency }}"
- if [[ -n "${{ inputs.validation-mode }}" ]] and --validation-mode "${{ inputs.validation-mode }}"
- if [[ "${{ inputs.keep-vm-on-failure }}" == "true" ]]
All of these are YAML-template-substituted before the shell parses the script, enabling injection. The fix is to route all these inputs through env: variables (as was correctly done for inputs.command, inputs.command-binary, and inputs.command-expect-exit) and reference them as quoted shell variables.

Locations:

- `action.yml:165`
- `action.yml:166`
- `action.yml:172`
- `action.yml:179`
- `action.yml:180`
- `action.yml:181`
- `action.yml:182`
- `action.yml:183`
- `action.yml:184`
- `action.yml:185`
- `action.yml:187`

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

Added 15 new env: mappings to the 'Run bpfcompat' step (INPUT_ARTIFACT, INPUT_MATRIX, INPUT_OUT, INPUT_MANIFEST, INPUT_MARKDOWN, INPUT_WORKDIR, INPUT_SUITE, INPUT_SUITE_OUT, INPUT_SUITE_MARKDOWN, INPUT_BINARY, INPUT_TIMEOUT, INPUT_CONCURRENCY, INPUT_VALIDATION_MODE, INPUT_KEEP_VM_ON_FAILURE) and replaced all corresponding ${{ inputs.* }} inline interpolations in the run: block with their safe shell variable equivalents. The fix covers all 19 reported injection locations across lines 165-319.

### Iteration 2

**Fixes applied:** script-injection

**Notes:**

Fixed three script injection vulnerabilities in .github/workflows/multiarch-compatibility.yml:
1. Line 52 (Verify runner virtualization): Moved `${{ matrix.qemu_bin }}` to `env: QEMU_BIN` and replaced both occurrences in the shell script with `$QEMU_BIN`.
2. Line 60 (Build validator step): Moved `${{ matrix.examples_target }}` to `env: EXAMPLES_TARGET` and replaced with `$EXAMPLES_TARGET` in the shell script.
3. Line 66 (Fetch architecture VM image step): Moved `${{ matrix.image_target }}` to `env: IMAGE_TARGET` and replaced with `$IMAGE_TARGET` in the shell script.
All matrix values are now passed through environment variables rather than being directly interpolated into shell command strings.

### Iteration 3

**Fixes applied:** github-env-injection

**Notes:**

In the 'Resolve build metadata' step of .github/workflows/release-artifacts.yml, added a sanitization step for the VERSION variable before writing it to $GITHUB_OUTPUT. The VERSION is derived from github.ref (via the REF env var), so it could contain attacker-controlled newlines. The fix introduces `SAFE_VERSION="$(printf '%s' "$VERSION" | tr -d '\n\r')"` and uses `SAFE_VERSION` instead of `VERSION` in the echo to $GITHUB_OUTPUT, preventing newline injection attacks that could poison downstream step outputs.

