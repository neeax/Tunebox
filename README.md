# Tunebox

A music app that searches free, Creative Commons music online (via Jamendo), downloads it to your device and plays it offline. It also plays your own audio files and embeds your Spotify playlists.

## 1. Get a Jamendo client ID (free)

1. Go to https://developer.jamendo.com and create an account.
2. Create an application. Copy the **client ID**.
3. Open Tunebox, go to **Discover**, tap **Add client ID** and paste it.

To skip that step, paste the ID into `DEFAULT_JAMENDO_KEY` near the top of the script in `index.html`.

## 2. Put it online (needs HTTPS)

Service workers and installing to the home screen only work over HTTPS (or `localhost`). Any static host works:

- **Netlify Drop:** drag this whole folder onto https://app.netlify.com/drop
- **GitHub Pages:** push the folder to a repo, then Settings > Pages
- **Vercel / Cloudflare Pages:** import the repo or upload the folder

To test on your computer first: `python3 -m http.server 8000` in this folder, then open http://localhost:8000

## 3. Install it like an app

- **iPhone (Safari):** Share > Add to Home Screen
- **Android (Chrome):** menu > Install app (or Settings in Tunebox > Install app)
- **Desktop (Chrome/Edge):** install icon in the address bar

## How offline works

- The app itself is cached by `sw.js`, so it opens without internet.
- Downloaded songs and cover art are stored in the browser's IndexedDB. They stay on that device and survive restarts. Tunebox asks the browser to keep them persistent.
- Search needs internet. Library, playlists and likes work offline.

## Things to know

- **Downloads can fail on some networks.** Tunebox fetches the track file directly from Jamendo. If a browser blocks that request (a CORS restriction), the download button shows an error. The fix is a small proxy (for example a Cloudflare Worker) that forwards the request and adds CORS headers; point `fetchBlob` in `index.html` at it.
- **Storage limits.** iOS Safari may clear site data after weeks without use unless the app is installed to the Home Screen. Installing it helps a lot.
- **Licenses.** Jamendo tracks are Creative Commons; each track has its own terms. Use the ⋮ menu > View license on a downloaded song. Some licenses forbid commercial use.
- **Spotify.** Spotify songs can't be downloaded into this app. The Spotify tab only embeds Spotify's own player.
- **Backups.** Songs are not synced between devices. Clearing browser data deletes them.

## Files

| File | Purpose |
| --- | --- |
| `index.html` | The whole app (UI, search, downloads, player) |
| `sw.js` | Service worker that caches the app for offline use |
| `manifest.json` | Makes the app installable |
| `icon-*.png`, `apple-touch-icon.png` | App icons |
