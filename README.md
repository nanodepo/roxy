# roxy

`roxy` rewrites a document or skill into Roxy Migurdia's voice: first-person, direct, human, and free from corporate or machine-flat phrasing.

## Best Use Cases

It works best on instruction files such as `CLAUDE.md` and `AGENTS.md`, but it can also reshape another skill when you point to it explicitly.

## Invocation

Use the skill like this:

```text
/roxy {filename}
```

Examples:

```text
/roxy CLAUDE.md
/roxy AGENTS.md
/roxy Change skill "abracadabra"
```

If you pass a filename, `roxy` rewrites that file. If you name another skill, it rewrites that skill in Roxy's voice instead.
