# NewCo survey — website

A single-page survey (`index.html`) that saves each response as a row in a
Google Sheet (`backend.gs`). No server, no database, free to host.

This version is answered by the young woman herself (ages 13–25), not by a
parent answering on her behalf. If you also need the original parent-facing
version, keep a copy of it separately — the two use different column names
in the Sheet and shouldn't be pointed at the same tab.

## 1. Try it locally (demo mode)

Double-click `index.html`. With `SUBMIT_URL` empty it runs in demo mode:
nothing is saved, and the submitted data is printed to the browser console
(right-click → Inspect → Console).

## 2. Connect the Google Sheet (~5 min)

1. Create a new Google Sheet (e.g. "NewCo parent survey responses").
2. **Extensions → Apps Script.** Delete the starter code and paste in `backend.gs`. Save.
3. **Deploy → New deployment →** gear icon → **Web app**.
   - Execute as: **Me**
   - Who has access: **Anyone** (so respondents don't need a Google account)
4. Click **Deploy**, approve the permissions, and copy the **Web app URL** (ends in `/exec`).
5. Open that URL in your browser. You should see `"NewCo survey endpoint is live."`
6. In `index.html`, paste the URL into `const SUBMIT_URL = '';` near the top.
7. Open `index.html` again, run through the survey, and check that a row shows up on the **Responses** tab.

If you edit `backend.gs` later: **Deploy → Manage deployments → edit → Version: New version**.
That keeps the same URL.

## 3. Put it online

Easiest: **Netlify Drop**. Go to https://app.netlify.com/drop and drag the
`survey` folder onto the page. You get a public link right away. Make a free
account so the link doesn't expire, and rename the site under Site settings
(e.g. `newco-parent-survey.netlify.app`).

Alternatives: GitHub Pages or Vercel. Both work, since it's just one static file.

## What the site does that Google Forms couldn't

- Screens people out using the Q1 answer directly: anyone not between 13 and 25 sees a thank-you page.
- Shows the 13–17 or 18–25 version of the concerns question to match her age.
- Shuffles the order of Directions A–D for each respondent and records the order in `direction_order`.
- The ranking grid lets each rank be used only once. Picking a rank that's already taken moves it to the new row.
- Shows the parent-visibility questions (Q15–Q18) only to respondents aged 13–17. Respondents aged 18–25 skip straight from the open-ended questions to the contact page, since those questions assume a parent is still involved in her care.
- Shows the "what would make you more comfortable" question only when comfort is 3 or lower.
- Shows two different thank-you messages, depending on whether the respondent checked a follow-up box and left an email.
- If the tab is refreshed partway through, answers are kept for that tab only.

## Reading the sheet

- Filter `status = complete` for analysis. `screened_out` rows only contain Q1–Q3.
- `rank_A`–`rank_D`: 1 means ranked first.
- `dirA_want`–`dirD_want` are the 1–5 ratings, whatever order the respondent saw them in.
- `q15_current_parent_access`, `q16_want_parent_to_see`, `q17_comfort` will be blank for every 18–25 respondent — that's expected, not missing data.
- Emails are opt-in, but they sit in the same row as the answers. Share the sheet only with people who need it.
