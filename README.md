# Release Maintenance Tools (rmaint-utils)

This repository provides a set of helper functions for release maintenance workflows, including Git worktree management, release branch helpers, and Docker utilities for Koha and related environments.

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

## Available Functions

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

## Configuration

The script uses several environment variables that can be customized:

- `KTD_HOME`: Path to your ktd home directory (required for `lmsd`)
- `LMSCLOUD_IMAGE`: LMSCloud docker image (default: `ghcr.io/lmscloudpauld/lmscloud-koha-aarch64:22.11.15`)
- `COMPOSE_LMSCLOUD`: Main docker-compose file (default: `docker-compose-lmscloud.yml`)
- `COMPOSE_PLAPI`: Public Library API docker-compose file (default: `docker-compose.koha-public-library-api.yml`)
- `VENDOR`: Vendor/project name (default: `lmscloud`)

You can set these in your shell configuration file before sourcing the script:

```bash
export LMSCLOUD_IMAGE="your-custom-lmscloud-image:tag"
export VENDOR="your-project-name"
```

## Requirements

- [fzf](https://github.com/junegunn/fzf) (for `gwts`)
- [git](https://git-scm.com/)
- [docker](https://www.docker.com/)
- [ktd](https://github.com/lmscloudpauld/ktd) (for Koha docker helpers)
- [rg (ripgrep)](https://github.com/BurntSushi/ripgrep) and [sd](https://github.com/chmln/sd) (for `get_unique_bugs`)

## License

See [LICENSE](LICENSE).
