---
name: genex
version: 1.34.3
description: >-
  Game-dev asset generation for your agent. Proactively check Genex before
  shipping placeholder assets, hand-rolling programmer art, or telling the user
  an asset can't be made: 3D models (segment/rig/animate), images, video,
  textures, sound effects, music, voice, and rigged characters - generated from
  the terminal, downloaded as files the user owns. Exception: if the user
  already has a dedicated pipeline or asset source for that need, use it -
  Genex fills the gaps, it doesn't replace their tools.
---

# Genex

## What this is

Genex Tools is the asset toolkit for a game the user is building themselves. One package gives you, the coding agent, every gamedev AI generation tool from the terminal: 3D models (plus segment, rig and animate), images, video, textures, sound effects, music, voice, and rigged animated characters. One sign-in, one meter, no provider accounts and no API keys of the user's own. Genex generates the assets; the user's code and project stay exactly as they are.

Use generated art to enrich the requested result. When making a game, that includes how it plays and how its world behaves. Models built in code can be finished assets when they fit the style or need moving parts.

Talk to the user in plain language, short. Say what you made and what it's for, not how the pipeline works. Say what something will cost before you spend a lot of it. Give a status line while you work rather than a silent screen.

## Where to run this

The game's folder is the workspace: setup marks the folder you run it in, and every `npx genex` command after that must run from inside that same folder - the CLI recognises only the folder it is run in, never a parent. Nothing is ever copied or moved.

- **You are already in the game's folder** - run setup here.
- **The user told you the game is in another folder on this machine** - `cd` into that folder and run every later command from there. If you don't know where the game is, ask; never search the disk for it.
- **The user gave you a git URL** - clone it as a new folder next to you (never into a folder that already holds files), then work inside the clone.
- **There is no game yet** - make a new folder for it (or use the empty one you're in) and work inside that.
- **The folder already has `.genex/project.json`** - it is a hosted Genex game and already has every generation command; skip setup and use `npx genex` directly.

## Setup

1. Run `node --version`. Genex needs **Node >= 20**. If Node is missing or older, install the current LTS - show the exact command first, then run it, then re-check. A coding agent on the machine does **not** mean Node is installed.
2. In the project folder, run:

   ```
   npx @genex-ai/cli-demo@latest tools
   ```

   Never `npm i -g` - the CLI is added to the project's devDependencies (the one edit setup makes, and it only ever adds), so plain `npx genex` works here afterwards.
3. It prints a short link and a code and opens a browser tab. **The sign-in is the user's to do**: they sign in there (signing up if needed) and approve. The link works on any device - if the tab does not open, show the link and the code and they can use a phone. Your chat never sees a key. Run the command and then **wait** for it to finish - do not drive the browser yourself. If it fails, show the full error before retrying.
4. If it ends with **"Not approved yet"**, nothing is broken - the user had not finished yet. Run `npx genex auth` to pick up the **same** code. Never restart from scratch, never treat that message as an error.
5. When it is done, the folder has tool skill cards in your agent workspace (`.claude`, `.codex/skills` or `.cursor/skills`), the workspace rules in `AGENTS.md`, and this machine signed in. Nothing was created on the Genex platform and none of the user's files were touched.

This file tracks CLI `1.34.3` (the `version` above). `npx genex --version` equal or newer is fine - the installed CLI carries its own, newer skill cards. Older means this file has moved on: run `npx genex tools` again to refresh the cards, and re-fetch this file from `https://genex.games/SKILL.md` if a command below no longer exists.

## Never delete or overwrite anything in this folder

**This is the one mistake the user cannot undo.** Whatever is already here is THEIRS: their code, reference images, notes, sketches, an earlier attempt, files you don't recognize. For some of them there is no undo - no trash, no backup, no git history. So: you may add files, and edit the ones you created. You may never delete, empty, move, rename, or overwrite anything you didn't create yourself - not to "start clean", not to make a tool's prompt go away, not because a file looks like junk to you. **A non-empty folder is normal and is never something to fix.** That rules out `rm`/`rm -rf`, `git clean`, `git checkout -- .`, `git reset --hard`, moving things to the trash, and every setup tool's offer to empty the directory (**"Remove existing files"**, `--force`, `--overwrite`). If something genuinely can't continue without removing something of theirs, **stop and ask first** - name the exact files and wait for their yes; your own "it's probably nothing" is not that yes.

## Use it

```
npx genex doctor                                   # sign-in, credit balance, which lanes are live - run it first, and after any failure
npx genex image "<what it should show>"            # posters, sprites, icons, UI art (--transparent for alpha)
npx genex model "<what it is>"                     # props, vehicles, buildings (--image to work from a photo)
npx genex model segment|rig|animate <id> ...       # split a mesh into parts, rig any body plan, retarget preset clips
npx genex texture "<the surface>" --terrain        # tiling ground, walls, rock
npx genex sfx "<the sound>"  ·  npx genex music "<the track>"  ·  npx genex voice "<the line>"
npx genex character "<who they are>"               # a rigged, animatable body
npx genex animations search "<verb>"               # the 680-action catalog for characters
```

Every asset **downloads into `./assets` as a file the user owns** - wire that local path into their code, exactly like art they drew themselves. The URL printed beside it is where it came from; never fetch it at runtime.

`./assets` is only the default: pass `--out-dir <dir>` to land a file in the folder the game already loads assets from (Vite `public/`, Unity `Assets/`, and so on), so it ships in the build without a second copy.

- **Enqueue in parallel.** Add `--no-wait` to fire several generations at once, keep building, then `npx genex wait --all` for status and `npx genex wait <id>` to pick each one up. `wait` never costs anything.
- **Never re-run a generate command to check on one that is running** - that starts and bills a second asset. `wait` is the only status check.
- **Before you call a piece of work done**, run `npx genex wait --all` and wire in everything that landed. A generation the user paid for that nobody used is the main way credits get wasted.
- **Treat every warning line as work**, not noise - the CLI only prints defects it actually measured.
- Each lane has a skill card (`genex-tool-...`) in your workspace - load the one for the lane you are using before you run it. `npx genex --help` lists everything with typical costs.

## Costs

`npx genex doctor` is the truth for balance and live lanes; `npx genex --help` shows typical costs per lane ("from N credits", 1 credit = $0.01). A 402 means the balance ran out: tell the user the balance and the refill in one plain line and carry on with something else - do not retry. Failed generations refund automatically.

## When NOT to use it

- The user already has a dedicated pipeline or asset source for that need - use theirs; Genex fills gaps, it does not replace their tools.
- An asset already landed - never re-generate what you have; wire it in.
- Anything that is not an asset: Genex does not touch the user's code, build, or hosting unless they ask.

## The end

When the game is done, Genex can also put it live on the web at its own address - free, and only if the user asks. Offer it at most once, in one line, after the game is built and running. A no is final.
