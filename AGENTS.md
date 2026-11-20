# AGENTS.md: .github

The `voxolith/.github` repo holds:
- the organisation's profile README (`profile/README.md`), and the public copies of the lockup
  that READMEs embed (`profile/*.svg`);
- the community files that apply to every public repo: `CONTRIBUTING.md` (setup, checks,
  conventions, commits, pull requests, the AI policy), `CODE_OF_CONDUCT.md`, `SECURITY.md`,
  `PULL_REQUEST_TEMPLATE.md` and the issue forms in `ISSUE_TEMPLATE/`.

## Rules

- These files apply org-wide by living here. Don't copy them into other repos, where they would
  silently override these.
- Keep CONTRIBUTING's check table in step with what each repo's CI runs. Its job names are
  required checks on `main`: `typecheck` for renderer and engine, `check` for generators, `build`
  for the apps and the site.
- The lockups come from the private `branding` repo's `dist/`. Replace them from there; don't
  edit them here.
- The AI policy describes how the project itself is built (AI-assisted, reviewed and run by the
  maintainer, with `Co-Authored-By` trailers). Change it only when the practice changes.

## Working in the Voxolith repos

- **Layout.** Every Voxolith repo is checked out side by side under one bun workspace root, and
  depends on its siblings as `"workspace:*"`. Run `bun install` from that root, never inside a
  repo. [CONTRIBUTING](https://github.com/voxolith/.github/blob/main/CONTRIBUTING.md) lists
  which siblings each repo needs.
- **Toolchain: bun.** Install and run everything with bun, never npm. Node may be present (it is
  on GitHub's runners, where the apps' Vite builds run on it), but no script here needs it. It is
  TypeScript 7 and Vite 8; scripts run `tsc`, `vite` and `bun tools/x.ts`. Use current dependency
  versions.
- **`tsconfig.base.json` is byte-identical in every repo**, because consumers compile the
  renderer's and engine's sources under their own flags. Change it everywhere or nowhere.
- **WebGPU, not WebGL.** Dev servers are HTTPS (`@vitejs/plugin-basic-ssl`), because WebGPU needs a
  secure context. Checks cannot see pixels: anything that changes what is drawn must be looked
  at in a WebGPU browser, with a before/after screenshot in the pull request.
- **Docs live on the site** ([voxolith.github.io](https://voxolith.github.io/docs/), repo
  `voxolith.github.io`). READMEs stay short and link there. The API reference is generated from
  the sources, so doc comments are published content: every exported symbol has a `/** */`, and
  entry files open with `@packageDocumentation`.
- **Credit research.** When an idea comes from a paper, cite it (authors, title, venue, DOI) in
  the code comment, in the docs (the page's References and `/docs/credits/`) and in the commit
  body. Check the citation against the paper or DataCite; don't cite from memory.
- **Prose.** British spelling in prose and comments (`colour`, `normalise`); identifiers follow the
  web platform (`lightColor`). "Voxolith" is capitalised in prose; lowercase is only for the
  wordmark.
- **Commits.** History is linear and read as prose:
  - The subject says what is now true, in plain words: no `feat:` prefixes, no trailing full
    stop, about 70 characters at most.
  - The body says why, what it costs and what it deliberately does not do, wrapped at about 72
    columns.
  - One change per commit. AI-assisted commits keep their `Co-Authored-By` trailer.
  - Pull requests are squash-merged or rebased; there are no merge commits.
  - Don't push, tag or publish unless asked.
- **Community files** (CONTRIBUTING with the AI policy, CODE_OF_CONDUCT, SECURITY, templates) live
  once in `voxolith/.github` and apply org-wide; don't copy them into other repos.
- **CI's job names are required checks** on `main` (rulesets). Renaming a job breaks merging.
