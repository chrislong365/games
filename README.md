# games.chrislong365.com

The front door for Chris Long's browser games, served free by GitHub Pages from the `main` branch of this repo.

```
/                  directory page (index.html), reads games.json
/thumbs/           one 640x360 picture per game
/<game>/           each game's built files, published by that game's own repo
```

This repo is public, but it only holds **built** games. Each game's source stays in its own private repo, and a merge to that repo's `main` builds the game and replaces its folder here. Don't edit a game's folder by hand: the next deploy overwrites it.

## Games

| Path | Source repo (private) |
| --- | --- |
| `/human-factor/` | `chrislong365/the-human-factor` |

## Adding a game

1. Pick a lowercase folder name, e.g. `space-race`. Pages URLs are case-sensitive, so keep them lowercase with dashes.
2. In the game's repo, use the deploy workflow from `the-human-factor` (`.github/workflows/deploy.yml`) and set its `GAME_PATH` to the folder name.
3. Give the game's repo its own publishing key (one per game, so each can be revoked alone):
   ```sh
   ssh-keygen -q -t ed25519 -N "" -C "<game-repo> -> games deploy" -f /tmp/k
   gh repo deploy-key add /tmp/k.pub -R chrislong365/games --allow-write -t "deploy from <game-repo> (Actions)"
   gh secret set GAMES_DEPLOY_KEY -R chrislong365/<game-repo> < /tmp/k
   rm /tmp/k /tmp/k.pub
   ```
4. Add a picture to `thumbs/` and an entry to `games.json`.

The game's build must use relative asset paths (Vite: `base: "./"`), since it is served from a sub-folder.

## Domain

`CNAME` holds `games.chrislong365.com`. DNS: a `CNAME` record for `games` pointing to `chrislong365.github.io`. HTTPS is enforced in **Settings → Pages**.
