# CLAUDE.md

## Repository

Desired state for `restate-digitalocean`: Restate and its TypeScript reference
workflow application on one DigitalOcean Droplet in Amsterdam, published at
`https://bigconfig.website` through Cloudflare and Caddy. Behavior lives in
`../restate`.

Tracked source is `colors.yml`, toolchain and documentation, the installed
Package Skill, lockfile, and a root launcher copied from its payload.
`.colors/` is generated private state and `.envrc.private` contains credentials;
never read, edit or commit either.

## Commands

```sh
./green build
./green create --dry-run
./green create
./green delete
```

Build and dry-run require no credentials. Never export `COLORS_PAR_PROFILE`.
Keep `compute-prevent-destroy: true`; deletion requires separate authorization
and a one-run `COLORS_PAR_COMPUTE_PREVENT_DESTROY=false` override.

The root `green` is a copy, not a symlink. After a Package Skill update run
`npx skills update -p -y` and copy
`.agents/skills/package-restate-green/green` over it. Never hand-edit its SHA.

## Verification

Real create performs HTTPS health, duplicate start, activity retry and full
Droplet reboot-during-durable-delay checks. Operational checks are documented in
README.md. Restate 8080/9070/5122 must remain private.

## Documentation

`index.html` is this repository's landing page and carries two analytics tags:
GA4 measurement ID `G-4VKP1WY4QJ`, whose explicit `page_title` must exactly
equal the decoded HTML `<title>` and stay distinct and stable so one Analytics
property can separate repositories, and the self-hosted Rybbit snippet
`<script src="https://rybbit.getcolors.ai/api/script.js" data-site-id="9fb9c41a6d49" defer></script>`,
which shares one site ID across every page because `getcolors.github.io/<repo>/`
paths already encode the repository. Never add one tag without the other.

## Git

Work on the current branch. Do not commit or push unless explicitly authorized.
