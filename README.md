# md-exec

Run code blocks from a Markdown file – documented workflows (setup, test, cleanup …)
stay readable *and* executable in a single file.

## Idea

Each fenced code block uses a tag as its language identifier. `md-exec` extracts the
requested section, prepends the `global` section, and executes it.

````markdown
[//]: # (md-exec: global)
```global
X="my_global_var"
```

[//]: # (md-exec: execute test)
```test
echo "execute test with X=$X"
```
````

## Usage

```
md-exec <md-file> [-e] <command> [section] [-o VAR=VALUE]...
```

| Command  | Effect                                                       |
|----------|--------------------------------------------------------------|
| `list`   | list available sections (tags)                               |
| `show`   | show relevant blocks including the ``` ``` ``` fences        |
| `dump`   | print a section as a plain script                            |
| `run`    | execute `global` + section                                   |
| `create` | create a template `.md`                                      |

Without `section` all sections apply. The exit code of `run` is the script's exit code.

### Options

| Option              | Effect                                                                  |
|---------------------|-------------------------------------------------------------------------|
| `-o VAR=VALUE`      | override a variable assignment before execution (repeatable)            |
| `-e`, `--envsubst`  | on `dump`, substitute `${VAR}` with values from `global` (needs `gettext`) |
| `-h`, `--help`      | show help                                                               |
| `-v`, `--version`   | show version                                                            |

## Examples

```bash
md-exec tasks.md create                    # create a template
md-exec tasks.md list                      # list sections
md-exec tasks.md run test                  # run 'test' (with global)
md-exec tasks.md run test -o X="new value" # override variable X
md-exec tasks.md dump test -e              # print 'test' with values substituted
```
