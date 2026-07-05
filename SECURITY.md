# Security policy

## Scope

The FCxO Toolkit is **instruction-only**: every skill is plain markdown that Claude
reads and follows. The plugin ships no executable code, scripts, hooks, MCP servers,
or network services of its own (see [PRIVACY.md](PRIVACY.md)). The security surface is
therefore the content of the skill instructions and templates themselves - for
example, an instruction that could steer the model into unsafe behavior, leak
workspace data into a web query, or mishandle billing details in a generated
document.

## Reporting a vulnerability

If you find a security issue, report it privately - please do not open a public
issue first:

- **GitHub**: use [private vulnerability reporting](https://github.com/ainova-systems/fcxo-toolkit/security/advisories/new)
  on this repository, or
- **Email**: info@ainovasystems.com with the subject `FCxO Toolkit security`.

Include the skill or file affected, what behavior you observed or expect could be
triggered, and steps to reproduce.

## What to expect

- Acknowledgement within **5 business days**.
- Reports are investigated with reasonable care; confirmed issues are fixed in the
  repository and noted in the `CHANGELOG.md` of the release that ships the fix.
- Please give us a chance to fix a confirmed issue before public disclosure.

## Supported versions

The latest release (and the current `main` branch) receives fixes. Older tags are
not patched retroactively.
