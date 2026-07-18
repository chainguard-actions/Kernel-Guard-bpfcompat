<!-- markdownlint-disable -->

# Hardening Report: Kernel-Guard--bpfcompat/v0.1.5

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Kernel-Guard--bpfcompat/v0.1.5** was hardened automatically. 20 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): The 'Run bpfcompat' step in action.yml directly interpolates multiple `${{ inputs.* }}` expressions inside the `run:` shell block without routing them through env vars first. Affected inputs include: inputs.artifact, inputs.matrix, inputs.out, inputs.manifest, inputs.markdown, inputs.workdir, inputs.suite, inputs.suite-out, inputs.suite-markdown, inputs.binary, inputs.timeout, inputs.concurrency, inputs.validation-mode, and inputs.keep-vm-on-failure. Any of these inputs could contain shell metacharacters (e.g. semicolons, backticks, command substitution) that would be executed by the shell before quoting takes effect. Example offending lines: `artifact_path="$(resolve_path "${{ inputs.artifact }}")"`, `binary_input="${{ inputs.binary }}"`, `--timeout "${{ inputs.timeout }}"`, `if [[ -n "${{ inputs.validation-mode }}" ]]`, `if [[ "${{ inputs.keep-vm-on-failure }}" == "true" ]]`.

Locations:

- `action.yml:110`
- `action.yml:120`
- `action.yml:121`
- `action.yml:122`
- `action.yml:123`
- `action.yml:124`
- `action.yml:125`
- `action.yml:126`
- `action.yml:127`
- `action.yml:129`

### script-injection (severity: high)

Sub-rule (a): Three `run:` blocks in multiarch-compatibility.yml directly interpolate `${{ matrix.* }}` expressions inside shell commands. Offending lines: (1) `command -v "${{ matrix.qemu_bin }}"` in the 'Verify runner virtualization' step; (2) `make "${{ matrix.examples_target }}"` in the 'Build validator' step; (3) `make "${{ matrix.image_target }}"` in the 'Fetch architecture VM image' step. Although matrix values are defined in the same file, any `${{ ... }}` directly inside a `run:` script is a script-injection finding per the check rules.

Locations:

- `.github/workflows/multiarch-compatibility.yml:43`
- `.github/workflows/multiarch-compatibility.yml:50`
- `.github/workflows/multiarch-compatibility.yml:55`

### unpinned-uses (severity: high)

All `uses:` references across all workflow files are pinned to mutable version tags (e.g. @v4, @v5, @v6, @v0, @v2, @v3) rather than immutable 40-character commit SHAs. This exposes the workflows to supply-chain attacks if any of these action repositories are compromised and the tag is moved. Affected actions include: actions/checkout@v4, actions/setup-go@v5, actions/upload-artifact@v4, golangci/golangci-lint-action@v6, anchore/sbom-action@v0, sigstore/cosign-installer@v3, softprops/action-gh-release@v2, actions/configure-pages@v5, actions/upload-pages-artifact@v3, actions/deploy-pages@v4.

Locations:

- `.github/workflows/arm64-build-smoke.yml:30`
- `.github/workflows/bpfcompat-example.yml:27`
- `.github/workflows/bpfcompat-example.yml:62`
- `.github/workflows/ci.yml:44`
- `.github/workflows/ci.yml:46`
- `.github/workflows/ci.yml:57`
- `.github/workflows/ci.yml:71`
- `.github/workflows/ci.yml:73`
- `.github/workflows/ci.yml:83`
- `.github/workflows/ci.yml:91`
- `.github/workflows/ci.yml:100`
- `.github/workflows/ci.yml:102`
- `.github/workflows/ci.yml:111`
- `.github/workflows/ci.yml:113`
- `.github/workflows/compatibility-matrix-publish.yml:31`
- `.github/workflows/compatibility-matrix-publish.yml:57`
- `.github/workflows/compatibility-matrix-publish.yml:89`
- `.github/workflows/compatibility-matrix-publish.yml:95`
- `.github/workflows/compatibility-matrix-publish.yml:100`
- `.github/workflows/firecracker-preflight.yml:14`
- `.github/workflows/latest-kernel-compatibility.yml:43`
- `.github/workflows/latest-kernel-compatibility.yml:79`
- `.github/workflows/multiarch-compatibility.yml:38`
- `.github/workflows/multiarch-compatibility.yml:70`
- `.github/workflows/profile-catalog-maintenance.yml:18`
- `.github/workflows/profile-catalog-maintenance.yml:19`
- `.github/workflows/profile-catalog-maintenance.yml:35`
- `.github/workflows/release-artifacts.yml:40`
- `.github/workflows/release-artifacts.yml:42`
- `.github/workflows/release-artifacts.yml:75`
- `.github/workflows/release-artifacts.yml:80`
- `.github/workflows/release-artifacts.yml:97`
- `.github/workflows/release-artifacts.yml:109`
- `.github/workflows/upstream-kernel-compatibility.yml:22`
- `.github/workflows/upstream-kernel-compatibility.yml:55`

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

**Fixes applied:** script-injection, static-inline-injection, unpinned-uses

**Notes:**

Fixed all findings:

1. script-injection / static-inline-injection in action.yml: Moved all 14 ${{ inputs.* }} expressions (artifact, matrix, out, manifest, markdown, workdir, suite, suite-out, suite-markdown, binary, timeout, concurrency, validation-mode, keep-vm-on-failure) from the run: shell block into an env: block on the 'Run bpfcompat' step. Shell script now references them as plain env vars (INPUT_ARTIFACT, INPUT_MATRIX, etc.).

2. script-injection in multiarch-compatibility.yml: Moved ${{ matrix.qemu_bin }}, ${{ matrix.examples_target }}, and ${{ matrix.image_target }} from run: blocks into env: blocks on their respective steps.

3. unpinned-uses: Pinned all uses: references in .github/workflows/ to full 40-char commit SHAs: actions/checkout@v4→34e114876b0b11c390a56381ad16ebd13914f8d5, actions/setup-go@v5→40f1582b2485089dde7abd97c1529aa768e1baff, actions/upload-artifact@v4→ea165f8d65b6e75b540449e92b4886f43607fa02, golangci/golangci-lint-action@v6→55c2c1448f86e01eaae002a5a3a9624417608d84, anchore/sbom-action@v0→e22c389904149dbc22b58101806040fa8d37a610, sigstore/cosign-installer@v3→398d4b0eeef1380460a10c8013a76f728fb906ac, softprops/action-gh-release@v2→3bb12739c298aeb8a4eeaf626c5b8d85266b0e65, actions/configure-pages@v5→983d7736d9b0ae728b81ab479565c72886d7745b, actions/upload-pages-artifact@v3→56afc609e74202658d3ffba0e8f6dda462b719fa, actions/deploy-pages@v4→d6db90164ac5ed86f2b6aed7e0febac5b3c0c03e. Several files were rewritten entirely to fix corruption from partial edits.

### Iteration 2

**Fixes applied:** github-env-injection

**Notes:**

In the 'Resolve build metadata' step of .github/workflows/release-artifacts.yml, added sanitization of the VERSION variable before writing it to $GITHUB_OUTPUT. Added `SAFE_VERSION="$(printf '%s' "$VERSION" | tr -d '\n\r')"` and replaced `echo "version=$VERSION"` with `echo "version=$SAFE_VERSION"`. This prevents a newline-embedded github.ref value from injecting additional key=value pairs into GITHUB_OUTPUT.

