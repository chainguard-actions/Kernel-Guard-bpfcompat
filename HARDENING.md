<!-- markdownlint-disable -->

# Hardening Report: Kernel-Guard--bpfcompat/v0.3.8-rc.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Kernel-Guard--bpfcompat/v0.3.8-rc.2** was hardened automatically. 22 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): The 'Run bpfcompat' step directly interpolates multiple ${{ inputs.* }} expressions inside the run: shell script body. The following inputs are interpolated directly into shell code without being routed through env: variables first: inputs.artifact (artifact_input="${{ inputs.artifact }}"), inputs.matrix (matrix_path and matrix_input assignments), inputs.out, inputs.manifest, inputs.markdown, inputs.workdir, inputs.suite, inputs.suite-out, inputs.suite-markdown, inputs.binary, inputs.timeout, inputs.concurrency, inputs.disk-resize, inputs.validation-mode, and inputs.keep-vm-on-failure. Because YAML template substitution occurs before the shell parses the script, an attacker-controlled value in any of these inputs can inject arbitrary shell syntax (e.g. semicolons, backticks, $(...) subshells). The action author correctly routed inputs.command, inputs.command-binary, and inputs.command-expect-exit through env: variables, but left all path, mode, and timing inputs as direct inline interpolations.

Locations:

- `action.yml:200`
- `action.yml:207`
- `action.yml:211`
- `action.yml:219`
- `action.yml:222`
- `action.yml:223`
- `action.yml:224`
- `action.yml:225`
- `action.yml:226`
- `action.yml:227`
- `action.yml:228`
- `action.yml:247`
- `action.yml:249`
- `action.yml:290`
- `action.yml:292`
- `action.yml:307`
- `action.yml:308`
- `action.yml:310`
- `action.yml:311`
- `action.yml:313`
- `action.yml:316`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.artifact }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:268`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.matrix }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:276`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.matrix }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:283`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.out }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:290`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.manifest }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:291`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.markdown }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:292`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.workdir }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:293`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.suite }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:294`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.suite-out }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:295`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.suite-markdown }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:296`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.binary }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:298`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.timeout }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:331`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.concurrency }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:333`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.timeout }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:403`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.concurrency }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:405`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.disk-resize }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:423`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.disk-resize }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:423`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.disk-resize }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:424`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.validation-mode }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:426`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.validation-mode }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:427`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.keep-vm-on-failure }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:432`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, static-inline-injection

**Notes:**

Moved all ${{ inputs.* }} expressions from the 'Run bpfcompat' step's run: shell script body into the step's env: block. Added 15 new env vars: INPUT_ARTIFACT, INPUT_MATRIX, INPUT_OUT, INPUT_MANIFEST, INPUT_MARKDOWN, INPUT_WORKDIR, INPUT_SUITE, INPUT_SUITE_OUT, INPUT_SUITE_MARKDOWN, INPUT_BINARY, INPUT_TIMEOUT, INPUT_CONCURRENCY, INPUT_DISK_RESIZE, INPUT_VALIDATION_MODE, INPUT_KEEP_VM_ON_FAILURE. All inline ${{ inputs.artifact }}, ${{ inputs.matrix }}, ${{ inputs.out }}, ${{ inputs.manifest }}, ${{ inputs.markdown }}, ${{ inputs.workdir }}, ${{ inputs.suite }}, ${{ inputs.suite-out }}, ${{ inputs.suite-markdown }}, ${{ inputs.binary }}, ${{ inputs.timeout }}, ${{ inputs.concurrency }}, ${{ inputs.disk-resize }}, ${{ inputs.validation-mode }}, and ${{ inputs.keep-vm-on-failure }} references in the run: block were replaced with their corresponding $INPUT_* environment variable references.

