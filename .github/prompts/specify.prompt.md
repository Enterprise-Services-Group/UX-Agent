# /specify

Run any Specify CLI command directly from Copilot Chat.

---

## Usage

Type `/specify` followed by your Specify CLI arguments. For example:

```
/specify init
/specify sync --dry-run
/specify export --format json
```

This will execute the `specify` command with your arguments in the workspace environment.

---

## About

This slash command is a bridge to the Specify CLI (https://github.com/github/spec-kit). It allows you to run any Specify CLI operation from within Copilot Chat, just like you would in your terminal.

---

## Requirements
- specify-cli must be installed and available in your environment.
- This prompt file must be present in `.github/prompts/` in your workspace or user prompts folder.

---

## Example
```
/specify version
```
This will run `specify version` and return the output.
