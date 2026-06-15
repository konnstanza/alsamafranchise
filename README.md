# Alsama Team Poll

A live 4-question poll built with vanilla HTML/CSS/JS and Supabase. No build step required.

---

## 1. Set up Supabase

### Create the table

In your Supabase project, open the **SQL Editor** and run:

```sql
create table responses (
  id uuid primary key default gen_random_uuid(),
  question_number int not null,
  answer text not null,
  created_at timestamptz default now()
);
```

### Enable Realtime

1. Go to **Database → Replication** in the Supabase dashboard.
2. Under **Source**, enable the `responses` table.

Or run:

```sql
alter publication supabase_realtime add table responses;
```

---

## 2. Add your Supabase credentials

Open **both** `index.html` and `results.html`. Near the top of each `<script>` block you’ll find:

```javascript
const SUPABASE_URL      = 'YOUR_SUPABASE_URL';
const SUPABASE_ANON_KEY = 'YOUR_SUPABASE_ANON_KEY';
```

Replace the placeholder strings with your project’s URL and `anon` key, found in **Project Settings → API**.

---

## 3. Run locally

Open `index.html` directly in a browser — no server needed.

Open `results.html` in a separate tab or window for the live admin view.

---

## 4. Deploy to Vercel

1. Push this repo to GitHub (if not already there).
2. Go to [vercel.com](https://vercel.com) and click **Add New → Project**.
3. Import the GitHub repository.
4. Leave all settings at their defaults — Vercel will detect a static site automatically.
5. Click **Deploy**.

Vercel serves `index.html` at the root URL and `results.html` at `/results.html`.

> **Tip:** You can also set `SUPABASE_URL` and `SUPABASE_ANON_KEY` as Vercel Environment Variables and inject them at build time if you prefer not to commit them, but for a quick internal tool the inline approach works fine.

---

## Files

| File | Purpose |
|------|--------|
| `index.html` | The poll — one question at a time, live tally after each answer |
| `results.html` | Admin view — all four questions with live tallies |
| `README.md` | This file |

---

## How it works

- Answers are written to the `responses` table in Supabase.
- `localStorage` tracks which questions a respondent has answered, so reloading mid-poll resumes from the correct place.
- Supabase Realtime streams `INSERT` events to update bar charts without polling.
- The `results.html` page has no password — keep the URL obscure or restrict access via Vercel password protection if needed.
