<!-- markdownlint-disable -->

# Hardening Report: Kernel-Guard--bpfcompat/v0.1.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Kernel-Guard--bpfcompat/v0.1.2** was hardened automatically. 16 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### script-injection (severity: high)

The 'Run bpfcompat' step in action.yml directly interpolates multiple `${{ inputs.* }}` expressions inside the `run:` shell script body (sub-rule a). GitHub Actions substitutes these expressions into the shell script before the shell executes it, so a calling workflow that passes attacker-controlled values can inject arbitrary shell metacharacters and commands. Offending lines include:
- `artifact_path="$(resolve_path "${{ inputs.artifact }}")"` 
- `matrix_path="$(resolve_path "${{ inputs.matrix }}")"` 
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
- `if [[ "${{ inputs.keep-vm-on-failure }}" == "true" ]]`
Fix: move all inputs into `env:` variables and reference them as quoted shell variables (e.g., `"$INPUT_ARTIFACT"`) inside the `run:` block.

Locations:

- `action.yml:95`
- `action.yml:96`
- `action.yml:97`
- `action.yml:98`
- `action.yml:99`
- `action.yml:100`
- `action.yml:101`
- `action.yml:102`
- `action.yml:103`
- `action.yml:105`
- `action.yml:116`
- `action.yml:118`
- `action.yml:158`
- `action.yml:160`
- `action.yml:165`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.artifact }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:102`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.matrix }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:103`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.out }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:104`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.manifest }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:105`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.markdown }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:106`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.workdir }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:107`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.suite }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:108`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.suite-out }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:109`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.suite-markdown }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:110`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.binary }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:112`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.timeout }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:145`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.concurrency }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:147`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.timeout }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:210`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.concurrency }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:212`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.keep-vm-on-failure }}" appears directly in run: block of step "Run bpfcompat"; move to env: map

Locations:

- `action.yml:221`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, static-inline-injection

**Notes:**

Moved all 13 `${{ inputs.* }}` expressions out of the `run:` shell script body in the 'Run bpfcompat' step and into a new `env:` block on that step. The inputs mapped are: INPUT_ARTIFACT, INPUT_MATRIX, INPUT_OUT, INPUT_MANIFEST, INPUT_MARKDOWN, INPUT_WORKDIR, INPUT_SUITE, INPUT_SUITE_OUT, INPUT_SUITE_MARKDOWN, INPUT_BINARY, INPUT_TIMEOUT, INPUT_CONCURRENCY, INPUT_KEEP_VM_ON_FAILURE. All references in the shell script were updated to use the corresponding environment variable names (e.g., `"$INPUT_TIMEOUT"`, `"$INPUT_CONCURRENCY"`, `"$INPUT_KEEP_VM_ON_FAILURE"`). No other files required changes.

### Iteration 2

**Fixes applied:** script-injection, github-env-injection, unpinned-uses

**Notes:**

Fixed all three findings across 10 workflow files:

1. script-injection (multiarch-compatibility.yml lines 52, 60, 66): Moved ${{ matrix.qemu_bin }}, ${{ matrix.examples_target }}, and ${{ matrix.image_target }} out of run: shell blocks into step-level env: variables (QEMU_BIN, EXAMPLES_TARGET, IMAGE_TARGET) and referenced them as "$VAR" in the shell.

2. github-env-injection (release-artifacts.yml line 66): Added printf '%s' "$VAR" | tr -d '\n\r' sanitization for VERSION, COMMIT, and BUILD_DATE before writing to $GITHUB_OUTPUT to prevent newline injection attacks.

3. unpinned-uses: Pinned all 9 action references to full 40-character SHA digests across all 10 workflow files: actions/checkout@v4→34e114876b0b11c390a56381ad16ebd13914f8d5, actions/setup-go@v5→40f1582b2485089dde7abd97c1529aa768e1baff, actions/upload-artifact@v4→ea165f8d65b6e75b540449e92b4886f43607fa02, golangci/golangci-lint-action@v6→55c2c1448f86e01eaae002a5a3a9624417608d84, anchore/sbom-action@v0→e22c389904149dbc22b58101806040fa8d37a610, sigstore/cosign-installer@v3→398d4b0eeef1380460a10c8013a76f728fb906ac, actions/configure-pages@v5→983d7736d9b0ae728b81ab479565c72886d7745b, actions/upload-pages-artifact@v3→56afc609e74202658d3ffba0e8f6dda462b719fa, actions/deploy-pages@v4→d6db90164ac5ed86f2b6aed7e0febac5b3c0c03e.

