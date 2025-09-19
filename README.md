# Actionfile specification


An _Actionfile_ is a special Markdown-based configuration file that describes a set of actions, scripts, or tasks for use in automation workflows, dotfile management, or application deployment. It is designed for human readability and machine parsing, combining documentation, configuration, and executable code blocks in one file.

## Structure

An Actionfile is organized into **sections** with Markdown headers. Each section can contain:

  - **Introduction/Description**:  
    General information about the action.
  - **Config Block**:
    INI-style configuration, usually under a `### config` header and fenced in a code block.
  - **Variables Block**:
    Shell variable assignments, under a `### vars` header and fenced in a code block.
  - **Action Sections**:
    Individual actions, typically under `### {action-context}` headers, containing documentation and executable code blocks.


## Implementations

  - [Actionfile.sh](https://github.com/gbraad-redhat/actionfile.sh)
  - [Actionfile.js](https://github.com/gbraad-redhat/actionfile.sh)

### The following as specific changes that I use for my own [dotfiles](https://dotfiles.gbraad.nl)

  - [`action.zsh`](https://github.com/gbraad-dotfiles/upstream/blob/main/zsh/.zshrc.d/action.zsh)
  - [`app.zsh`](https://github.com/gbraad-dotfiles/upstream/blob/main/zsh/.zshrc.d/app.zsh) and [`apps.md`](https://github.com/gbraad-redhat/actionfile/blob/main/actionfiles/apps.md)
  - [Application definitions](https://github.com/gbraad-dotfiles/applications)


## Example

---
`myapp.md`
# MyApp Actionfile

This Actionfile defines actions for installing and configuring MyApp.

---

### config
```ini
[install]
  user = "myuser"
  path = "/opt/myapp"
```

> [!NOTE]
> These will be exported as `INSTALL_USER`, and `INSTALL_PATH`. This can be overriden by an `.ini` file on disk that is named like the Actionfile, but using the `.ini`-extension.


### vars
```sh
APP_VERSION="1.2.3"
COPFIG_REPO="..."
```

## install

Installs MyApp to the configured path.

```sh
echo "Installing MyApp version $APP_VERSION to $INSTALL_PATH"
# installation script goes here
```

### post-install

Configures MyApp after installation.

```sh
echo "Configuring MyApp for user $INSTALL_USER"
# configuration script goes here
```


---

## Section Types

| Section      | Header Example     | Purpose                                           |
|--------------|--------------------|---------------------------------------------------|
| Description  | `# ...`, `## ...`  | File-level docs                                   |
| Config Block | `### config`       | INI-style settings inside a fenced `ini` block    |
| Vars Block   | `### vars`         | Shell variable assignments in a fenced `sh` block |
| Action       | `### action`       | Each action with docs and fenced `sh` code block  |

---

## Parsing Conventions

  - **Config blocks** (INI) are only parsed if under `### config` and inside a <code>ini</code> code block.
  - **Vars blocks** are only parsed if under `### vars` and inside a <code>sh</code> code block.
  - **Actions** are discovered by scanning for `###` headers; code blocks under these are the scripts to run.
  - Comments and documentation outside code blocks are ignored by automation tools but helpful for users.
