# Puwido Media Share

SirPuwido's video site, hosted on GitHub Pages:
**https://puwido.github.io/PuwidoMediaShare/**

The public site is just the `docs/` folder. It's a static site (`index.html`, `app.js`, `style.css`) that reads its video list from `videos.json`. The site itself has no upload. Videos are posted with `videos.js`, which commits and pushes them, and GitHub Pages republishes the site.

## Post videos

```
node videos.js add "C:\path\to\video.mp4" --title "My video" --desc "Optional description" --thumb "C:\path\to\thumb.jpg"
node videos.js list
node videos.js edit <id> --title "New title"
node videos.js remove <id>
node videos.js clean
```

- Videos up to 95 MB go in `docs/media/` in the repo.
- Bigger videos (or any added with `--release`) are uploaded as GitHub Release assets, up to 2 GB each.
- Every video gets a share link like `https://puwido.github.io/PuwidoMediaShare/v/<id>/`, which shows a preview in Discord/X and opens the player.
- Add `--no-push` to skip committing and pushing.

## Privacy and safety

- **Metadata is stripped from every video and image before it's published** (`tools/strip-metadata.js`): GPS/location, device make and model, Windows/Game Bar info, recording software, capture dates, XMP/EXIF and embedded text. The video and audio themselves aren't re-encoded or changed. If a file can't be cleaned, it isn't published.
- Published files get random names, so original file names (which often contain dates or places) never appear online.
- `node videos.js clean` re-strips everything already on the site.
- The site has a strict Content-Security-Policy: only its own scripts and styles run, nothing loads from third parties, no `eval`, and videos can only come from `docs/media/` or this repo's Releases.
- Trusted Types are enforced, so the page can't be tricked into injecting HTML or script outside the site's one vetted path.
- The script and stylesheet carry Subresource Integrity hashes: if either file is altered, the browser refuses to run it.
- The site refuses to display inside another site's frame (clickjacking protection).
- The `main` branch is protected against force-pushes and deletion, and GitHub secret scanning, push protection and Dependabot alerts are on.
- No trackers, analytics, cookies or external fonts.
- Commits use a GitHub noreply email address.

Metadata stripping can't remove what's *in* the video picture or sound itself (usernames on screen, your voice, a window title in a screen recording). Check those before posting.

## Code

All JavaScript in this repo is obfuscated in two layers: control-flow flattening, dead-code injection, every string RC4-encrypted, all names replaced, and self-defending code that breaks if it's reformatted. The website script is also domain-locked, so a copy won't run on any other site. HTML and CSS are minified. The readable source lives only in a local `src/` folder, which isn't committed, and a git pre-commit hook blocks any commit that contains readable code. To change the code, edit the files in `src/`, then rebuild:

```
node src/build.js
```

## Preview locally

```
npm install
npm run preview
```

Then open http://localhost:3000 (only reachable from this computer).

## Profile

- Name, handle, bio and site URL: `docs/profile.json`
- Avatar: `docs/avatar.webp`
- Banner: `docs/banner.webp` (3:1 image, e.g. 1500x500)
