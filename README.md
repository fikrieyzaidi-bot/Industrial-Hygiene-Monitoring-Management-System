# Corporate IH Control Center — setup (about 10 minutes)

## 1. Supabase
1. Create a project at supabase.com (or use your existing one).
2. **SQL Editor → New query** → paste all of `setup.sql` → **Run**.
3. **Authentication → Sign In / Providers → Email**: turn **off** "Allow new users to sign up"
   (so only people you create can log in).
4. **Authentication → Users → Add user → Create new user** for yourself:
   - Email: `fikri01@ihcc.app` (this is what username **Fikri01** turns into)
   - Choose a NEW password, tick **Auto Confirm User**.
5. **Project Settings → API**: copy the **Project URL** and **anon public** key.

## 2. Connect the app
// ===== IH Control Center configuration =====
// Get these from Supabase: Project Settings -> API.
// The anon key is meant to be public; the database rules in setup.sql protect the data.
window.IHCC_CONFIG = {
  SUPABASE_URL: "https://yfinqralinnznqmcbkcb.supabase.co",
  SUPABASE_ANON_KEY: "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6InlmaW5xcmFsaW5uem5xbWNia2NiIiwicm9sZSI6ImFub24iLCJpYXQiOjE3OTMzNzA1MTMsImV4cCI6MjEwODk0NjUxM30.kUjAU42apHminiDJ6WEfFK2Y1j-cyjtdN_uL6UoWg70",
  // Lets people log in with a short username: "Fikri01" becomes "fikri01@ihcc.app".
  // Create the Supabase user with that email. People can also type a full email.
  USERNAME_DOMAIN: "ihcc.app"
};


## 3. Publish
Drag the folder onto Netlify (or push to GitHub and connect Netlify).
On your phone, open the site → Share → **Add to Home Screen**.

## Adding colleagues
1. Supabase → Authentication → Users → **Add user** (e.g. `ali@ihcc.app`, auto-confirm).
2. In the app (as admin), **Authorised users** panel → add `ali` with a role:
   - **Viewer** – can see everything, cannot change anything
   - **Editor** – can add, edit and delete jobs
   - **Admin** – everything, plus manage the user list
Removing someone from the list blocks them immediately, even if they still have a login.

Security is enforced by the database (Row Level Security in `setup.sql`), not just by hiding buttons.
CEM calculator entries are kept in each user's browser; only the job register is shared.

## CEM Calculator (cem.html)
Files: `cem.html`, `cem-app.js`, `cem-report.js`, `usechh-pel.js`. Keep them all in the same folder as `index.html`.

1. **Upload CoA**: the lab's PDF (or Excel/CSV/photo). Points, sample IDs, parameters and µg/sample results are read and shown for checking. Blanks are unticked. The CoA is attached as Appendix G automatically.
2. **Chemicals**: type a name (English or Malay) or CAS number in the search box. The PEL comes from USECHH 2000 Schedule I, along with a suggested NIOSH method and sampling media. ppm-only limits are converted to mg/m³ using molecular weight (e.g. methylene chloride 50 ppm ≈ 173.68 mg/m³).
3. Enter field data (period, fc, t, temperatures, pressures) and the **existing controls** (section 3).
4. Section 6 writes the executive summary, discussion and recommendations from the results, the PELs, Schedule II and the controls. Edit them freely.
5. **Download full report…** lets you choose a PDF file or a Word (.docx) file. Both include all appendices.

`usechh-pel.js` holds all 678 Schedule I entries, transcribed from the gazetted PDF and checked page by page.
Three entries are printed inconsistently in the official text (hydrazine, methyl 2-cyanoacrylate, vinyl bromide) and are flagged "verify".
NIOSH methods are suggestions for common chemicals. Always confirm the PEL and method for anything critical.
