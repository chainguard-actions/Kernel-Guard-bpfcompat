<!-- markdownlint-disable -->

# Hardening Report: Kernel-Guard--bpfcompat/v0.3.3

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Kernel-Guard--bpfcompat/v0.3.3** was hardened automatically. 19 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### script-injection (severity: high)

The 'Run bpfcompat' step in action.yml directly interpolates multiple ${{ inputs.* }} expressions inside the run: shell script (rule a). An attacker-controlled calling workflow can supply values containing shell metacharacters (semicolons, backticks, $(...), etc.) that will be executed by the shell before any quoting takes effect. Affected lines include:
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
- `if [[ -n "${{ inputs.validation-mode }}" ]]` and `--validation-mode "${{ inputs.validation-mode }}"`
- `if [[ "${{ inputs.keep-vm-on-failure }}" == "true" ]]`
All of these inputs should be routed through env: variables (as the action already does for inputs.command, inputs.command-binary, and inputs.command-expect-exit) and then referenced as quoted shell variables.

Locations:

- `action.yml:1`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.artifact }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:208`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.matrix }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:216`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.matrix }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:223`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.out }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:230`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.manifest }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:231`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.markdown }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:232`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.workdir }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:233`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.suite }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:234`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.suite-out }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:235`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.suite-markdown }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:236`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.binary }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:238`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.timeout }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:271`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.concurrency }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:273`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.timeout }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:343`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.concurrency }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:345`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.validation-mode }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:363`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.validation-mode }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:364`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.keep-vm-on-failure }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:369`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, static-inline-injection

**Notes:**

Fixed all 19 script injection findings in the 'Run bpfcompat' step of action.yml. Added 14 new environment variable mappings to the step's env: block (INPUT_ARTIFACT, INPUT_MATRIX, INPUT_OUT, INPUT_MANIFEST, INPUT_MARKDOWN, INPUT_WORKDIR, INPUT_SUITE, INPUT_SUITE_OUT, INPUT_SUITE_MARKDOWN, INPUT_BINARY, INPUT_TIMEOUT, INPUT_CONCURRENCY, INPUT_VALIDATION_MODE, INPUT_KEEP_VM_ON_FAILURE). Replaced all corresponding ${{ inputs.* }} inline expressions in the run: shell script with their safe $INPUT_* environment variable references. The run: block now contains zero ${{ }} expressions — all user-controlled values flow through the env: map and are referenced as plain shell variables, preventing shell metacharacter injection.

### Iteration 2

**Fixes applied:** script-injection, github-env-injection, unpinned-uses

**Notes:**

1. multiarch-compatibility.yml: Moved three ${{ matrix.* }} interpolations out of run: blocks into env: variables (QEMU_BIN, EXAMPLES_TARGET, IMAGE_TARGET) and updated shell commands to reference those env vars. 2. release-artifacts.yml: Added sanitization of VERSION, COMMIT, and BUILD_DATE using `printf '%s' "$VAR" | tr -d '\n\r'` before writing to $GITHUB_OUTPUT to prevent newline injection. 3. consumer-canary.yml: Pinned `Kernel-Guard/bpfcompat@main` to full commit SHA `a91b25568563d322220f8fea8fa1ee26d231c8fd` with `# main` comment.

