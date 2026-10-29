# GOTV Final Week Page: Edit Plan

*Wed Oct 28, 2026. Internal working draft. Evaluates `index.html` in `voter-mockup` (commit a77fcb7, "gamification, track toggle, and Tally embed") against the Partnerships SOPs. Plan only: no edits to the page have been made.*

**Sources used:** `SOP_Copy_Review_Rubric.md` (four passes), `SOP_AI_Gamma_Deck_Building.md` (accessibility block, color pairs, pre-send checklist), `SOP_Partner_OnePagers.md` (hosting and firewall rules for public pages), the approved copy in Card Text v1, and the Plan v2 guardrails.

---

## Verdict

**Copy: needs a few tweaks. Page behavior and mobile layout: needs real work before anyone outside the team sees it.**

The structure is right: one action a day, two tiles, one button each, date strip, commitment form. The look is on brand and passes contrast almost everywhere. The problems are mostly mechanical, and the worst of them only show on a phone.

**How I checked:** rendered the page at 375 px and 1280 px wide, read the code, computed contrast ratios for every color pair, and compared every line of copy to Card Text v1. I did not click through live links or load the Tally form, since external sites were blocked in my test render.

---

## Must Fix Before Launch

### 1. The sticky bar covers a third of a phone screen, and jump links land under it

At 375 px wide the bar is 267 px tall on an 812 px screen. It holds the title, progress, subtitle, track toggle, the mockup dropdown, and the date pills. When you tap a date pill or "Start With Today," the card's heading scrolls to a spot underneath the bar. I saw this on the Friday jump: the heading is hidden. The cause is `scroll-margin-top: 170px`, which is smaller than the bar.

**Edit:**
- On phones, keep only the date pills and a one-line progress count in the sticky bar.
- Move the "All Actions / For You / For Organization" toggle into the page body, above the first card.
- Remove the mockup dropdown (see item 3).
- Set `scroll-margin-top` from the bar's real height with a few lines of JavaScript, so it stays right at every width.

**SOP:** the one-idea, generous-white-space rule in the Gamma accessibility block.

### 2. The buttons don't go anywhere

Every tile button is a `<button>` that only marks the day complete. None open the email packet, share kit, phonebank sign-up, or hotline page. After a click, the label is replaced with "Completed ✓," so a person can't get back to the resource.

**Edit:**
- Make each button a real link (`<a href>`, opens in a new tab) to the resource in Card Text v1, Section 3.
- Add a small separate "Mark done" control per tile. Keep the button label after it's marked.
- Add per-day, per-tile `utm_content` values (`d1_you`, `d1_org`, and so on), so you can see which day and track drive clicks.

**SOP:** every link must open and go where intended (Gamma pre-send checklist).

### 3. The "Mockup only" date dropdown is still the date logic

The page has no real date handling. Without the dropdown it always shows Wednesday as "Today."

**Edit:**
- Highlight today from the visitor's date in America/New_York.
- Before 10/28, highlight Wednesday. After 11/3, swap the hero and cards for a thank-you state.
- Keep a hidden `?preview=thu` URL option for testing. Remove the visible control.

This is a real advantage over a Gamma page, which can't highlight the current date.

### 4. No-animation rule and the confetti script

The Gamma SOP accessibility block says "No animations or transitions." Confetti fires on every completion, and buttons use CSS transitions. The confetti also loads from `cdn.jsdelivr.net`, a third-party script that hospital firewalls often block.

**Edit:** remove confetti and its script. Show a plain check mark and a text change instead. Drop the button transitions, or wrap them in `prefers-reduced-motion`.

### 5. "0 of 7 Complete" contradicts "Start where you are"

Someone arriving on Saturday sees "0 of 7" and a nearly empty bar. That's discouraging, and it undercuts the line right above it that says every card still works.

**Edit:** show "Actions taken: 2" with no denominator, or count only days that have passed.

### 6. One browser setting can break the whole page

The first line of the script reads `localStorage` with no safety net. In a private window, or with site data blocked, that line throws an error and none of the page's JavaScript runs: no filter, no highlight, no progress.

**Edit:** wrap every `localStorage` read and write in try/catch, and make the page work without it.

### 7. The commitment form has a stale tracking source, and may not display

The embed URL ends in `utm_source=afd_acog`, which looks copied from another partner page. Every pledge from this page would be credited to the wrong source. The iframe is also fixed at 320 px tall, so a longer form is clipped, and there's no fallback if the Tally script is blocked.

**Edit:**
- Change the source to `gotv_final_week` (or your chosen name).
- Add a visible "Open the form in a new tab" link under the embed as the fallback.
- Let Tally resize the embed instead of fixing the height.
- Confirm the form works on `forms.vot-er.org` before launch. I couldn't load it.

---

## Copy Fixes (Review Rubric, Passes 1 to 4)

### Pass 1: Fact-check

Nothing in the page is a statistic, so no number to check. Two claims to watch:

- **Mon, For You:** "A quick text tonight makes all the difference tomorrow." This is stronger than Card Text v1 ("can make the difference") and implies causation. Restore the softer wording.
- **Sat, For You:** the page drops "no experience needed." That's one of the main reasons people sign up for a first phonebank.

### Pass 2: IBLESSU

| Element | On the page now |
|---|---|
| Ease | Strong. One button per tile |
| Swiftness | Light. Nothing says how long an action takes |
| Safe | Present. Hotline in every card footer |
| Urgency | Present. Dated cards and a "Today" tag |
| Impact | Weak. Nothing says why one action matters |
| Belonging | Weak. "Bring your people" is the only nod to community |
| Legitimacy | **Missing.** No partner logos, no "2,100+ sites," no validators |

**The single highest-leverage addition is legitimacy.** The mockup dropped the Our Partners strip and the network line from earlier versions. Put back the partner logo strip and one line under the hero: "Trusted by 2,100+ participating sites and 80,000+ health professionals." Both are on the approved fact list.

### Pass 3: Structural polish

The mockup trimmed several tiles that were fine in Card Text v1. Restore these, unless you cut them on purpose:

| Card | Card Text v1 | Mockup now | Action |
|---|---|---|---|
| Thu, For You | "...Not an option where you live? Set your Election Day plan." | Fallback sentence dropped | Restore |
| Sat, For You | "We provide the training and the script, no experience needed. Haven't voted yet? Vote early if you can." | "Training provided." | Restore |
| Sun, For You | "Then check your polling place and hours." | "Check polling location and hours." | Restore v1 wording |
| Sun, For Organization | "...where your people will see it: your staff channel, waiting room, or patient text." | "...in your staff space or patient text." | Restore v1 wording |
| Mon, For Organization | "Send a last-call reminder to your people." | "...to your team or organization." | Restore v1 wording |
| Buttons | Open the Make a Plan Checklist, Get the Hotline Info, Get the Reminder, Find My Polling Place | Shortened, and Mon now has two different button names | Restore v1 labels |

Anything you want to keep from the mockup's wording is new copy, so it needs the **[NEW: needs OK]** tag before it goes to partners.

### Pass 4: Brand voice and house style

- No em dashes found. Good.
- Headings are Title Case, per your deliverable rule.
- **Stay Involved card is gone.** The Election Protection observer link and the registration pointer to the Action page (Plan v2 guardrail) aren't on the page. Put both back, or confirm you cut them on purpose.
- **No Vot-ER logo.** CLAUDE.md says formal deliverables carry the logo (`Foundation Workflows/logo.png`). Add it to the hero and footer.
- **Hotline is plain text.** Make `866-OUR-VOTE` a tap-to-call link on phones, and link `vot-er.org/hotline` for the Spanish, Asian-language, and Arabic lines. This is the biggest ease win in the footer.

---

## Accessibility and Contrast (Gamma SOP Block and Color Pairs)

**Passes:** navy on sky blue (4.98), sky blue on navy (4.98), dark blue on white (5.98), dark blue on light blue (5.36), gray text on white (7.53), white on the completed green (5.13). No light blue or cyan text on a light background. No images, so no alt text needed yet.

**Fails or off-palette:**

| Item | Issue | Fix |
|---|---|---|
| Primary and org buttons on hover | Navy on `#2CA3DC` is 4.25, below the 4.5 body-text minimum | Use a darker hover, or keep sky blue and add a border |
| Completed button green `#2E7D32` | Not in the Vot-ER palette | Use Dark Blue `#106A94` with a check |
| Toggle, date pills, dropdown | No visible keyboard focus outline (only `.btn` has one) | Add `:focus-visible` outlines |
| Track toggle | No `aria-pressed`, and the click handler uses the global `event` | Pass the button in, and add `aria-pressed` |
| Date pills | Active state is only a color change | Add `aria-current="date"` on today's pill (the "Today" tag already helps) |
| Page | No skip link | Add "Skip to today's action" |
| Filter | Sets inline grid columns using the window width at click time, so it breaks if the window is resized later | Use CSS classes instead of inline styles |

---

## Hosting and Firewalls (One-Pager SOP)

The one-pager SOP says a public page that partners forward has to "survive hospital firewalls" and live at a stable URL, which Comms hosts on vot-er.org. This page loads three outside resources: Google Fonts, the confetti script, and the Tally widget. Fonts fall back safely and confetti is being removed. The Tally embed is the exposure, so the fallback link in item 7 matters.

Also add `og:image`, `og:url`, and a meta description. The page is meant to be forwarded, so the preview card on a text or email is part of the first impression.

---

## Decisions I Need From You

1. **Ship this HTML page, or rebuild in Gamma?** The HTML page keeps today's highlight, the You/Organization toggle, and progress tracking. A Gamma page can't do any of those. If it stays HTML, who hosts it: Comms on vot-er.org, or this repo (is GitHub Pages on)?
2. **Share kit link.** Still open: a public Drive link, or a Comms-hosted page.
3. **Mon line.** Restore "can make the difference," or do you want something stronger?
4. **Progress count.** Drop the denominator, or drop progress tracking altogether?

---

## Edit Order

| Step | Edits | Effort |
|---|---|---|
| 1 | Items 1, 2, 3 (layout, real links, real date logic). These change what the page is | Largest |
| 2 | Items 4, 5, 6, 7 (no confetti, progress wording, storage safety, form fix) | Small |
| 3 | Copy fixes: restore v1 wording, add legitimacy line and partner strip, Stay Involved, logo, tap-to-call hotline | Small |
| 4 | Accessibility fixes from the table above | Small |
| 5 | Pre-publish checklist: every link opens, Tally loads, read aloud once, test at 375 px and 1280 px, then test with the confetti and fonts blocked | Small |

**Definition of done:** a person on a phone lands on today's card with its heading visible, taps one real button, and reaches the resource in one tap.
