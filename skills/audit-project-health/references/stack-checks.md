# Stack checks and verified follow-up

## Contents

- Safe static coverage
- Stack adapters
- Confidence rules
- Verified checks

## Safe static coverage

The bundled scanner reads bounded source and configuration text only. It checks project-root availability, malformed package metadata, declared entry points, relative JavaScript and TypeScript imports, risky Electron security settings, large source/assets, test-script presence, lockfile hygiene, debug residue, empty catch blocks, and TODO/FIXME concentration. Generated folders, dependencies, caches, binaries, secret files, and symlinks are skipped. Escaping links are refused; unresolved links remain visible in scan boundaries and findings with only a safe operating-system error code such as `ENOENT`.

Workspace bookkeeping folders are excluded by exact directory name before files enter any downstream check: `.proof`, `.proof-pipeline`, `.backup-pipeline`, `.backup-install`, and `.trash`. Every encountered folder remains visible as a non-actionable scan-boundary notice with its relative path and artifact classification; the scanner neither walks its contents nor charges them against the normal evidence budget.

Relative-import checks use the bounded file index first, then exact in-root filesystem metadata when a valid target may fall beyond a file, byte, or time bound. The fallback reads no target content, preserves filename-case checks on case-insensitive filesystems, and refuses symlink targets or parent paths that escape the explicit project root. A missing target remains a Confirmed finding even when the wider scan is incomplete. Nested `.backup-install` dependency caches are skipped so temporary package trees do not consume the project budget.

Files above 1 MiB remain actionable unless their role is proven by project evidence. Exact declared build icons, their matching iconset inputs, `-src` image masters with an optimized sibling explicitly listed in `build.files`, and ZIPs in an explicit `*-zips/` distribution folder with a matching unpacked source directory are disclosed as non-actionable scan-boundary notices. Names or extensions alone never earn the exemption, and the global threshold is unchanged.

## Stack adapters

- **JavaScript / TypeScript / Node**: package metadata, entry points, relative imports, test scripts, lockfiles, debug residue, empty catch blocks, and risky dynamic execution. Explicit Node CLI entrypoints may use `console.log` for their human-facing output; executable `debugger` statements remain actionable there, while ordinary application modules retain `console.log` detection.
- **React / Next.js**: JavaScript checks plus large client assets and plain image loading opportunities.
- **Electron**: JavaScript checks plus `nodeIntegration`, `contextIsolation`, `sandbox`, and `webSecurity` evidence.
- **PWA**: manifest and service-worker presence when package metadata advertises PWA tooling.
- **Generic**: README, oversized files, TODO/FIXME concentration, secret-file presence without reading values, and bounded-scan notices.

Adapters intentionally prefer a small explainable rule set over broad regex guessing. Extend the scanner only with a deterministic fixture proving the rule and its false-positive boundary.

## Confidence rules

- **Confirmed** requires direct static proof of a broken declaration or reference.
- **Likely** requires concrete risky code or configuration but still needs runtime confirmation.
- **Recommendation** describes missing safeguards or optimization opportunities and must never be called a bug.

Severity and confidence are independent. A high-severity recommendation can matter greatly without being a confirmed defect.

## Verified checks

Tests, lint, builds, audits, and profiling may create caches or artifacts even when source files stay unchanged. Show the exact command and obtain human approval first. Prefer existing scripts already declared by the project. Record the command, exit status, relevant output, duration when useful, and any generated artifacts. Do not install missing dependencies during diagnosis unless the human separately approves installation.
