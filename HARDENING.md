<!-- markdownlint-disable -->

# Hardening Report: Kernel-Guard--bpfcompat/v0.1.3

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Kernel-Guard--bpfcompat/v0.1.3** was hardened automatically. 19 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): Multiple `${{ inputs.* }}` expressions are interpolated directly inside `run:` shell commands in the 'Run bpfcompat' step of action.yml. Affected lines include: `artifact_path="$(resolve_path "${{ inputs.artifact }}")"` (line 102), `matrix_path="$(resolve_path "${{ inputs.matrix }}")"` (line 103), `out_path="$(resolve_path "${{ inputs.out }}")"` (line 104), `manifest_path="$(resolve_path "${{ inputs.manifest }}")"` (line 105), `markdown_path="$(resolve_path "${{ inputs.markdown }}")"` (line 106), `workdir_path="$(resolve_path "${{ inputs.workdir }}")"` (line 107), `suite_path="$(resolve_path "${{ inputs.suite }}")"` (line 108), `suite_out_path="$(resolve_path "${{ inputs.suite-out }}")"` (line 109), `suite_markdown_path="$(resolve_path "${{ inputs.suite-markdown }}")"` (line 110), `binary_input="${{ inputs.binary }}"` (line 112), `--timeout "${{ inputs.timeout }}"` (line 145), `--concurrency "${{ inputs.concurrency }}"` (line 147), and `if [[ "${{ inputs.keep-vm-on-failure }}" == "true" ]]`. Any caller-supplied input value is substituted into the shell script before the shell parses it, enabling command injection.

Locations:

- `action.yml:102`
- `action.yml:103`
- `action.yml:104`
- `action.yml:105`
- `action.yml:106`
- `action.yml:107`
- `action.yml:108`
- `action.yml:109`
- `action.yml:110`
- `action.yml:112`
- `action.yml:145`
- `action.yml:147`

### script-injection (severity: high)

Sub-rule (a): `${{ matrix.* }}` expressions are interpolated directly inside `run:` shell commands in multiarch-compatibility.yml. Offending lines: `command -v "${{ matrix.qemu_bin }}"` (line 52) in the 'Verify runner virtualization' step; `make "${{ matrix.examples_target }}"` (line 60) in the 'Build validator' step; `make "${{ matrix.image_target }}"` (line 66) in the 'Fetch architecture VM image' step. Matrix values flow through YAML template substitution before the shell parses them, enabling injection of shell metacharacters.

Locations:

- `.github/workflows/multiarch-compatibility.yml:52`
- `.github/workflows/multiarch-compatibility.yml:60`
- `.github/workflows/multiarch-compatibility.yml:66`

### github-env-injection (severity: high)

In release-artifacts.yml, the 'Resolve build metadata' step sets `REF: ${{ github.ref }}` in its `env:` block, then derives `VERSION` from `$REF` via shell parameter expansion (`VERSION="${REF#refs/tags/}"`) and writes it to `$GITHUB_OUTPUT` with `echo "version=$VERSION" >> "$GITHUB_OUTPUT"` (line 66) without the required sanitization step (`printf '%s' "$VERSION" | tr -d '\n\r'`). This is a case (c) violation: an indirect write of a github context value to a special environment file without sanitization. A tag or branch name containing newline characters could inject additional key=value pairs into GITHUB_OUTPUT.

Locations:

- `.github/workflows/release-artifacts.yml:66`

### unpinned-uses (severity: high)

All `uses:` references across every workflow file are pinned to mutable version tags (e.g. @v4, @v5, @v6, @v0, @v3) rather than immutable 40-character commit SHAs. This exposes the workflows to supply-chain attacks if any referenced action's tag is moved or the action repository is compromised. Affected actions include: actions/checkout@v4, actions/setup-go@v5, actions/upload-artifact@v4, golangci/golangci-lint-action@v6, anchore/sbom-action@v0, sigstore/cosign-installer@v3, actions/configure-pages@v5, actions/upload-pages-artifact@v3, actions/deploy-pages@v4.

Locations:

- `.github/workflows/arm64-build-smoke.yml:27`
- `.github/workflows/bpfcompat-example.yml:28`
- `.github/workflows/bpfcompat-example.yml:64`
- `.github/workflows/ci.yml:46`
- `.github/workflows/ci.yml:48`
- `.github/workflows/ci.yml:57`
- `.github/workflows/ci.yml:64`
- `.github/workflows/ci.yml:66`
- `.github/workflows/ci.yml:73`
- `.github/workflows/compatibility-matrix-publish.yml:28`
- `.github/workflows/compatibility-matrix-publish.yml:64`
- `.github/workflows/compatibility-matrix-publish.yml:87`
- `.github/workflows/compatibility-matrix-publish.yml:91`
- `.github/workflows/compatibility-matrix-publish.yml:95`
- `.github/workflows/firecracker-preflight.yml:16`
- `.github/workflows/latest-kernel-compatibility.yml:37`
- `.github/workflows/latest-kernel-compatibility.yml:82`
- `.github/workflows/multiarch-compatibility.yml:45`
- `.github/workflows/multiarch-compatibility.yml:73`
- `.github/workflows/profile-catalog-maintenance.yml:20`
- `.github/workflows/profile-catalog-maintenance.yml:21`
- `.github/workflows/profile-catalog-maintenance.yml:36`
- `.github/workflows/release-artifacts.yml:45`
- `.github/workflows/release-artifacts.yml:48`
- `.github/workflows/release-artifacts.yml:80`
- `.github/workflows/release-artifacts.yml:84`
- `.github/workflows/release-artifacts.yml:91`
- `.github/workflows/upstream-kernel-compatibility.yml:27`
- `.github/workflows/upstream-kernel-compatibility.yml:56`

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

**Fixes applied:** script-injection, static-inline-injection, github-env-injection, unpinned-uses

**Notes:**

Fixed all findings:

1. script-injection/static-inline-injection (action.yml): Moved all 13 ${{ inputs.* }} expressions (artifact, matrix, out, manifest, markdown, workdir, suite, suite-out, suite-markdown, binary, timeout, concurrency, keep-vm-on-failure) from the run: shell script to an env: block on the 'Run bpfcompat' step. Shell script now uses plain env vars ($INPUT_ARTIFACT, etc.).

2. script-injection (multiarch-compatibility.yml): Moved ${{ matrix.qemu_bin }}, ${{ matrix.examples_target }}, and ${{ matrix.image_target }} from run: blocks to env: blocks on their respective steps.

3. github-env-injection (release-artifacts.yml): Added sanitization with `printf '%s' "$VAR" | tr -d '\n\r'` for VERSION, COMMIT, and BUILD_DATE before writing to $GITHUB_OUTPUT.

4. unpinned-uses: Pinned all action references to full 40-char commit SHAs across all 9 workflow files: actions/checkout@v4, actions/setup-go@v5, actions/upload-artifact@v4, golangci/golangci-lint-action@v6, anchore/sbom-action@v0, sigstore/cosign-installer@v3, actions/configure-pages@v5, actions/upload-pages-artifact@v3, actions/deploy-pages@v4.

