<p align="center">
  <a href="https://plotwork.dev">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://plotwork.dev/brand/logo/plotwork-lockup-on-dark.svg">
      <img alt="Plotwork" src="https://plotwork.dev/brand/logo/plotwork-lockup-on-light.svg" width="260">
    </picture>
  </a>
</p>

# Plotwork command line

Check, draw, and sync Plotwork diagrams in your repository. A diagram is a few lines of text in a `diagram` fence of a Markdown file; `plotwork sync` draws it as one SVG that follows the reader's light or dark setting and writes the picture under the fence, so it shows on GitHub. It runs on your machine and in CI.

## TL;DR

| Question | Answer |
|---|---|
| Does it need an account or a key? | No. |
| Does it use the network? | No. It reads and writes only the files you name, and reads its own icon art from `icons/` beside the program: an icon outside the few hundred most diagrams draw is read from there when a diagram uses it. The one request it can make is for a hosted picture link you give to `read`. |
| Does it count anything? | No. It keeps no usage counts and sends none, whatever `PLOTWORK_USAGE` is set to. |
| How do I run it? | `npx @buun_group/plotwork <command>`. |
| Which Node version? | Node 22 or newer. |
| Where is every option? | `plotwork <command> --help` prints one command's options, defaults, examples, and exit codes. The [command reference](https://docs.plotwork.dev/cli/commands) has the same, one page for each command. |
| How do I keep pictures in step in CI? | `npx @buun_group/plotwork sync --check --format github "docs/**/*.md"`, or the Plotwork sync action. See [Keep diagrams in sync in CI](https://docs.plotwork.dev/diagrams-in-ci). |
| Can a diagram come from my Terraform, Bicep, Pulumi, CloudFormation, CDK, Helm, Kustomize, Backstage, Structurizr, AsyncAPI, OpenAPI, or SQL files, or my pipelines? | Yes. Name the files in the `sources` of `plotwork.config.json` and run `npx @buun_group/plotwork generate`; `generate --check` fails when the files moved and the diagram did not. See [Diagrams as code](https://docs.plotwork.dev/diagrams-as-code). |
| Will the pictures look right in GitHub's dark mode? | Yes. `sync` and `generate` write one SVG per diagram that follows the reader's light or dark setting, and remove the old `.dark.svg` files they drew. `"defaults": { "scheme": "light-and-dark" }` in `plotwork.config.json` writes a light and a dark file instead, which GitHub picks between by its own theme setting. See [Diagrams as code](https://docs.plotwork.dev/diagrams-as-code). |
| What do the pictures look like? | An arrow labelled with a verb, active or passive ("reads", "consumed by", "stored in"), draws that relationship's icon, and a sequence frame names its operator (`alt`, `loop`, `par`) in plain bold text in its corner. An `icon=` that names no icon is warning `W208` (`validate --strict` fails on it) and the item draws its default icon, so a missing brand never stops a picture. |
| Which licence? | MIT. Code and data from other people keep their own licences, listed in `THIRD-PARTY-NOTICES.md`. |

## Commands

| Command | What it does |
|---|---|
| `plotwork validate <file> [--strict]` | Checks every diagram in a Markdown or diagram file. Each mistake has a line, a column, and a hint. `--strict` also fails on warnings, but not on style suggestions (`W204`, `W205`, `W500`). A hand-written arrow labelled with a verb ("reads", "publishes") draws its icon, and a group named "VPC", "Production" or "Kafka" takes its type. |
| `plotwork render <file> --out <svg> [--scheme light\|dark\|auto] [--theme <colour theme>]` | Draws the first diagram to an SVG that carries its source. `--no-embed-source` leaves the source out. The file can be a view link from a Plotwork tool (`viewUrl`, `svgUrl`), in quotes: the diagram is read from it here, with no request. |
| `plotwork sync <files...> [--check] [--format github]` | Writes the SVGs and the picture block under every diagram fence. Each word is a file, a folder, or a quoted glob. `--check` writes nothing and fails when a picture is stale; `--format github` prints each stale fence and each mistake as a GitHub annotation on its line. |
| `plotwork generate [--check] [--capture] [--source <name>] [--since <ref>] [--config <file>] [--format github\|json\|sarif\|junit]` | Builds each diagram named in the `sources` of `plotwork.config.json` from its files (Terraform with its modules and environments, or a Terraform plan or state as JSON, Bicep, ARM, Pulumi, CloudFormation, a CDK `cdk.out`, OpenAPI, Compose, Kubernetes, Helm, Kustomize, a Backstage catalog, a Structurizr workspace, AsyncAPI documents (what each application receives drawn above it and what it sends below), SQL, Prisma, Drizzle, GitHub Actions workflows, GitLab CI/CD and Azure Pipelines pipelines, GraphQL and Protocol Buffers schemas, Serverless Framework services, Dockerfiles, nginx configuration, dbt projects, JavaScript workspaces, CSV, JSON, folders, git log), applies its overrides file, and writes the `.diagram` file or the Markdown fence and its pictures. Each imported arrow shows its relationship with an icon and a line colour (an eye for reads, a key for a grant), and every frame is typed, so it has its colour. `--check` writes nothing and fails on drift, naming each node and edge added, removed, or changed. `--capture` turns hand edits of a generated diagram into its overrides file. `--since origin/main` runs only the sources whose files changed since that git ref. `--format sarif` writes a report for code scanning and `--format junit` one for a test view, with the same exit code as `--check`. |
| `plotwork import <file> [--out <file>] [--write]` | Converts Mermaid to Plotwork: a `.mmd` file to source, or every `mermaid` fence of a Markdown file (rewritten only with `--write`). |
| `plotwork read <file.svg\|link> [--out <file>]` | Prints the source a Plotwork SVG carries, so it can be edited. |
| `plotwork export <file> --format confluence\|confluence-wiki\|notion [--image <link>]` | Prints paste-ready text for Confluence or Notion. Without `--image`, the SVG is written beside it for you to upload. |
| `plotwork outline <file>` | Prints a plain-text outline of every diagram. |
| `plotwork icons <query>` | Searches the bundled icons. |

Exit codes: 0 when all is well, 1 for problems in the diagrams, 2 for a mistake in the command line.

## License

@buun_group/plotwork is licensed under the MIT licence. The text is in the file `LICENSE`.

The single file `plotwork.mjs` also contains code and data that other people wrote: the ELK layout engine, icon artwork, and font measurements. Each keeps its own licence. `THIRD-PARTY-NOTICES.md` lists every one with its version, copyright, and the full licence text. The MIT licence of this package does not replace those licences.

The package includes the official architecture icons of AWS, Microsoft Azure, Google Cloud, unmodified. They belong to their owners and are **not** covered by the MIT licence. Their terms and the trademark notice are in `THIRD-PARTY-NOTICES.md`.

## About this repository

This repository holds the built package that is published to npm. A change here is replaced by the next release, so open an issue instead of a pull request. A published version is never changed; a fix is a new version.

Each release carries `checksums.txt` with the SHA-256 of every file in the package. To check a download:

```sh
sha256sum --check checksums.txt
```

## See also

| Link | Why |
|---|---|
| [Command line documentation](https://docs.plotwork.dev/cli/overview) | Install, every command, CI on GitHub Actions, GitLab, Azure Pipelines, Jenkins, and CircleCI, exit codes, and troubleshooting |
| [Documentation](https://docs.plotwork.dev) | The diagram language and the guides |
| [Issues](https://github.com/plotwork-official/plotwork-cli/issues) | Report a problem or ask a question |
| [Security policy](https://github.com/plotwork-official/plotwork-cli/security/policy) | Report a vulnerability privately |
