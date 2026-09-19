# Maintenance Checklist

This repository is a version-sensitive setup guide. Commands that worked for one Windows/WSL/Ubuntu release can become stale, architecture-specific, or unsafe to copy unchanged.

Use this checklist before presenting the guide as a current recommended setup.

## Version-sensitive review

For each major section, record or verify:

- Windows/WSL version assumptions;
- Ubuntu release(s) supported;
- CPU architecture assumptions (x86_64 / arm64);
- Node.js version strategy;
- package/repository URLs;
- commands copied from third-party install pages;
- last verified date.

## Separate requirements from preferences

Mark steps as one of:

- **Required** — needed for the documented environment to work.
- **Recommended** — useful default with trade-offs.
- **Personal preference** — shell, font, editor, convenience tools, aliases, etc.

This prevents a reader from confusing one developer's workflow with a platform requirement.

## Safety review

Pay special attention to:

- passwordless sudo;
- shell profile modification;
- SSH key generation/permissions;
- commands that download and execute remote content;
- binaries downloaded by architecture-specific URL;
- system-wide symlinks or file replacement.

For each high-impact command, add:

1. what it changes;
2. how to verify success;
3. how to undo/recover.

## Verification pattern

After each major stage, prefer a short verification block, for example:

```sh
wsl --status
wsl --list --verbose
```

Inside Ubuntu:

```sh
cat /etc/os-release
uname -m
node --version
command -v jq
command -v yq
```

Verification should check the result of the step rather than assume the command succeeded.

## Maintenance decision

Before making large local edits, decide whether this fork is:

- an upstream-tracking copy;
- a deliberately customized Windows/WSL setup;
- or a historical reference.

If it becomes a customized guide, document its supported versions and local differences explicitly.
