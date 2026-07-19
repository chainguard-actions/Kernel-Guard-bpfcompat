<!-- markdownlint-disable -->

# Hardening Report: Kernel-Guard--bpfcompat/v0.1.6

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Kernel-Guard--bpfcompat/v0.1.6** was hardened automatically. 18 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### script-injection (severity: high)

The 'Run bpfcompat' step in action.yml directly interpolates multiple ${{ inputs.* }} expressions inside the run: shell script (rule a). YAML template substitution occurs before the shell processes the string, so a caller-supplied input value containing shell metacharacters (e.g. `;`, `|`, `$(...)`, backticks) would be executed by the shell. Affected interpolations include: `${{ inputs.artifact }}` (line 146), `${{ inputs.matrix }}` (line 147), `${{ inputs.out }}` (line 148), `${{ inputs.manifest }}` (line 149), `${{ inputs.markdown }}` (line 150), `${{ inputs.workdir }}` (line 151), `${{ inputs.suite }}` (line 152), `${{ inputs.suite-out }}` (line 153), `${{ inputs.suite-markdown }}` (line 154), `${{ inputs.binary }}` (line 156), `${{ inputs.timeout }}` (lines ~175 and ~233), `${{ inputs.concurrency }}` (lines ~176 and ~234), `${{ inputs.validation-mode }}` (lines ~241 and ~242), and `${{ inputs.keep-vm-on-failure }}` (line ~245). All of these inputs should be moved to the step's env: block and referenced as double-quoted shell variables (e.g. "$INPUT_ARTIFACT") instead of being interpolated directly into the script.

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

Fixed all script injection findings in the 'Run bpfcompat' step of hardened/action/action.yml. Added an env: block with 14 environment variables (INPUT_ARTIFACT, INPUT_MATRIX, INPUT_OUT, INPUT_MANIFEST, INPUT_MARKDOWN, INPUT_WORKDIR, INPUT_SUITE, INPUT_SUITE_OUT, INPUT_SUITE_MARKDOWN, INPUT_BINARY, INPUT_TIMEOUT, INPUT_CONCURRENCY, INPUT_VALIDATION_MODE, INPUT_KEEP_VM_ON_FAILURE) mapping all ${{ inputs.* }} expressions. Replaced all inline ${{ inputs.* }} interpolations in the run: shell script with double-quoted environment variable references ($INPUT_ARTIFACT, $INPUT_MATRIX, etc.), preventing shell metacharacter injection from caller-supplied input values.

### Iteration 2

**Fixes applied:** script-injection, github-env-injection

**Notes:**

1. multiarch-compatibility.yml: Moved ${{ matrix.qemu_bin }}, ${{ matrix.examples_target }}, and ${{ matrix.image_target }} out of run: shell blocks into env: variables (QEMU_BIN, EXAMPLES_TARGET, IMAGE_TARGET respectively). Shell commands now reference these as plain $VAR environment variables, eliminating the script injection risk. 2. release-artifacts.yml: Added sanitization for VERSION, COMMIT, and BUILD_DATE before writing to $GITHUB_OUTPUT. Each value is now passed through `printf '%s' "$VAR" | tr -d '\n\r'` to strip any embedded newlines/carriage-returns that could inject additional key=value pairs into GITHUB_OUTPUT. The sanitized values (SAFE_VERSION, SAFE_COMMIT, SAFE_BUILD_DATE) are what get written to the output file.

