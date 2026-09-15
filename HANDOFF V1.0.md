# Project Handoff — Behavior Kit Agency

**Purpose of this file:** This is the source of truth for this project. If you're starting a new Claude conversation, paste this entire file as your first message so Claude has full context and doesn't need to rediscover anything.

Last updated: 2026-09-15

---

## 1. Current State (read this first)

- **Project**: Plain HTML site — no framework, no build step (just `index.html` + `assets/`)
- **Local folder**: `E:\VS-Code-Project\behavior-kit-mockup - agency`
- **This is a fork/copy** of an original project (`E:\VS-Code-Project\behavior-kit-mockup`), intentionally separated so it can live under a different GitHub + Vercel account without touching the original.
- **GitHub repo**: `https://github.com/podcastkimiwalker/behavior-kit-agency` (account: `podcastkimiwalker`)
- **Vercel project**: deployed under the `podcastkimiwalker` Vercel account, team name "Podcast" (Hobby plan). Project name: `behavior-kit-agency`.
- **Deployment status**: ✅ Live. Deployed via Vercel's GitHub integration — pushes to `main` should auto-deploy going forward, assuming the Vercel GitHub App still has access to this repo (see Section 3).
- **Local git identity for this folder only**: `user.name = podcastkimiwalker`, `user.email = podcast@kimiwalker.com` (set locally, not globally — original project's git identity is untouched).

## 2. Original Project (do not modify from this thread)

- **Local folder**: `E:\VS-Code-Project\behavior-kit-mockup`
- **GitHub account**: TriuniSolutions
- **Vercel account**: original/separate account
- This project's git config, GitHub remote, and Vercel deployment were left completely untouched throughout this work. No changes were made to it.

## 3. Known Gotchas / Things to Watch

- **Credential switching**: This machine has previously hit `403` errors when pushing to `podcastkimiwalker`'s repo because Windows Credential Manager cached GitHub credentials for `TriuniSolutions`. Fix if it recurs: open Windows Credential Manager → Windows Credentials → remove the `git:https://github.com` entry → push again to get a fresh login prompt (make sure to log in as `podcastkimiwalker`).
- **Folder vs. terminal mismatch**: Early on, VS Code's Explorer panel was showing the *original* mockup folder while the terminal was `cd`'d into the agency folder. Always confirm the Explorer panel header says `BEHAVIOR-KIT-MOCKUP - AGENCY` before editing, or use File → Open Folder to explicitly reopen the agency folder if unsure.
- **Copying folders in Windows Explorer carries the hidden `.git` folder with it.** If you ever duplicate this project again, remember to delete the `.git` folder and run `git init` fresh in the copy before connecting it to a new GitHub repo — otherwise it'll still think it's the original repo.
- **Vercel GitHub App permissions**: Vercel could only "see" repos it's been explicitly granted access to for the `podcastkimiwalker` account. If a future repo doesn't show up in Vercel's import list, go to the import screen → "Adjust GitHub App Permissions" → grant access to the new repo.

## 4. Pending / Not Yet Done (low priority)

- [ ] **SSH key setup per GitHub account** — would eliminate the need to manually clear Windows Credential Manager every time you switch between pushing to `TriuniSolutions` vs `podcastkimiwalker` repos. Not urgent, but worth doing if this project (or more multi-account projects) continues.

## 5. Deploy / Update Workflow (for ongoing edits)

From the agency folder in VS Code terminal:
```
git add .
git commit -m "describe your change"
git push
```
This should auto-trigger a new Vercel deployment (via the GitHub integration) within about 10–30 seconds.

---

## Appendix: Technical Log (how we got here)

1. Original project existed at `behavior-kit-mockup`, connected to TriuniSolutions GitHub + its own Vercel account.
2. User duplicated the folder in Windows Explorer to `behavior-kit-mockup - agency`, intending to run it under a separate GitHub + Vercel account (`podcastkimiwalker`) while preserving the original untouched.
3. Discovered the copy retained the original's hidden `.git` folder. Removed it:
   ```
   Remove-Item -Recurse -Force .git
   ```
4. Verified disconnection (`git status` → "not a git repository"), then re-initialized:
   ```
   git init
   git add .
   git commit -m "initial commit - agency version"
   ```
5. Created a new empty repo on GitHub under `podcastkimiwalker`: `behavior-kit-agency`.
6. Connected and pushed:
   ```
   git remote add origin https://github.com/podcastkimiwalker/behavior-kit-agency.git
   git branch -M main
   git push -u origin main
   ```
7. Hit a `403 Permission denied to TriuniSolutions` error — Windows Credential Manager had cached the old account's GitHub login. Removed the cached `git:https://github.com` entry via Credential Manager, retried push, authenticated fresh as `podcastkimiwalker` via browser OAuth flow. Push succeeded.
8. Noticed the commit author showed as "TriuniSolutions" on GitHub despite pushing under the correct account (git commit authorship is separate from push credentials). Fixed by setting local (non-global) git identity:
   ```
   git config user.name "podcastkimiwalker"
   git config user.email "podcast@kimiwalker.com"
   ```
   Then rewrote the existing commit's author and force-pushed (safe since this is a solo, brand-new repo with no collaborators):
   ```
   git commit --amend --author="podcastkimiwalker <podcast@kimiwalker.com>" --no-edit
   git push --force origin main
   ```
9. Imported the repo into a new Vercel account (`podcastkimiwalker`, team "Podcast", Hobby plan). Had to first grant the Vercel GitHub App access to the new repo via "Adjust GitHub App Permissions," since it initially only had access to a different repo (`sop-audit-dashboard`).
10. Set Application Preset to "Other" (plain HTML, no framework), left build settings blank, deployed successfully.
11. Discovered VS Code's Explorer panel was still showing the original folder's files even though the terminal was correctly `cd`'d into the agency folder — a workspace/window mismatch, not a git issue. Resolved by using File → Open Folder to properly open the agency folder as the active workspace.
