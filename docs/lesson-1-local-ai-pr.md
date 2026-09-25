# Lesson 1: A labeled issue becomes a draft CSS PR

This first version is deliberately narrow. It only lets AI replace `css/style.css` and it always creates a draft pull request. It cannot push to `main`.

## What the workflow does

1. A maintainer adds the `ai-ready` label to an issue.
2. GitHub sends the job to a runner on this Mac, labeled `bionic`.
3. The runner sends the issue text and the current CSS to Bionic at `127.0.0.1:1234`.
4. Bionic returns the complete proposed CSS as JSON.
5. The runner commits that one file on `codex/issue-<number>-css-fix`, pushes the branch, and opens a draft PR.

Pushing the proposal branch is necessary for GitHub to display a PR. The workflow never pushes to the default branch.

## One-time GitHub runner setup

1. On the repository's GitHub page, open **Settings**, then **Actions**, then **Runners**.
2. Choose **New self-hosted runner**, select macOS and ARM64, and follow GitHub's displayed commands in a terminal on this Mac.
3. When GitHub asks for runner labels, add `bionic`. The default `self-hosted` label stays in place.
4. Start the runner. Leave it running while testing. GitHub should show it as **Idle**.
5. In **Settings > Actions > General**, allow workflows to have **Read and write permissions**. The workflow needs this only to push its proposal branch and create a PR.

## First test

1. Commit these workflow files and get them onto GitHub through a normal reviewed setup PR.
2. Keep Bionic Desktop open with its local server and `qwen3.5-4b-mlx` loaded.
3. Remove `ai-ready` from your background-color issue, then add it again. Events do not replay for labels added before the workflow existed.
4. Watch the run under GitHub's **Actions** tab. It should finish with a draft PR changing only `css/style.css` to use `#333333`.

If the model returns invalid JSON or Bionic is closed, the job fails before it can push anything. That is intentional: failure is safer than a guess.
