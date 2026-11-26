# DuelKing public assets

Images served to the open internet, published by Cloudflare Pages at
`https://assets.duelking.dev/`. So `email/crown.png` in this repository is
`https://assets.duelking.dev/email/crown.png`.

Nothing in here is private, and nothing private may ever be added. There is
no application code, no configuration and no credentials. That is the whole
point of the repository existing separately, and it is the rule to apply to
anything anyone proposes adding.

## Why this is not in the application repository

The platform sits behind Cloudflare Access, which requires a login on every
path of `duelking.dev`. A mark in a confirmation email has to be fetchable
by a stranger's mail client, so serving it from the application would mean
carving a public hole into a site whose rule is otherwise that everything
requires a login. A hole like that has to be scoped correctly, has to keep
being scoped correctly, and has to be remembered by everyone who later
touches the folder it exposes.

A separate hostname removes the question rather than answering it. The
Access application is scoped to the bare `duelking.dev` hostname, with no
subdomain and no path wildcard, so `assets.duelking.dev` was never covered
by it and needs no bypass rule.

A separate repository extends the same reasoning one step further. A build
that publishes the wrong directory would expose whatever sits in that
directory, so the directory holds images and nothing else. The worst
outcome available here is that a picture becomes public, which it already
is.

Private evidence, match demos included, belongs in object storage behind
signed URLs. Never here.

## How it deploys

A Cloudflare Pages project builds this repository with no build command and
the repository root as the build output directory, so every merge to `main`
publishes the tree as it stands. Pages reads `_headers` from that root.

The application never links to this repository or hardcodes a path. It knows
one environment variable, `EMAIL_LOGO_URL`, set in
`/srv/duelking/shared/env`. Unset, the email falls back to a typographic
header, which is also what recipients who block remote images see.

## When the platform moves to duelking.com

Add `assets.duelking.com` as a second custom domain on the same Pages
project and point `EMAIL_LOGO_URL` at it. Nothing in the application
changes, because the application only ever knows the URL.
