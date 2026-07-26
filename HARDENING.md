<!-- markdownlint-disable -->

# Hardening Report: Kernel-Guard--bpfcompat/v0.3.5

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Kernel-Guard--bpfcompat/v0.3.5** was hardened automatically. 22 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### script-injection (severity: high)

The 'Run bpfcompat' composite action step directly interpolates multiple ${{ inputs.* }} expressions inside the run: shell script (rule a). This allows an attacker-controlled calling workflow to inject arbitrary shell commands. Affected inputs interpolated directly in the run: block include: inputs.artifact (e.g. `artifact_input="${{ inputs.artifact }}"`), inputs.matrix (e.g. `matrix_path="$(resolve_path "${{ inputs.matrix }}")"`), inputs.matrix again as `matrix_input="${{ inputs.matrix }}"`, inputs.out, inputs.manifest, inputs.markdown, inputs.workdir, inputs.suite, inputs.suite-out, inputs.suite-markdown, inputs.binary, inputs.timeout (e.g. `--timeout "${{ inputs.timeout }}"`), inputs.concurrency, inputs.disk-resize (e.g. `if [[ -n "${{ inputs.disk-resize }}" ...`), inputs.validation-mode, and inputs.keep-vm-on-failure. The action correctly routes inputs.command, inputs.command-binary, and inputs.command-expect-exit through env: variables, but all other inputs are unsafely interpolated directly into the shell script text before the shell parses it.

Locations:

- `action.yml:163`
- `action.yml:170`
- `action.yml:173`
- `action.yml:185`
- `action.yml:186`
- `action.yml:187`
- `action.yml:188`
- `action.yml:189`
- `action.yml:190`
- `action.yml:191`
- `action.yml:192`
- `action.yml:218`
- `action.yml:220`
- `action.yml:268`
- `action.yml:270`
- `action.yml:285`
- `action.yml:287`
- `action.yml:289`
- `action.yml:292`
- `action.yml:294`
- `action.yml:298`

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

**Fixes applied:** script-injection, static-inline-injection

**Notes:**

Moved all ${{ inputs.* }} expressions that were directly interpolated in the run: shell script of the 'Run bpfcompat' step into the step's env: block. Added 15 new environment variable mappings: INPUT_ARTIFACT, INPUT_MATRIX, INPUT_OUT, INPUT_MANIFEST, INPUT_MARKDOWN, INPUT_WORKDIR, INPUT_SUITE, INPUT_SUITE_OUT, INPUT_SUITE_MARKDOWN, INPUT_BINARY, INPUT_TIMEOUT, INPUT_CONCURRENCY, INPUT_DISK_RESIZE, INPUT_VALIDATION_MODE, and INPUT_KEEP_VM_ON_FAILURE. All references in the run: block were updated to use the corresponding $INPUT_* environment variables. The remaining ${{ inputs.* }} expressions in the file are only in if: conditions (inputs.prebuilt, inputs.build) and in the env: block itself, which are safe and not subject to shell injection.

### Iteration 2

**Fixes applied:** script-injection, unpinned-uses, github-env-injection

**Notes:**

1. script-injection (multiarch-compatibility.yml lines 52, 60, 66): Moved ${{ matrix.qemu_bin }}, ${{ matrix.examples_target }}, and ${{ matrix.image_target }} out of run: blocks into step-level env: blocks as QEMU_BIN, EXAMPLES_TARGET, and IMAGE_TARGET respectively. Shell scripts now reference plain $VAR names. 2. unpinned-uses (consumer-canary.yml line 130): Pinned `Kernel-Guard/bpfcompat@main` to the resolved full commit SHA `02615c5c97ba88d39c4fde6d4b815879241ffabe` with a `# main` comment for readability. 3. github-env-injection (release-artifacts.yml line 70): Added sanitization of VERSION, COMMIT, and BUILD_DATE values using `printf '%s' "$VAR" | tr -d '\n\r'` before writing to $GITHUB_OUTPUT, preventing newline injection of additional key=value pairs.

