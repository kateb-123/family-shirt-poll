# Family Shirt Size Poll — Design

Date: 2026-07-14
Owner: Kate Barnes

## Purpose

Collect T-shirt sizes for all 12 members of the Barnes family via a single web
link Kate texts to the adults. One tap per person, no logins, no accounts.

## People

| Button label | Role | Enters sizes for |
|---|---|---|
| Jefe | Grandpa (Mark) | Self |
| Gram | Grandma (Eileen) | Self |
| Bryant | Parent, couple A | Self + Emma, Isaac, Ian (if first of couple) |
| Charli | Parent, couple A | Self + Emma, Isaac, Ian (if first of couple) |
| Kate | Adult, single | Self |
| Tim | Parent, couple B | Self + Eileen, Rory (if first of couple) |
| Madison | Parent, couple B | Self + Eileen, Rory (if first of couple) |

Kids: Emma, Isaac, Ian (Bryant & Charli); Eileen, Rory (Tim & Madison).
Kids are ages 2–7. The niece keeps the plain label "Eileen" — no collision
because Grandma appears as "Gram".

## Flow

1. Main screen: 7 name buttons. Names that have submitted show a checkmark.
2. Tap your name → pick your own size → adult sizes: S, M, L, XL, 2XL.
3. Couple logic (per couple, first-in wins):
   - If the couple's kids have **no sizes yet**: after your own size, pick a
     size for each kid. Kid sizes: 2T, 3T, 4T, 5T, Youth XS, Youth S, Youth M.
   - If the kids **already have sizes** (spouse went first): show them with a
     single "Looks right" confirm button; tapping any kid's size lets you
     correct it before confirming.
4. Submit writes everything at once; main screen updates with checkmarks.
5. Results view (link/section at the bottom, e.g. "See all sizes"): all 12
   names + sizes in one list Kate can copy when ordering. Not hidden — family
   shirt sizes aren't secret.
6. Re-tapping your own name after submitting lets you change your answer.

## Architecture

- **Frontend**: single static `index.html` (vanilla HTML/CSS/JS, no build
  step), hosted on GitHub Pages, repo `kateb-123/family-shirt-poll`.
  Mobile-first: big tap targets, works on any phone browser.
- **Backend**: Firebase Realtime Database (free Spark plan), REST/SDK from the
  page. One JSON document:

  ```json
  {
    "responses": {
      "jefe":   { "size": "L",  "ts": 0 },
      "emma":   { "size": "YS", "enteredBy": "charli", "confirmedBy": "bryant" }
    }
  }
  ```

- **Security**: open read/write rules scoped to this one database path. It is
  a family shirt poll behind an unguessable URL; honor system is proportional.
- **Error handling**: if the database write fails, keep the picks on screen and
  show "Couldn't save — try again" with a retry button. Page load failure
  shows a plain message rather than a blank screen.

## Open item

No Firebase project exists yet on Kate's machine or GitHub account. One-time
setup (create free Firebase project + Realtime Database, paste config into
`index.html`) is required before the link goes live. Options: Kate follows a
5-minute guided walkthrough, or Claude performs it in her signed-in Chrome
with her explicit go-ahead (involves accepting Firebase terms on her Google
account).

## Testing

- Local preview: open the page via dev server, walk each persona through the
  flow (first spouse, second spouse confirm, correction, single adult, re-edit).
- Verify checkmarks and the results list update after each submission.
- Phone-width viewport check (375px) and dark mode.

## Out of scope (YAGNI)

Logins, editing other people's adult sizes, color/fit preferences, deadlines,
reminder emails, admin password.
