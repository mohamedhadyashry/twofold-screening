# Twofold — free team screening for systematic reviews

A free alternative to Rayyan for title/abstract screening with a team.
Everyone signs in with Google; the lead imports the records, removes duplicates,
splits the papers between reviewers, and resolves conflicts.

**Open it:** https://mohamedhadyashry.github.io/twofold-screening/

## What it does

- **Google sign-in** for everyone. A review is visible only to the emails the lead adds.
- **CSV import** from PubMed, Scopus, Web of Science, Embase, Rayyan, Zotero or EndNote, with column mapping.
  Several files are merged.
- **Duplicates**: same DOI, same PMID, identical title or a similar title (spelling variants, small typos).
  The lead keeps one record of each group or marks them as different papers. Every decision can be undone.
- **Any team size, papers split between reviewers.** Choose who screens and how many reviewers each paper needs.
  Example: 100 papers, 4 reviewers, 2 per paper → papers #1–50 go to A + B and #51–100 to C + D.
  Papers can be shuffled first, ranges can be assigned by hand, and papers added later are shared out
  without moving the ones already assigned. If the team doesn't split into equal groups, pairings rotate
  so everyone gets the same workload.
- **Blind screening**: each reviewer sees only their own papers and decisions. Include / Maybe / Exclude,
  exclusion reasons, notes, keyword highlighting, keyboard shortcuts (`I` `E` `M` `U`, `J`/`K`). Saved automatically.
- **Compare** (lead): progress per reviewer while blind; after the lead turns blind mode off, agreement and
  Cohen's kappa for each reviewer pair, and every conflict to settle with a final Include or Exclude.
- **Export**: results CSV (who screened each paper, every decision, reason and note, final decision),
  final includes, removed duplicates, and PRISMA flow numbers.

## How a review runs

1. The lead opens the site, signs in and creates a review.
2. **Records**: import the CSV files. **Duplicates**: resolve the duplicate groups.
3. **Team**: add each reviewer's Google email and send them the link shown there. Then assign the papers.
4. Each reviewer signs in with that Google account and screens their papers on the **Screen** tab.
5. **Compare**: when everyone is done, turn blind mode off, check kappa and settle the conflicts.
6. **Export**: download the results and the PRISMA numbers.

## Where the data lives

In the `twofold-screening` Firebase project (Google Firestore, free Spark plan). Access is enforced on the
server by `firestore.rules`:

- only people the lead added can open a review;
- only the lead sees the full record set, the duplicate decisions, the assignment plan and the final decisions;
- a reviewer can read only the papers assigned to them and only their own decisions;
- the lead can read reviewers' decisions only after turning blind mode off.

The free plan allows 50,000 reads and 20,000 writes a day, enough for a team of 10 screening thousands of papers.

## One-time Firebase setup (already done for this project)

1. Firebase console → **Authentication** → Sign-in method → enable **Google**.
   Settings → Authorized domains → add `mohamedhadyashry.github.io`.
2. **Firestore Database** → create the database in production mode.
3. Firestore Database → **Rules** → replace everything with the contents of `firestore.rules` → **Publish**.
   Do this again whenever `firestore.rules` changes.
4. Project settings → Your apps → web app → copy the config into `firebase-config.js`.

## Files

- `index.html`: the whole app.
- `firebase-config.js`: the Firebase project settings (public by design).
- `firestore.rules`: the database access rules (paste into the Firebase console).
