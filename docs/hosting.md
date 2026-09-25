# Hosting

A Claude artifact link needs a Claude account to open, even when it's shared. To let anyone watch a battle, put `index.html` on a static web host.

The standalone file sets `window.LUCKY_STANDALONE = true` in its `<head>`. That makes the share buttons build links to the site's own address rather than the Claude artifact.

## Option 1: Netlify Drop (quickest)

1. Go to https://app.netlify.com/drop.
2. Drag in `index.html`, or this whole folder. Keep the file name as `index.html`.
3. Netlify gives you a web address. To keep the site running, check whether you need to sign up for a free account.
4. Open the address, go to Space battle, press **New battle**, then **Send on WhatsApp**.

## Option 2: GitHub Pages

1. Create a public repository, for example `space-battle`.
2. Add `index.html` at the top level of the repository.
3. In the repository, go to **Settings → Pages**, set the source to the `main` branch and the root folder, then save.
4. The site appears at `https://<your-username>.github.io/space-battle/`.

## After any change

1. Upload the new `index.html` to the host.
2. Any battle links already sent keep working, as long as the game rules haven't changed.
3. If the simulation code has changed (anything in `step()` or `setup()`), old links will play out differently. Send fresh links.

## Keeping the two versions in step

- The Claude artifact and `index.html` are built from the same page code.
- The standalone file adds a normal HTML `<head>` (charset, viewport, share preview text) and the `LUCKY_STANDALONE` flag.
- When you make a change, update both. Otherwise links from one version won't match fights on the other.
