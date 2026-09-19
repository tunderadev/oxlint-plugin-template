# Using this template

This repo is a GitHub template for Oxlint plugins. It is never published to npm itself.

1. On GitHub, press "Use this template" and name the new repo after the npm package, like `oxlint-plugin-llm`.
2. Clone it, then run the init script once:

   ```sh
   pnpm install
   pnpm init-template oxlint-plugin-llm "Lint rules for code that calls LLM SDKs." tunderadev
   ```

   It renames the package, the short name in `src/index.ts`, the README, CODEOWNERS, and SECURITY, drops `private`, and deletes itself and this file.

3. Replace `assets/logo.svg` with the project's mark.
4. Keep or delete the `no-fixme` example rule. `pnpm new-rule <name>` adds real ones.
5. Run the checks: `pnpm check && pnpm test && pnpm build && pnpm dogfood`.
6. Set up publishing, once:
   - Enable 2FA on npm (passkey; npm no longer offers authenticator apps).
   - Log in with `npm login` from a directory outside the repo, then run `pnpm publish --access public` inside it to publish a 0.0.1. npm refuses every command inside the repo because `devEngines` pins pnpm, so registry commands here go through pnpm. It asks for the one-time code with `--otp` if your 2FA needs one.
   - On npmjs.com, package settings, Trusted Publisher, GitHub Actions: your user, the repo name, workflow `publish.yml`, environment `release`. Allow `npm publish` for direct publishing.
   - Account settings, Publishing access: "Require two-factor authentication and disallow tokens".
   - On GitHub, create the `release` environment (Settings, Environments). No secrets go in it.
7. Run `pnpm setup-repo`. It turns on auto-merge and adds a ruleset so nothing reaches `main` without green CI. That is what lets Renovate merge its own PRs. Admins bypass the ruleset, so `pnpm release` still pushes straight to `main`.
8. Install the GitHub Apps, or delete the config for the ones you skip:
   - Renovate (Mend). Dependency PRs merge themselves once CI is green. Majors of runtime dependencies wait for you. Config: `.github/renovate.json`.
   - pkg.pr.new, for installable PR builds. Config: `preview.yml`.
   - CodeRabbit, free on public repos, reviews every PR and approves once its comments are resolved. Config: `.coderabbit.yaml`.
   - Macroscope, free for non-commercial open source after applying at macroscope.com/open-source. It reviews for correctness and its Approvability check approves low-risk PRs. Turn Approvability on under Settings, Repos, and add `renovate[bot]` under Skip PRs by Author. Config: `.macroscope/approvability.md`.
9. Enable Discussions and private vulnerability reporting in the repo settings. Create the labels: `bug`, `false positive`, `false negative`, `new rule`, `enhancement`, `good first issue`, `help wanted`, `documentation`.

Releasing after that is `pnpm release`.
