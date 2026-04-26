# Afterglow

Track your mood daily. Understand yourself over time.

A minimal, private mood tracker that runs entirely in your browser — no app install required.

## Features

- **Daily grading** — rate each day A+ through F with an optional note
- **Calendar view** — monthly and year-at-a-glance heatmap
- **Statistics** — grade distribution, mood trends, streaks
- **Import** — upload data from the official spreadsheet template
- **Cloud sync** — sign in with Google or Facebook to sync across devices (via Supabase)
- **Guest mode** — use locally without an account; data stays in your browser

## Usage

Open the app at your GitHub Pages URL. No installation needed.

To use cloud sync, sign in with Google or Facebook. Any data you saved as a guest is automatically migrated to your account on first sign-in.

## Self-hosting

1. Fork this repo
2. Create a [Supabase](https://supabase.com) project and run the SQL below to set up the database
3. Enable Google and/or Facebook OAuth in Supabase → Authentication → Providers
4. Replace `YOUR_SUPABASE_URL` and `YOUR_SUPABASE_ANON_KEY` in `index.html` with your project credentials
5. Enable GitHub Pages (Settings → Pages → main branch)

### Database setup

```sql
create table entries (
  id uuid primary key default gen_random_uuid(),
  user_id uuid references auth.users not null,
  date text not null,
  grade text,
  note text,
  updated_at timestamptz default now(),
  unique(user_id, date)
);

alter table entries enable row level security;

create policy "Users manage own entries" on entries
  for all using (auth.uid() = user_id)
  with check (auth.uid() = user_id);
```

## Tech stack

- Vanilla HTML / CSS / JS — single file, no build step
- [Supabase](https://supabase.com) for auth and database
- [Mixpanel](https://mixpanel.com) for analytics (optional)

## License

MIT — see [LICENSE](LICENSE)
