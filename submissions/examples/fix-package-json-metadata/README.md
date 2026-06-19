# Fix: package.json Metadata

## What this fixes
Corrects the `repository`, `bugs`, and `homepage` fields in the root `package.json`.

## Changes
- `repository.url` — normalized to lowercase
- `bugs.url` — normalized to lowercase  
- `homepage` — changed from GitHub Pages URL to GitHub repo README link

## Related Issue
Fixes #13201