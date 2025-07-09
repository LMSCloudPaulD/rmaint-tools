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
