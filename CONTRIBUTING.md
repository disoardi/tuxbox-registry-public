# Contributing to TuxBox Public Registry

Thank you for wanting to share your tool with the community!
Adding a tool takes about **5 minutes** and requires only a PR to this repo.

---

## Before You Start

Your tool must:
- Live in a **public Git repository** (GitHub, GitLab, etc.)
- Have a clear **entry point** (a script, a Python module, a binary)
- Work on Linux and/or macOS

---

## Step 1 — Understand Tool Types

TuxBox supports two execution modes:

### `type = "python"`
For Python tools. TuxBox will:
- Clone the repository
- If a `Dockerfile` is present → build and run in Docker (recommended for complex deps)
- Otherwise → create a Python venv and install `requirements.txt` or `pyproject.toml`

```toml
[tools.my_tool]
type = "python"

[tools.my_tool.dependencies]
python       = ">=3.8"
requirements = "requirements.txt"   # or: poetry = true
```

### `type = "bash"`
For shell scripts, Ansible playbooks, or anything executable directly.
TuxBox runs the entry point script in the tool's directory.

```toml
[tools.my_tool]
type = "bash"

[tools.my_tool.commands]
run = "./scripts/run.sh"   # path relative to repo root
```

---

## Step 2 — Add Your Tool to `tools.toml`

Open `tools.toml` and add a new `[tools.<name>]` section.

### Minimal entry

```toml
[tools.my_awesome_tool]
name        = "my_awesome_tool"
repo        = "https://github.com/youruser/my-awesome-tool"
branch      = "main"
version     = "1.0.0"
type        = "python"          # or "bash"
description = "One-line description of what the tool does"
author      = "youruser"        # your GitHub username

[tools.my_awesome_tool.commands]
run = "python3 -m my_awesome_tool"   # how to invoke the tool

[tools.my_awesome_tool.dependencies]
python       = ">=3.8"
requirements = "requirements.txt"
```

### All available fields

```toml
[tools.my_tool]
name        = "my_tool"          # required — unique identifier (no spaces)
repo        = "https://..."      # required — HTTPS URL (public repos)
branch      = "main"             # optional — default: main
version     = "1.0.0"            # optional — for display only
type        = "python"           # required — "python" or "bash"
description = "..."              # required — shown in `tbox list`
author      = "username"         # required — your GitHub username
private     = false              # optional — set true for SSH repos (private registries only)

[tools.my_tool.commands]
run = "..."                      # required — how to run the tool

[tools.my_tool.dependencies]
python       = ">=3.8"           # optional — minimum Python version
requirements = "requirements.txt" # optional — pip requirements file
poetry       = true              # optional — set true if tool uses Poetry
```

---

## Step 3 — Test Locally

Before opening a PR, verify your entry works end-to-end:

```bash
# 1. Make sure tbox is installed
tbox --version

# 2. Point tbox to your local fork of the registry
tbox init file:///path/to/your/tuxbox-registry-public

# 3. Check your tool appears in the list
tbox list | grep my_tool

# 4. Clean slate (force fresh clone)
rm -rf ~/.tuxbox/tools/my_tool

# 5. Run it!
tbox run my_tool --help

# 6. Run again (should skip clone)
tbox run my_tool --help
```

Expected behaviour:
- **First run**: clones repo → sets up environment → runs tool
- **Second run**: skips clone → runs directly (faster)

---

## Step 4 — Open a Pull Request

1. Fork this repository
2. Add your tool to `tools.toml`
3. Open a PR with:
   - **Title**: `feat: add <tool_name>`
   - **Description**: what the tool does, link to its repo, tested platforms

### PR checklist

- [ ] Entry has all required fields (`name`, `repo`, `type`, `description`, `author`)
- [ ] Repository is public and accessible without authentication
- [ ] Tool has a README or some documentation
- [ ] Tested locally with `tbox run <tool_name>`
- [ ] Entry uses HTTPS URL (not SSH) for `repo`

---

## Naming Conventions

- `name` should be lowercase with underscores: `my_tool` ✅, `MyTool` ❌
- Keep descriptions concise (< 80 chars)
- `author` should match your GitHub username

---

## Tool Types — Decision Guide

```
Does your tool have a Dockerfile?
  ├── YES → type = "python", tbox uses Docker automatically
  └── NO
       ├── Is it a Python package/module?
       │     └── YES → type = "python"
       └── Is it a shell script / Ansible playbook?
             └── YES → type = "bash", set run = "./your_script.sh"
```

---

## Questions?

Open an [issue](https://github.com/disoardi/tuxbox-registry-public/issues) or
check the [TuxBox documentation](https://disoardi.github.io/tuxbox).
