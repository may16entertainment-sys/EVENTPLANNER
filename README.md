# Build-Up Site Board

A site map for event build-up. Draw every area and line on your floor plan, say who installs it and in what order, and the board works out start dates, locks areas while each supplier works, flags clashes, and gives every supplier their own brief.

**How it's put together**
- **GitHub Pages** hosts the page.
- **Firebase** (Google) handles sign-in links and stores the plans.
- **You** register suppliers by email. A supplier signs in with a link sent to that email (no password) and sees **only their part**: their own work, dates, drop-off deadlines, site rules, and which areas are locked while others work (shown as "another supplier").
- Firebase's security rules enforce this on Google's side, so a supplier can't get at the rest of the plan even by digging into the page.

## Files

| File | What it does |
|---|---|
| `index.html` | The board. |
| `firestore.rules` | Who can read and write what. You paste this into Firebase. |
| `.nojekyll` | Tells GitHub Pages to publish the files as they are. |
| `README.md` | This guide. |

## Setup (about 20 minutes, once)

### 1. Create the Firebase project
1. Go to **console.firebase.google.com** and click **Create a project**. Name it, for example `site-board`. You can turn Google Analytics off.
2. Click **Upgrade** (bottom left) and choose the **Blaze** (pay-as-you-go) plan, then add a card.
   - The free plan only sends **5 sign-in emails a day**, which isn't enough. Blaze sends up to 25,000.
   - At your volume it should cost nothing, because Blaze includes the same free allowance.
   - Set a **budget alert** (for example $5) under **Usage and billing** so you're told if that ever changes.

### 2. Turn on email-link sign-in
1. **Build → Authentication → Get started**.
2. **Sign-in method → Email/Password**. Turn on **Email/Password** and **Email link (passwordless sign-in)**, then click **Save**.
3. **Settings → Authorized domains → Add domain**. Enter your GitHub Pages address, for example `yourname.github.io`.

### 3. Create the database
1. **Build → Firestore Database → Create database**.
2. Pick the location closest to you. For Jordan and the Gulf, `me-central2` (Dammam) or `europe-west1` both work. You can't change it later.
3. Start in **production mode**.
4. Open the **Rules** tab and replace everything with the contents of `firestore.rules`. Change `you@example.com` to **your** email (lowercase), then click **Publish**.

### 4. Connect the board
1. In Firebase, open **Project settings** (gear icon) → **Your apps** → the **</>** (Web) button. Name it `Site board` and register it. Firebase Hosting isn't needed.
2. Firebase shows a `firebaseConfig` block. Open `index.html` in a text editor, find `CONFIG` near the top of the script, and copy in `apiKey`, `authDomain`, `projectId` and `appId`.
3. In the same place, set `adminEmails:['your@email.com']`. Use the same email you put in the rules.

These values identify your Firebase project. They aren't secret and are safe in a public GitHub repository.

### 5. Publish and sign in
1. Upload `index.html` (and `firestore.rules`, `.nojekyll`, `README.md`) to your GitHub repository.
2. Open `https://yourname.github.io/site-board/` and enter your email.
3. Open the link Firebase emails you **on the same device and browser**. You're in as the planner.

### 6. Coming from the GitHub-only version?
1. Open **Event settings → Team → Import events from the GitHub version**. It copies your old events into Firebase.
2. Then **delete the `data` and `uploads` folders from your GitHub repository**. They're public, and the old version kept supplier details there.
3. Re-upload floor plans per event under **Event settings → Floor plan image**.

## Giving a supplier access

1. Open the **Vendors** tab and open the supplier.
2. Fill in their **Email**.
3. Leave **Can sign in and see their part** ticked. A few seconds after the plan saves, the card says **Can sign in now**.
4. Click **Email them a sign-in link**, or send them your board's address. They enter their email there and get a link.

What they see:
- **Your brief**: their work in order, with dates, what they wait for and who comes after them, where they may leave items and until when, site rules, and areas to stay out of.
- **Schedule**: only their work.
- **Map**: their areas in their colour; other work as "another supplier", with the same lock and restriction markings you see.
- Changes you make reach their page within seconds. They can't edit anything.

**See what they see** on a supplier card shows exactly their view, built the same way. **Back to planning** returns you to the full plan.

**Event settings → What suppliers see**: tick to show each supplier who works right before and after them, with names and contact details. Untick and everyone else is "another supplier". Your vendor notes are never shown.

To remove someone's access, untick **Can sign in** or clear their email. Their access goes with the next save.

**One email, several events:** if the same supplier is registered in several events, they get an event switcher after signing in.

**Other planners:** **Event settings → Team → Planners**. Planners see and edit everything.

## Signing in, in short
- Sign-in links work once and expire. Open the newest one.
- Open it on the device where you asked for it. On a different device, the board asks you to type your email once more.
- **Sign out** is at the top right.

## Events
The **Event** dropdown switches between events. **+ New event** starts from an empty plan, from the same vendors and questions, or from a full copy. A copy moves its dates with the new build-up day and sets every task back to not started.

**Event settings → Clear or delete:**
- **Clear everything** empties the plan but keeps the event name and dates.
- **Clear areas and tasks only** keeps vendors, questions and the floor plan.
- **Delete this event** also removes every supplier's access to it.

## Dates
Every task, step and job has a **Start** and **Finish** date box (Area tab, and in the job card).
- **Type a start date** and the task sits on that date: it shows *Fixed start*. Work that waits for it moves with it.
- **Type a finish date** and the number of days is worked out for you. Dates are whole days and both count: start Mon 19, finish Tue 20 = 2 days.
- **Leave the start empty** (or click **Make automatic**) and it starts as soon as the work before it is done.
- **A job's start date** moves its first step, and the other steps follow. A job's finish is set by its last step.
- **When you draw something new**, you can type a start and finish right in the form.
- **If you fix a start before the work it waits for is finished**, Clashes shows it, with one-click fixes: *Start on the first possible day*, or *Start automatically*.

## Select, edit and undo
- **Select** (top of the toolbar, or press **V**): click anything to see its details, and drag to move around the map. Nothing can move by accident.
- **Edit shapes** (press **E**): drag a shape to move it and drag corners to reshape it. Click **Done editing** to go back to Select.
- **Undo / Redo** (top bar, or **Ctrl+Z** / **Ctrl+Shift+Z**, ⌘ on Mac) step back through every change to the plan: drawing, moving, deleting, edits to tasks, vendors, types and lists. While drawing, Ctrl+Z removes the last point. Inside a text box, Ctrl+Z undoes your typing as usual.

## One task in several pieces (fences, walls…)
A fence or wall often runs in several separate places but is one job. To draw it that way:
1. Pick the item (for example **High fence**) and tick **Several pieces** in the bar under the map.
2. Draw the first run. Click the last point again (or **End this piece**) to end it. For areas, click the first corner to close each piece.
3. Draw the next run anywhere on the map, and repeat.
4. Click **Finish · N pieces**. You answer the questions once, and it becomes one task.

Later, in the Area tab:
- **+ Add a piece** draws another run for the same item.
- **Remove** deletes one piece.
- In **Edit shapes** you can drag the corners of every piece.

Length, area, clashes and supplier views all count every piece.

## Hiding things and sharing what's happening
The bar at the top left of the map has three buttons:
- **Show** chooses what's on the map:
  - everything, only the work happening on the day shown, or that work plus site rules and no-go areas;
  - hide finished work;
  - turn whole item types or suppliers off.
  The button shows how many items are hidden.
- **Hide on map** (Area tab) hides a single item. To bring it back, use **Show** → *Hidden one by one*.
- **Today** jumps to today and shows only what's happening.
- **Share view** makes a picture of the map exactly as you see it (same day, same things hidden), with a title, your message and the list of who's working that day. Send it from your phone's share menu, download it, or **Copy as text** for WhatsApp. Tick *whole site* to include the full plan instead of the part you're zoomed into.

What you hide only changes your own screen; it's remembered in your browser for each event.

## Seeing what's underneath
- **See-through** (Area tab, for areas): fades that area's fill so the floor plan and other items show through. The outline and label stay sharp.
- **Area fill** (slider under the map): fades the fill of every area at once. Your setting is remembered in your browser and doesn't change anyone else's view.

## Adding and editing item types
- **+ Add** next to *Areas* or *Lines* in the toolbar creates a new item type of that kind, ready to draw.
- **✎** next to any item (it appears when you point at it) renames it or changes its default work, days and restriction, or hides it.
- **All item types** under Setup lists everything, including hidden types.

## Drawing
- **Areas** (turf, stage, caravans, furniture, drop-off, no-go, and your own types): click each corner, then click the first corner again to close.
- **Lines** (walls, fences, cables, vehicle routes): click each point, then click the last point again or press **Enter**.
- **Snapping** pulls you to corners, edges, straight / 45° / square lines, and into line with other corners. Hold **Shift** to draw freely, or press **S** to turn snapping off.
- **Editing a shape:** select it, then drag corners, drag the small squares to add a corner, or right-click a corner to remove it. **Straighten lines** squares up a shape.
- **Set scale:** click both ends of something you know the length of and type the real distance. Areas then show m² and lines show metres.
- **Floor plan:** Event settings → Floor plan image (PNG or JPG). It's shrunk to fit Firebase, and only planners and that event's suppliers can load it.

## Jobs, steps and your own lists
- When you draw something, the board asks its questions: who, how many days, who goes before and after, who can work at the same time, next steps, power, and restrictions.
- Steps are subtasks of one job, each with its own vendor and days.
- Every dropdown ends with **+ Add…** and **✎ Edit list…**: item types, restrictions, task statuses, vendors, option questions and events.
- **Lists** in the toolbar shows them all in one place.

## Backups
**Event settings → Download backup** saves an event to a file; **Load from backup** brings one back. Firebase also keeps the data safe on Google's side.

## Using it without Firebase
If `CONFIG.firebase` is left empty, the board works in a single browser: it saves there only and has no sign-in. That's useful for trying it out.

## Settings in `index.html`
- `firebase`: your project's web settings
- `adminEmails`: planner emails (also put yours in `firestore.rules`)
- `saveDelaySeconds`: how long to wait after your last change before saving
- `snapPixels`, `snapDegrees`: how strongly drawing snaps
