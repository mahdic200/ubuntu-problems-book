# Git commit finder

```bash
git log --all -S "sidebar" --oneline
```

Searches for commits where the string "sidebar" was added or removed.
Or by file:

```bash
git log --oneline -- *sidebar*
```

Or by function name:

```bash
git log -L :functionName:path/to/file.tsx
```

The `-L` one is the interesting one you probably read about - it tracks the history of a specific function across commits.