<!-- markdownlint-disable -->

# Hardening Report: Kernel-Guard--bpfcompat/v0.3.4

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Kernel-Guard--bpfcompat/v0.3.4** was hardened automatically. 24 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): The 'Run bpfcompat' step's run: block in action.yml directly interpolates multiple ${{ inputs.* }} expressions into shell commands without routing them through env: variables first. Affected expressions include: ${{ inputs.artifact }}, ${{ inputs.matrix }} (used twice), ${{ inputs.out }}, ${{ inputs.manifest }}, ${{ inputs.markdown }}, ${{ inputs.workdir }}, ${{ inputs.suite }}, ${{ inputs.suite-out }}, ${{ inputs.suite-markdown }}, ${{ inputs.binary }}, ${{ inputs.timeout }} (used twice), ${{ inputs.concurrency }} (used twice), ${{ inputs.disk-resize }} (used twice), ${{ inputs.validation-mode }} (used twice), and ${{ inputs.keep-vm-on-failure }}. Any of these inputs could contain shell metacharacters that are interpreted before the shell ever sees the value, enabling command injection by a caller workflow.

Locations:

- `action.yml:145`
- `action.yml:148`
- `action.yml:155`
- `action.yml:162`
- `action.yml:163`
- `action.yml:164`
- `action.yml:165`
- `action.yml:166`
- `action.yml:167`
- `action.yml:168`
- `action.yml:169`

### script-injection (severity: high)

Sub-rule (a): The 'Verify runner virtualization' and 'Build validator and architecture-specific BPF fixtures' and 'Fetch architecture VM image' steps in multiarch-compatibility.yml directly interpolate ${{ matrix.qemu_bin }}, ${{ matrix.examples_target }}, and ${{ matrix.image_target }} into run: shell commands. Matrix context values flow through YAML template substitution before the shell sees them, enabling injection of shell metacharacters. Offending lines: `command -v "${{ matrix.qemu_bin }}"`, `make "${{ matrix.examples_target }}"`, `make "${{ matrix.image_target }}"`

Locations:

- `.github/workflows/multiarch-compatibility.yml:47`
- `.github/workflows/multiarch-compatibility.yml:54`
- `.github/workflows/multiarch-compatibility.yml:60`

### unpinned-uses (severity: high)

The workflow consumer-canary.yml references `uses: Kernel-Guard/bpfcompat@main`, which is pinned to a mutable branch name rather than a full 40-character commit SHA. If the referenced repository is compromised or the branch is force-pushed, the workflow will silently execute attacker-controlled code. All other uses: references in the repository are correctly SHA-pinned.

Locations:

- `.github/workflows/consumer-canary.yml:148`

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

Fixed all findings across three files:

1. action.yml: Moved all ${{ inputs.* }} expressions from the run: block into the env: block of the 'Run bpfcompat' step. Added 16 env variables (INPUT_ARTIFACT, INPUT_MATRIX, INPUT_OUT, INPUT_MANIFEST, INPUT_MARKDOWN, INPUT_WORKDIR, INPUT_SUITE, INPUT_SUITE_OUT, INPUT_SUITE_MARKDOWN, INPUT_BINARY, INPUT_TIMEOUT, INPUT_CONCURRENCY, INPUT_DISK_RESIZE, INPUT_VALIDATION_MODE, INPUT_KEEP_VM_ON_FAILURE, plus the 3 already present). All shell script references now use safe $INPUT_* env var form.

2. .github/workflows/multiarch-compatibility.yml: Fixed three steps ('Verify runner virtualization', 'Build validator and architecture-specific BPF fixtures', 'Fetch architecture VM image') by adding env: blocks with QEMU_BIN, EXAMPLES_TARGET, and IMAGE_TARGET variables respectively, replacing inline ${{ matrix.* }} expressions.

3. .github/workflows/consumer-canary.yml: Pinned `uses: Kernel-Guard/bpfcompat@main` to the full commit SHA `59ea07d2216113e86c0c9a5514be4c5c2a21d90c` with a `# main` comment.

### Iteration 2

**Fixes applied:** github-env-injection

**Notes:**

In the 'Resolve build metadata' step of release-artifacts.yml, added sanitization of the VERSION variable (derived from github.ref via $REF) before writing it to $GITHUB_OUTPUT. The fix uses `SAFE_VERSION="$(printf '%s' "$VERSION" | tr -d '\n\r')"` to strip embedded newlines/carriage returns, then writes `version=$SAFE_VERSION` to $GITHUB_OUTPUT instead of the raw `version=$VERSION`. This prevents newline injection attacks that could overwrite subsequent step outputs. COMMIT and BUILD_DATE are locally computed values and remain unchanged.

