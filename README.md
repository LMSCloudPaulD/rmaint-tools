# Release Maintenance Tools (rmaint-tools)

This repository provides a set of helper functions and scripts for release maintenance workflows, including Git worktree management, release branch helpers, Docker utilities for Koha and related environments, and Bugzilla integration.

## Usage

To use these functions in your shell (bash or zsh), source the `utils` script in your shell configuration file:

### Bash

Add the following line to your `~/.bashrc` or `~/.bash_profile`:

```bash
source /path/to/your/clone/rmaint-tools/utils
```

### Zsh

Add the following line to your `~/.zshrc`:

```zsh
source /path/to/your/clone/rmaint-tools/utils
```

Replace `/path/to/your/clone/rmaint-tools/` with the actual path where you cloned this repository.

After sourcing, you can use the functions in any new terminal session.

### Using the bzq Script

The `bzq` script can be used directly without sourcing:

```bash
# Fetch and display bug 12345
./bzq 12345

# The script will show cache status and display the bug details
```

The script automatically caches responses to reduce API calls and improve performance.

## Available Functions and Scripts

### Git Worktree Helpers

- **gwts**: Fuzzy-select and cd into a git worktree directory.

### Git Release Maintenance Helpers

- **gcrm**: Create and switch to a new rmaint bug branch from the current rmaint branch.
- **gcorm**: Switch to the current rmaint bug branch (N.N.xbug_NNNNN).
- **gmffrm**: Fast-forward merge the current rmaint bug branch into the base rmaint branch.
- **get_unique_bugs**: Extract unique bug numbers from commit messages on stdin.

### Docker Helpers

- **lmsd**: Run Koha docker-compose commands in the `$KTD_HOME` directory.
- **dtk**: Run ktd with a given variant and action, optionally switching worktree if `$ASK` is set.
- **dtka**: Like dtk, but always prompts for worktree selection.
- **kdp**: Start and follow logs for a named docker environment.

### Bugzilla Integration

- **bzq**: Fetch and display Koha Bugzilla issues with intelligent caching.
  - Usage: `./bzq <bug_number>`
  - Displays bug details in markdown format with comments
  - Caches responses to reduce API calls
  - Configurable via environment variables
- **git-bz-overlay**: Overlay Bugzilla metadata onto commits produced by `git log`.
  - Usage: `./git-bz-overlay [options] [revisions...] [-- <extra git log args>...]`
  - Request arbitrary Bugzilla fields (`--field`, `--fields`), customise output via templates (`--format`), guard large commit selections (`--max-commits`/`KOHA_BZ_MAX_COMMITS`), control retry/throttle behaviour (`--max-retries`, `--request-delay`), batch API calls (`--batch-size`/`KOHA_BZ_BATCH_SIZE`), and emit pipe-friendly rows with a custom delimiter (`--separator`/`KOHA_BZ_SEPARATOR`).
  - Structured rows include: `commit_hash`, `commit_subject`, `bug_id`, `status`, `importance`, `assignee`, `qa_contact`, `depends_on`, `blocks`, `resolution`, `summary`, `bug_url`, `status_block`, `status_resolution`, and the rendered template output. A header row is emitted by default when a separator is used (disable with `--no-header`/`KOHA_BZ_HEADER=0`).
  - Caches per-bug responses in `~/.cache/git-bz-overlay` by default (configurable via `--cache-*` flags or environment).
- **git-rmaint**: Thin wrapper around `git-bz-overlay` that injects the canonical release-maintenance `git log --right-only --cherry-pick --no-merges` selection and defaults to tab-separated output (override via `KOHA_BZ_SEPARATOR`).
- Manual pages for both scripts live under `man/` and can be viewed with `man -l man/git-bz-overlay.1` or `man -l man/git-rmaint.1`.

## Configuration

The scripts use several environment variables that can be customized:

### Docker Configuration

- `KTD_HOME`: Path to your ktd home directory (required for `lmsd`)
- `LMSCLOUD_IMAGE`: LMSCloud docker image (default: `ghcr.io/lmscloudpauld/lmscloud-koha-aarch64:22.11.15`)
- `COMPOSE_LMSCLOUD`: Main docker-compose file (default: `docker-compose-lmscloud.yml`)
- `COMPOSE_PLAPI`: Public Library API docker-compose file (default: `docker-compose.koha-public-library-api.yml`)
- `VENDOR`: Vendor/project name (default: `lmscloud`)

### Bugzilla Configuration

- `KOHABUGZILLA_API_BASE`: Koha Bugzilla API base URL (default: `https://bugs.koha-community.org/bugzilla3`)
- `KOHA_BZ_BATCH_SIZE`: Number of bug IDs fetched per API request (default: `20`)
- `KOHA_BZ_MAX_COMMITS`: Maximum commits before prompting for confirmation (default: `400`; `0` disables)
- `KOHA_BZ_MAX_RETRIES`: Retry failed Bugzilla fetches this many times before giving up (default: `3`; `0` allows unlimited retries)
- `KOHA_BZ_REQUEST_DELAY`: Seconds to wait between Bugzilla batch requests (default: `0.2`; set to `0` to disable throttling)
- `KOHA_BZ_SEPARATOR`: When set, emit delimiter-separated rows (`commit_hash`, `commit_subject`, Bugzilla fields, rendered template) using the given separator (default: ` | `). Literal escape sequences like `\t` or `\n` are recognised.
- `KOHA_BZ_HEADER`: Set to `false`/`0` to suppress the header row when using structured output (default: enabled)
- `KOHA_BZ_ASSUME_YES`: Set to `true`/`1` to continue automatically when limits are exceeded
- `KOHA_BZ_PROGRESS`: Set to `false`/`0` to suppress progress messages (enabled by default)
- `KOHA_BZ_CACHE_DIR`: Cache directory for `git-bz-overlay` (default: `~/.cache/git-bz-overlay`)
- `KOHA_BZ_CACHE_TTL`: Cache duration in seconds for `git-bz-overlay` (default: `300`; `<=0` disables cache)
- `BZQ_CACHE_TTL`: Cache TTL in seconds for bzq (default: `300`)

You can set these in your shell configuration file before sourcing the script:

```bash
export LMSCLOUD_IMAGE="your-custom-lmscloud-image:tag"
```

## Requirements

### Core Dependencies

- [git](https://git-scm.com/)
- [docker](https://www.docker.com/)

### Function Dependencies

- [fzf](https://github.com/junegunn/fzf) (for `gwts`)
- [ktd](https://gitlab.com/koha-community/koha-testing-docker) (for Koha docker helpers)
- [rg (ripgrep)](https://github.com/BurntSushi/ripgrep) and [sd](https://github.com/chmln/sd) (for `get_unique_bugs`)

### Script Dependencies

- [curl](https://curl.se/) (for `bzq` API calls)
- [jq](https://jqlang.github.io/jq/) (for `bzq` JSON parsing)
- [skate](https://github.com/charmbracelet/skate) (optional, for `bzq` caching)

## License

See [LICENSE](LICENSE).
