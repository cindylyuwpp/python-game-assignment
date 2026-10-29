# Key Manager

A small Python class for loading and adding secret keys (API keys, tokens) without putting them in your code or uploading them to GitHub.

## Files

| File | Purpose |
|------|---------|
| `key_manager.py` | The `KeyManager` class |
| `.env.example` | Template showing which keys are needed (safe to upload) |
| `.env` | Your real keys — **ignored by git, never uploaded** |
| `.gitignore` | Keeps `.env`, caches and virtual environments out of the repo |

## Setup

1. Requires Python 3.10+.
2. Copy the template and add your real key:
   ```bash
   cp .env.example .env
   ```
3. Edit `.env`:
   ```
   EXAMPLE_API_KEY=abc123...
   ```

## Usage

```python
from key_manager import KeyManager

keys = KeyManager()                  # reads .env
api_key = keys.require("EXAMPLE_API_KEY")   # errors clearly if missing

keys.add("NEW_SERVICE_KEY", "xyz789")       # adds a new key and saves it to .env
print(keys.names())                         # lists key names, never values
```

Environment variables take priority over `.env`, so on a server you can set keys there instead.

## Before uploading

Check that your key file is not being tracked:

```bash
git status        # .env should NOT appear
```

If `.env` was committed before, remove it from tracking (the file stays on your computer):

```bash
git rm --cached .env
git commit -m "Stop tracking .env"
```

Then change that key with the provider, since it was already exposed.
