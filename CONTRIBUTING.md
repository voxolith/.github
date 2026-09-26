# Contributing to Voxolith

Thanks for looking. Voxolith is a small project with one maintainer, so the most useful
contributions are focused ones: a bug with a clear reproduction, a fix with its reason, an
example that shows something the engine can already do.

This guide covers every public repo in the organisation. The documentation itself lives at
[voxolith.github.io/docs](https://voxolith.github.io/docs/) (repo:
[voxolith.github.io](https://github.com/voxolith/voxolith.github.io)). A repo's own README has the details of
its package; where it disagrees with this file, the README wins.

## Where things go

| repo | what | file issues about |
|---|---|---|
| [renderer](https://github.com/voxolith/renderer) | `@voxolith/renderer`, the WebGPU raymarcher, `.vox` / `.mca` I/O | rendering, shaders, GPU errors, file parsing |
| [engine](https://github.com/voxolith/engine) | `@voxolith/engine`: entities, generator contract, placement, streaming, input, animation, atmosphere | anything above the renderer and below an app |
| [generators](https://github.com/voxolith/generators) | `gen-kit` and the tree, bush, grass, rock, building, creature and terrain generators | generated models, parameters, share codes |
| [viewer](https://github.com/voxolith/viewer) | `.vox` / `.mca` viewer | the viewer app |
| [editor](https://github.com/voxolith/editor) | voxel editor (early) | the editor app |
| [examples](https://github.com/voxolith/examples) | example pages | a single example |
| [demolition-shot](https://github.com/voxolith/demolition-shot) | mobile demolition game | the game |
| [voxolith.github.io](https://github.com/voxolith/voxolith.github.io) | the website and documentation | docs pages, the tutorial, the site |

Not sure? Open it where you saw the problem; it can be transferred.

## Setting up

Voxolith is several repos that depend on each other as `"workspace:*"`, so they are checked out
side by side under one bun workspace root. The toolchain is **[bun](https://bun.sh) only**:
there is no npm or node step anywhere.

```sh
mkdir voxolith && cd voxolith
git clone https://github.com/voxolith/renderer
git clone https://github.com/voxolith/engine
git clone https://github.com/voxolith/generators
git clone https://github.com/voxolith/viewer        # and/or editor, examples, demolition-shot
```

Then create `package.json` in that folder, listing the repos you cloned:

```json
{
  "name": "voxolith-workspace",
  "private": true,
  "workspaces": ["renderer", "engine", "generators/*", "viewer"]
}
```

and install from there, never from inside a repo:

```sh
bun install
```

Which siblings a repo needs:

| working on | also clone |
|---|---|
| renderer | nothing |
| engine | renderer |
| generators | renderer, engine |
| editor, demolition-shot | renderer, engine |
| viewer, examples | renderer, engine, generators |

This is the same layout CI builds (see any repo's `.github/workflows/`).

### Running an app

```sh
bun run --cwd viewer dev       # https://localhost:5173
```

Dev servers are HTTPS with a self-signed certificate, because WebGPU needs a secure context.
Accept the certificate once. You need a browser with WebGPU: current Chrome or Edge, Safari 26+,
or Firefox with WebGPU enabled. On Linux, Chrome's WebGPU is much faster with
`--enable-features=Vulkan`; start a separate profile for it (`--user-data-dir=...`) rather than
flipping the flag globally, which can break video in other tabs.

## Before you open a pull request

Run the checks for every repo you touched. CI runs the same ones.

| repo | commands |
|---|---|
| renderer | `bun run --cwd renderer typecheck` and `bun run --cwd renderer verify` |
| engine | `bun run --cwd engine typecheck` and `bun run --cwd engine verify` |
| generators | `bun run --cwd generators typecheck`, `bun run --cwd generators verify` and `bun run --cwd generators/contract full` |
| apps | `bun run --cwd <app> build` (typechecks too), plus `verify` where the app has one |

Checks cannot see pixels. If your change affects what is drawn, open it in a WebGPU browser and
look, and put a before/after screenshot in the pull request.

## Conventions

The repo READMEs are the reference; these are the ones that are easy to break without noticing.

- **Raw TypeScript.** `@voxolith/renderer` ships sources, which consumers compile. Browser code
  imports from `@voxolith/renderer`; bun scripts import `@voxolith/renderer/core` (or `/vox`,
  `/ray`), never the barrel, which pulls shaders in through Vite's `?raw`.
- **`tsconfig.base.json` is identical in every repo.** Change it everywhere or nowhere.
- **Generators are pure and deterministic.** All randomness comes from the injected rng, never
  `Math.random`. Voxel values are role indices, not colours. A parameter default must lie on its
  step grid; fix the default, never the step, or old share codes decode to different models.
- **Constants shared with WGSL** (`BRICK_B`, `TOP_B` in `renderer/src/brick.ts` and
  `grid.wesl`) are kept in sync by hand.
- **Apps** take input from `@voxolith/engine/input`, never raw listeners; render on demand
  through `makeFrameLoop`; use the theme tokens (`--bg`, `--accent`, ...) instead of hex colours;
  and never hard-code absolute URLs (they are served under `/<repo>/` on GitHub Pages).
- **Dependencies:** use current versions. Don't add one for something a few lines can do.
- **British spelling** in prose and comments (`colour`, `normalise`); identifiers follow the
  web platform (`lightColor`). "Voxolith" in prose, capitalised.

## Commits

History is linear and read as prose, so write commits for the next person who runs `git log`.

- The subject says what is now true, in plain words: `Brick lookups take voxel coordinates`,
  `Generator workers can cache models in IndexedDB`. No `feat:` prefixes, no trailing full stop,
  about 70 characters at most.
- The body says why, and anything a reviewer would otherwise have to work out: what was wrong,
  what the change costs, what it deliberately does not do. Wrap at about 72 columns.
- One change per commit. Formatting and moves go in their own commit.

## Pull requests

- Keep each one to a single topic. Split an unrelated fix into its own pull request.
- Say what changed and why, how you tested it, and on which browser and GPU.
- Changes that span repos (say, a renderer API used by an app) are separate pull requests that
  link each other. Land the lower layer first: renderer, then engine, then generators, then apps.
- For a large change, open an issue first so the design can be agreed before the code is written.
- Pull requests are squash-merged or rebased; there are no merge commits on `main`.

## AI-assisted contributions

Voxolith itself is built with AI assistance. Almost all of its code and documentation was
drafted with Claude (through Claude Code), then directed, reviewed and run by the maintainer.
Commits made that way carry a `Co-Authored-By` trailer. AI tools are welcome here, on the
same terms the project holds itself to:

- **You are the author.** Understand every line you submit and be able to explain it and change
  it in review. "The model wrote it" is not an answer to a review question.
- **Run it.** Run the checks, and look at anything visual in a real browser. Don't submit code
  you haven't run, or a fix for a bug you haven't reproduced.
- **Say so.** Tick the AI box in the pull request template and name the tool. If your tool adds
  a `Co-Authored-By` trailer, keep it.
- **Stay in scope.** No unrequested rewrites, renames or reformatting, and no generated churn.
  A small diff that does one thing is far easier to review than a large one that does it plus
  other things.
- **Write for people.** Keep descriptions, issues and review replies short and specific. Don't
  paste unverified model output as a bug report; check the claim against the code first.
- **Only submit what you may.** Don't include code copied from sources whose licence is not
  compatible with MIT, whether you or a tool copied it.

## Licence

Everything public is MIT (see each repo's `LICENSE`). By contributing you agree that your
contribution is licensed under the same terms.

## Conduct and security

Everyone taking part follows the [Code of Conduct](CODE_OF_CONDUCT.md). Please don't report
security problems in public issues; see the [security policy](SECURITY.md).
