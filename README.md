# Cocky Blockers Rotation Lab — Standalone PWA

A mobile-friendly, offline-capable volleyball starting-rotation simulator. No server, framework, paid service, or API key is required.

## Publish to GitHub Pages (about 5–10 minutes)

1. Sign in to [github.com](https://github.com) and create a **public** repository, for example `rotation-lab`.
2. Extract the ZIP. Upload **the files inside** the `cocky-blockers-rotation-lab` folder (including `index.html`, `app.js`, `styles.css`, `sw.js`, `manifest.webmanifest`, and the `icons` directory) into the **root of the repository**. On GitHub, use **Add file → Upload files → Commit changes**.
3. Go to **Settings → Pages**. Under **Build and deployment**, choose **Deploy from a branch**. Select branch `main` and folder `/ (root)`, then click **Save**.
4. After deployment, the URL is usually `https://YOUR-USERNAME.github.io/rotation-lab/`. GitHub Pages may take a few minutes to deploy.
5. Open the URL in a browser. On iPhone, use **Safari → Share → Add to Home Screen**. On Android, use the browser's **Install app** or **Add to Home screen** option.

**Important:** GitHub Pages is public hosting. Anyone with the URL can view the app and its built-in baseline stats. Personal saved scenarios are kept locally in each user's browser, not uploaded to GitHub or shared with other users.

## Test locally

The HTML may be opened directly for a basic preview, but offline caching and installation require an HTTP(S) origin. For local testing with Python:

```bash
cd cocky-blockers-rotation-lab
python3 -m http.server 8000
```

Then visit `http://localhost:8000` on your computer. GitHub Pages automatically provides HTTPS.

## How to use

- Choose whether your team **serves** or **receives** first.
- Choose the first server. When **receiving first**, this is the first server **after the opening side-out**, not the current receiving rotation.
- Rename players, reorder the circular serving order, and adjust the percentage sliders.
- Click **Run simulation** to compare all six starting positions.
- Save a scenario to this device, or export/import JSON to move saved scenarios between devices.
- Export results to CSV for further analysis.

## Simulation model

- A serving rally uses the current server's **point scoring %**. A missed serve is a loss, and point scoring is conditioned on a legal serve so errors are not counted twice.
- When we **lose serve**, we stay in the same positional rotation while receiving.
- When we **win on receive**, we rotate to the next player in the six-person serving order and that player serves.
- Sets end at the target score with a two-point margin. Each candidate starter is simulated with the same deterministic random seed.
- Service turns and attempts are **outputs** of the simulation, not preset weights.
- **Earned side-out, FBSO and earned FBSO** are editable diagnostics. They are not independent extra point-winning events.
- Point-scoring percentage must be less than or equal to serve-in percentage for the conditional serve model to be valid.
- If a set has not finished after 600 rallies, it is excluded from the estimated win probability and averages; completed-set counts are shown in Results.

## Data and interpretation

Baseline point scoring, side-out, earned side-out, FBSO and earned FBSO use combined 2026 Seattle (15 sets) and Portland (14 sets) counts. Baseline serve-in values are **provisional** and should be checked against full serving records before making decisions.

This model does **not** simulate opponent rotations, player-specific attacks, score-dependent performance, or statistical uncertainty in historical rates. The displayed uncertainty describes Monte Carlo sampling only, not uncertainty in the input estimates. Small differences should not be treated as definitive strategic advantages.

## Update the app

Edit `app.js`, `styles.css`, or `index.html`, commit the updated files to GitHub, and GitHub Pages will redeploy. When changing offline-cached assets, increment the `CACHE` version in `sw.js` so installed clients fetch the latest app files.

## Files

- `index.html` — interface and app structure
- `styles.css` — responsive dark styling
- `app.js` — UI, storage, scenarios, and Monte Carlo simulator
- `manifest.webmanifest` — installable app metadata
- `sw.js` — offline caching
- `icons/` — app icons
