# Security policy

How to report a vulnerability in the Plotwork command line package, and what to expect. It is for anyone who finds a security problem in the package published from this repository.

## TL;DR

| Question | Answer |
|---|---|
| Where do I report? | Use "Report a vulnerability" on the Security tab of this repository. That report is private. |
| Is there an email address? | No. The private report above is the only channel, so every report stays private until a fix is out. |
| Which versions get fixes? | The latest published version. |
| Should I open a public issue? | No. Public issues are for bugs that are not security problems. |

## What the package does

The package is a command line that checks, draws, and syncs diagrams in a repository. It needs no API key and counts nothing. It reads the files you name and writes only the pictures and the Markdown blocks a command says it writes. The one network request it can make is for a hosted picture link you give to `read`; a view link is read where you run it, with no request. A report is most useful when it shows one of the following:

- the package reaching the network, or another process, in a way the README does not describe
- the package reading or writing a file outside the paths a command names
- a diagram, a Markdown file, or an argument that makes the package misbehave or exhaust memory
- a secret or a path of a build machine inside the published files
- a mismatch between a published version and its `checksums.txt`

## What to include

Give the package version (the output of running the package with `--version`), your Node version, the smallest input that shows the problem, and what you expected instead.

## What happens next

The maintainers acknowledge a report, confirm the problem, and publish a fixed version. A published version is never changed or removed. A fix is a new version, and an affected version is deprecated on npm with a pointer to the fix.

## See also

| Link | Why |
|---|---|
| [README](./README.md) | What the package is and how to install it |
| [Documentation](https://docs.plotwork.dev) | The commands and the diagram language |
