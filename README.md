# Build-Up Site Board

A site map for event build-up. Draw every area and line on your floor plan, say who installs it and in what order, and the board works out start dates, locks areas while each vendor works, flags clashes, and gives every vendor their own brief and link.

It runs entirely on GitHub, with no server and nothing to pay. The plan is saved as a file in this repository (`data/plan.json`), and every save is a commit, so you get a full history of every change.

## Files

| File | What it does |
|---|---|
| `index.html` | The board itself. |
| `.nojekyll` | Tells GitHub Pages to publish the files as they are. Keep it. |
| `README.md` | This guide. |

`data/plan.json` and the `uploads/` folder are created automatically the first time you save or upload a floor plan.

## Set it up (about 10 minutes, once)

### 1. Create the repository
1. On GitHub, click **New repository**.
2. Name it, for example `site-board`.
3. Choose **Public**. GitHub Pages is free for public repositories; a private repository needs a paid GitHub plan.
4. Click **Create repository**.
5. Click **uploading an existing file** and drag in `index.html`, `.nojekyll` and `README.md`. Then click **Commit changes**.

> `.nojekyll` starts with a dot, so some computers hide it. If you can't see it, skip it; the board still works.

### 2. Turn on GitHub Pages
1. In the repository, go to **Settings → Pages**.
2. Under **Build and deployment**, set Source to **Deploy from a branch**.
3. Set Branch to **main** and the folder to **/ (root)**, then click **Save**.
4. After a minute the board is live at `https://YOUR-USERNAME.github.io/site-board/`.

### 3. Make your planner token
This is your password for editing. It stays in your browser and is never saved in the repository.

1. Go to **github.com → your photo → Settings → Developer settings → Personal access tokens → Fine-grained tokens → Generate new token**.
2. Name it `Site board` and choose an expiry date (for example 1 year).
3. Under **Repository access**, choose **Only select repositories** and pick `site-board`.
4. Under **Permissions → Repository permissions**, set **Contents** to **Read and write**. Leave everything else as it is.
5. Click **Generate token** and copy it. It starts with `github_pat_`.

### 4. Sign in
Open your board, click **Planner sign-in** and paste the token. That browser stays signed in. Do the same on any other computer or phone you plan from.

When the token expires, make a new one and sign in again.

## Sharing with vendors

Go to **Vendors**, open a vendor, and click **Copy vendor link**. It looks like
`https://YOUR-USERNAME.github.io/site-board/?vendor=v_ab12cd34`.

- Each vendor sees only their own steps, deadlines, drop-off areas and site rules, plus the contacts of the vendors just before and after them.
- They can't change anything, and they don't need a GitHub account.
- Their page checks for updates every minute.
- **Copy brief** gives the same information as text for WhatsApp or email.

**Privacy:** in a public repository, anyone can find and read the plan, including vendor names, phone numbers and emails. Only keep information there you're happy to share. If that's a problem, use a private repository; GitHub Pages for private repositories needs a paid GitHub plan.

## Using your own domain

If you point your own domain at GitHub Pages (**Settings → Pages → Custom domain**), open `index.html` and fill in the two lines near the top of the script:

```js
owner:'your-github-username',
repo:'site-board',
```

On a `github.io` address you can leave them empty; the board works them out from the address.

## Drawing

- **Areas** (turf, stage, caravans, furniture, drop-off, no-go): click each corner. Click the first corner again to close the shape.
- **Lines** (walls, high and low fences, electrical cables, vehicle routes): click each point. Click the last point again, press **Enter**, or use **Finish line**.
- **Snapping** pulls you to existing corners, onto existing edges, into straight / 45° / square lines, and into line with other corners. A cyan marker shows what you snapped to. Hold **Shift** to draw freely, or press **S** to turn snapping off.
- **Fix a shape:** select it, then drag a corner to move it, drag a small square on an edge to add a corner, or right-click a corner to remove it. **Straighten lines** in the Area tab squares up sides that are nearly straight.
- **Set scale:** click both ends of something you know the length of and type the real distance. Every area then shows m² and every line its length in metres.
- **Floor plan:** Event settings → Floor plan image. Export your AutoCAD drawing as PNG or JPG. It's saved into the repository's `uploads/` folder.

## Jobs, steps and vendors

When you draw something, the board asks its questions: who installs it, how many days, who must finish first, who comes after, who can work at the same time, what the next steps are, whether it needs power, and what's not allowed once it's installed.

**Steps are subtasks of one job.** For example, *Main stage works*: Build structure → Stairs → Paint → Furniture → Numbering.

- Each step has its own vendor and number of days.
- Tick **same time as the step before** for steps that can run together.
- In the Area tab you can add, reorder or remove steps, or split a plain task into steps.
- Other jobs can wait for one step or for a whole job, meaning all of its steps.

**Vendors** is your directory: company, what they do, contact person, phone, email and notes, plus every step assigned to them in order.

**Edit questions** (left side) lets you reword, reorder, remove or add questions for each item type. Tick **Remember these answers** and the next turf or stage starts pre-filled.

## History, backups and the next event

- Every save is a commit. To see or restore an older version, open `data/plan.json` on GitHub and click **History**.
- **Event settings → Download backup** saves the whole plan to your computer.
- **For the next event:** download a backup of the current one, then use **Clear all areas and tasks**. Your vendors and questions stay; upload the new floor plan and redraw.

## Good to know

- Saving waits about 3 seconds after your last change, so a burst of edits becomes one commit. Don't close the tab while the top bar says **Saving to GitHub…**.
- If you edit on two devices at once, the second save is refused rather than overwriting the first. The board reloads the latest version and tells you to redo your last change.
- GitHub lets each signed-out visitor make 60 requests an hour, which covers a vendor page that's open all day. If the limit is reached, the page falls back to the copy GitHub Pages serves, which can be a few minutes behind.
- Opening `index.html` straight from your computer works as a private sandbox. It saves in that browser only and has no vendor links.

## Settings in the file

At the top of the script in `index.html`:

- `owner`, `repo`: only needed on a custom domain
- `branch`: the branch Pages publishes from (default `main`)
- `pollSeconds`: how often vendor pages check for updates
- `saveDelaySeconds`: how long to wait after your last change before saving
- `snapPixels`, `snapDegrees`: how strongly drawing snaps
