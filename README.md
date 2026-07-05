# Science Laboratory — Booking Board (GitHub version)

Teachers request a lab slot through a GitHub Issue. Nothing shows on the
public board until you add the `approved` label — at that point a GitHub
Action writes it into `schedule.json` automatically and the board updates.

Teachers never touch any file in this repo. Their only way to contribute is
opening an Issue with the request form, so there's no way for them to edit
anything else.

## One-time setup

1. **Create a repository.** On GitHub, click **New repository**. Make it
   **Public** (Issues need to be open to anyone, and Pages needs a public
   repo unless you're on a paid GitHub plan).

2. **Add these files**, keeping the exact folder structure:
   ```
   index.html
   schedule.json
   .github/ISSUE_TEMPLATE/book-slot.yml
   .github/workflows/approve-booking.yml
   ```
   Easiest way with no git needed: in your repo, click **Add file → Create
   new file**, then type the full path (e.g.
   `.github/workflows/approve-booking.yml`) into the filename box — GitHub
   creates the folders for you. Paste the file's contents in, then commit.
   Repeat for each file.

3. **Create the `approved` label.** Go to your repo's **Issues** tab →
   **Labels** → **New label**. Name it exactly `approved` (any color). This
   is the only label the automation checks for.

4. **Turn on Pages.** Repo **Settings → Pages** → under "Build and
   deployment," set Source to **Deploy from a branch**, branch **main**,
   folder **/(root)** → **Save**. GitHub gives you a URL like
   `https://yourusername.github.io/your-repo-name/` — that's the link you
   share with teachers.

5. **Give Actions permission to save changes.** Repo **Settings → Actions →
   General**, scroll to "Workflow permissions," select **Read and write
   permissions** → **Save**. Without this, the approval step can't commit
   the update.

That's the whole setup. Steps 3 and 5 are the two easiest to forget — if
approving a request doesn't seem to do anything, check those first.

## Day-to-day use

**Teachers:**
1. Open the board link.
2. Tap **Request a slot** (or tap directly on an open slot to pre-fill the
   day/period).
3. Fill in name, subject/activity, section, submit. It shows on the board
   as **pending** right away.

**You (approving):**
1. Check the repo's **Issues** tab for new requests.
2. If the slot's free and the request is fine, add the `approved` label.
   Within about 30 seconds the Action updates `schedule.json`, comments on
   the issue, and closes it. Refresh the board to see it as **booked**.
3. If a request conflicts with an already-booked slot, the Action leaves a
   comment on the issue explaining that instead of applying it — remove the
   label, and either close the issue or ask the teacher to pick another
   slot.
4. To decline a request outright, just close the issue without labeling it
   — nothing gets written anywhere.

## Making corrections directly

To fix a typo, cancel a confirmed booking, or clear the whole week, open
`schedule.json` in the repo, click the pencil (edit) icon, make the change,
and commit directly to `main`. The board picks it up on next page load.

Each entry is keyed by `Day-PeriodNumber`, for example:
```json
{
  "Mon-3": { "teacher": "Sin Datu", "subject": "General Science – Mineral ID", "section": "STEM 3", "issue": 12 }
}
```
Delete an entry (or the whole file's contents, leaving `{}`) to free those
slots back up.

## Notes

- The period times (7:30 AM–3:50 PM, 9 periods, lunch after 5th period) are
  set in both `index.html` and `book-slot.yml`. If your school's schedule is
  different, tell whoever's helping you edit this and both files should be
  updated together so they stay in sync.
- The board auto-detects your repo name from the page URL. If you ever move
  it to a custom domain, set `CONFIG.repoPath` near the top of the
  `<script>` block in `index.html` to `"yourusername/your-repo-name"`.
- Two teachers requesting the exact same slot at the same time is handled:
  whichever request you approve first wins, and the second gets a warning
  comment automatically instead of silently overwriting the first.
