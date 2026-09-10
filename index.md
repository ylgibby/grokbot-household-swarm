# Running a household with Grok Bots

A shareable look at how one family uses a **swarm of specialized Grok Bots** instead of one mega-assistant.

_Snapshot as of Sep 9, 2026 · personal / non-work use · health details omitted_

---

## What the swarm actually does

Day to day, this is not a chatbot hobby — it’s **household ops on autopilot with a meat proxy veto**.

The **meat proxy** (that’s the meat proxy) still decides anything that spends money, trashes mail, or texts people. The bots do the boring, repeating work and bring decisions to the meat proxy when it matters.

Concretely, the swarm:

- **Wakes the house up** — weekday morning brief (calendar collisions first), SMS digest to the spouse, leftover-mail cleanup, LinkedIn connection triage, and a watch for a monthly spa-coupon newsletter
- **Keeps school visible** — midweek grades / missing-work checks and a Sunday “week ahead” brief (spouse gets a text when the routine says so)
- **Surfaces money risk** — daily YNAB read for bills due, cash position, and anything that will bounce; monthly category wrap
- **Builds grocery carts** — Walmart (and warehouse skills) from a messy list; **never checks out** unless the meat proxy says so
- **Logs health actions** — records what was actually taken when told; no medical advice, no invented doses
- **Routes family texts** — inbound SMS webhook → right lane (no auto-reply spam); **only Tech Master** sends or receives SMS
- **Keeps the machine healthy** — weekday tool/connector updater on already-installed stuff only
- **Scouts learning** — short AI-for-dev briefs; never implements finds on its own
- **Plans a finite conference trip** — AWS re:Invent session batches + schedule-change webhook (delete after the event unless extended)
- **Referees the team** — Chief of Staff **STOP/VETO** when lanes collide (no standing digests of its own); **dr eggbot** does bot/skill surgery

If nothing useful happened, most digests **stay quiet**. The meat proxy's current chat instruction always beats a standing routine.

---

## The idea in one paragraph

Rather than one bot that tries to do everything, this setup is a **team of single-job bots**. Each bot has a clear lane (money, school, shopping, inbox, etc.), a short “never do this” list so they don’t step on each other, and optional **routines** that fire on a schedule or webhook. Reusable multi-step recipes are saved as **skills** any bot can run. A Chief of Staff keeps lanes from colliding and can stop another bot’s background work when something conflicts — but it does **not** run the daily digests anymore (those were peeled to specialists). The meat proxy's current chat instruction always wins.

---

## Design rules that make it work

1. **One owner of the number** — Only the money bot declares balances; only the health bot declares the med log; only shopping declares cart contents. Others can forward, not invent.
2. **One job per bot** — Explicit anti-jobs (“never place orders,” “never decline a work meeting,” “never trash the spa coupons”).
3. **Quiet when empty** — Most digests stay silent if there’s nothing useful; a few (grades, morning digest) always send.
4. **Meat proxy approval for risky moves** — Paying, trashing mail, outbound texts, and similar actions can require an explicit yes or an approval card.
5. **One owner of SMS + GitHub/Pages** — **Tech Master only** owns Twilio (inbound webhook + every outbound text other bots hand over) and GitHub/Pages publish paths. Other bots hand exact SMS bodies; they never install SMS credentials or send themselves.
6. **Surgery through the designer bot** — Personality / skill cleanup goes through **dr eggbot**, not a standing weekly committee meeting.

---

## The swarm (13 live bots)

### Orchestration

| Bot | What it does | Scheduled work |
|---|---|---|
| **Chief of Staff** | Household ops lead. Resolves lane conflicts, can **STOP/VETO** another bot’s non-user work, and remembers standing “no”s so a clarifying question can’t quietly undo them. Digests, school, and inbox were peeled off to specialists — **no standing digests of its own**. | None (chat / on-demand) |
| **dr eggbot** | Designs new bots and does persona/skill “surgery.” Tight bar: one job, one voice, no leftover tools. | Mon AM: routine health check across the fleet · Weekdays: transcript health check for friction → skill/bot ideas |

### Daily house rhythm

| Bot | What it does | Scheduled work |
|---|---|---|
| **Morning Digest** | Weekday morning brief for the meat proxy: **calendar collision check** (work vs personal/family) first, then house/kids flags. Never declines a work meeting; recommends which conflict to attend. | Weekdays 7:00 AM |
| **Laura Digests** | Morning SMS to the spouse: *her* calendar, house, and middle-schooler lines only — never the meat proxy's work schedule. Hands the exact text to Tech Master to send; shows the meat proxy what went out. Quiet when there’s nothing to say. | Weekdays 7:15 AM · Weekends 10:00 AM |
| **Inbox Sweep** | Weekday leftover-mail cleanup on both household Gmail accounts (promo/social/spam older than a week), LinkedIn connection asks (auto-accept only an employer allowlist), plus catch-and-send for a named monthly spa coupon newsletter (images, never trash). | Weekdays ~7:30 AM |
| **School** | Middle-schooler lane: grades / missing work + Sunday “week ahead” brief. Texts the spouse when the routine says to (via Tech Master); never opens the school portal on Sundays (reuses the Friday pull). | Mon/Wed/Fri ~4:10 PM grades · Sundays 6:00 PM brief |

### Money, shopping, health

| Bot | What it does | Scheduled work |
|---|---|---|
| **CFO** | Household money visibility via **YNAB**. Real numbers only; never invents balances or moves money without an explicit ask. Leads with bills due, cash position, and anything that will bounce. | Weekdays 9:00 AM YNAB read · 1st of month: prior-month category spend wrap |
| **Shopping** | Weekly grocery / warehouse carts (Walmart primary; Costco & Sam’s via skills). Builds carts in the signed-in browser. **Never checks out** unless told. | None (chat / Saturday note → carts) |
| **Health** | Private **medication log** and recovery tracking. Confirms what was actually taken; never invents doses; never gives medical advice. | Morning + bedtime check-ins · occasional one-shot seasonal clinic reminder |

### Tech & learning

| Bot | What it does | Scheduled work |
|---|---|---|
| **Tech Master** | Code and tools for the swarm; **sole owner of Twilio/SMS** (inbound webhook + outbound sends other bots hand over) and **GitHub/Pages**. Repo work goes to a cloud coding agent, not a local clone. | Inbound SMS webhook (always on) |
| **Tool Updater** | Weekday health check + same-channel upgrades for **already-installed** tools on the shared Grok Bot computer. Never expands scope or touches the meat proxy's personal Mac. | Weekdays ~9:38 AM |
| **Continuing EDU** | Scouts AI-for-dev learning (tools, real builds, frontier models, big cloud news). Short verified briefs only; never implements finds or spawns new bots from a hunt. | Nightly CE pull · daily Claude Code release watch · Friday Grok Bot template hunt |
| **Re:Invent** | Finite trip bot for AWS re:Invent: session suggestion batches (abstracts, level floor, hard skips) + planner schedule-change webhook. Pulls CE prefs from Continuing EDU. Delete after the event unless extended. | Weekdays 9:00 AM + 8:00 PM session suggest · schedule webhook |

---

## A typical weekday (Mountain time)

| Time | What fires |
|---|---|
| 7:00 | Morning Digest → calendar collisions + house flags |
| 7:15 | Laura Digests → spouse SMS via Tech Master (or silence) |
| ~7:30 | Inbox Sweep → leftover mail / LinkedIn / coupon keepers |
| 8:00 | Health morning check-in |
| 9:00 | CFO YNAB read · Re:Invent morning session batch |
| ~9:38 | Tool Updater |
| ~4:10 | School grades watch (Mon/Wed/Fri) → report + optional spouse SMS via Tech Master |
| 6:00 | Continuing EDU — Claude Code features watch |
| 7:00 | Continuing EDU — nightly CE pull |
| 8:00 | Re:Invent evening session batch |
| 9:00 | Health bedtime check-in |

Sundays add the school-week brief at 6:00 PM. Weekends use the 10:00 AM spouse digest instead of 7:15. Mondays add dr eggbot’s routine health check (~8:49). Fridays add the Grok Bot template hunt (5:00 PM). The 1st of the month adds CFO’s prior-month category wrap (~9:30).

---

## What each bot does in practice (non-identifying examples)

These are the kinds of moments the bots handle — not real names, numbers, or medical details.

### Chief of Staff
- Someone starts saving a routine Chris already said **no** to → CoS **STOP/VETO**s the saver until there’s an explicit new yes.
- Two bots start the same leftover-mail cleanup → CoS kills the duplicate run.
- Chris asks “what’s on Tuesday?” → CoS pulls work + personal + Family calendars and flags overlaps (e.g. a work stand-up stacked on a personal appointment).

### dr eggbot
- Chris wants a new single-job bot (e.g. conference trip planner) → asks a few preference questions, creates it with a tight “only job / never do” persona.
- Weekly scan finds a routine firing every empty morning for no reason → proposes a quieter cadence or a “stay quiet when empty” fix.
- Transcript audit spots the same correction three times → proposes a skill so the swarm stops relearning it in chat.

### Morning Digest
- Weekday 7am: “Garbage and recycling today. Work meeting A overlaps personal hold B — attend A, leave B on the calendar. Remind spouse about kid pickup timing.”
- Never declines a work meeting; never dumps the spouse’s full workday into Chris’s brief.

### Laura Digests (spouse SMS)
- Builds a short text like: `Kid : missing worksheet in science` / `House : recycling out` — hands it to Tech Master to send — shows Chris the exact body.
- Empty morning → silence (no “nothing today” spam).
- Never includes Chris’s work meetings in the spouse text.

### Inbox Sweep
- Trashes promo/social older than a week; **keeps** receipts, bills, school mail, and a named monthly spa-coupon newsletter.
- Lists new LinkedIn connection asks; auto-accepts only people from an employer allowlist; leaves everyone else pending.
- New spa-coupon mail arrives → restores it if trashed, pulls the coupon **images**, sends them to Chris (no invented typed codes).

### School
- Mon/Wed/Fri: “Three missing items in one class; grade dipped in another” → report in chat + spouse SMS via Tech Master.
- Sunday 6pm: week-ahead brief (forms, fees, early-outs) using **Friday’s** grades pull — does **not** ask for a weekend school-portal login.
- All-clear still sends (Chris asked not to stay quiet on grades).

### CFO
- Weekday morning: “Two bills due this week; one account looks thin before payday” — real YNAB numbers only, no invented balances.
- 1st of month: prior-month spend by category (read-only wrap). Optional partner summary is handed to Tech Master as SMS, not sent by CFO.
- Never moves money or places trades without an explicit ask.

### Shopping
- Saturday note “milk, dog chews, bulk paper towels” → splits into Walmart vs warehouse lists, builds carts in the signed-in browser.
- Price swap needed → asks once; otherwise proceeds.
- Cart sits ready — **checkout only when Chris says so**.

### Health
- “Logged morning set as taken” / “Bedtime set not logged yet — did you take it?” — one ask, then stop.
- Never invents a dose, never gives medical advice, never scrapes a pharmacy portal.
- Details stay in Health’s private log — not in this public writeup.

### Tech Master
- Inbound family text “dentist Thursday 2pm” → routes to the calendar lane; no auto-reply unless Chris already asked for one.
- School/Laura Digests/CFO hand an exact SMS body → Tech Master sends on household Twilio and confirms.
- Publishes/updates GitHub Pages (like this site); other bots don’t hold GitHub PATs.

### Tool Updater
- Weekday check: connector healthy? Already-installed CLI one patch behind? → same-channel upgrade on the shared Grok Bot computer only.
- Quiet when everything’s current. Never expands into new products or the meat proxy's personal Mac.

### Continuing EDU
- Nightly: “New frontier model release notes + one real build writeup” — short, sourced, deduped; skips tutorial spam.
- Daily Claude Code release watch: only surfaces real new capabilities; silence if nothing changed.
- Never implements a find or spins up a new bot from a hunt (that’s Tech Master / dr eggbot).

### Re:Invent
- Preference intake once (“level floor, hard skips, topics”) → weekday morning + evening **session suggestion batches** with abstracts.
- Schedule-change webhook: reserved vs wishlist drift → ping only when something actually moved.
- Finite trip bot — delete after the conference unless Chris extends it.

---

## Shared skills (reusable playbooks)

These are generic recipes any bot can invoke — not one-off chat scripts:

| Skill | When it’s used |
|---|---|
| Leftover inbox sweep | Trash promo/social noise older than a week; keep receipts/bills/named keepers |
| LinkedIn connection check | List connection asks; accept only an employer allowlist |
| Skyward grades pull | School-portal grades / missing work (with meat proxy login handoff) |
| Recurring bill scan | YNAB: expected bills that didn’t post |
| Monthly category spend | Close prior month by category |
| Taken-log intake | Record what was actually taken (medication log shorthand) |
| Seasonal clinic lookup | Find seasonal immunization clinic announcements in household mail/calendars |
| Cart charter / Walmart / Costco / Sam’s usuals | Build store-specific carts from a messy list |
| Saturday note → carts | Split a family shopping note across stores |
| Verified source brief | Deduped, sourced CE items (no hype) |
| CE preference feedback | Lock / reshape CE preference marks after ranks |
| Connector health check | Are MCP/connectors up? |
| Connector lane map | Route auth / PAT / Pages asks to the right owner (Tech Master for GitHub/Pages/SMS) |
| SMS lane handoff | Non–Tech Master bots hand exact SMS bodies; never dual-queue Twilio |
| Routine / transcript healthcheck | Fleet waste + friction → improvement ideas |
| Design a Grok Bot / Make Bot UI | New bot design; optional webhook UI |
| Wire Grok Bot webhook | End-to-end webhook wiring for a routine |
| One owner of the number | Lane ownership rule when bots share work |
| Prefer standing preference | Don’t flip quiet-vs-always-ping without intent |
| After peel reoffer | After peeling routines onto new bots, reoffer any leftovers |
| Ghost agent purge | Clean orphan agent records (only after an explicit yes) |
| Re:Invent preference intake / session suggest / planner pick map | Conference preference capture, session batches, reserved-vs-wishlist status |

---

## How the bots talk to each other

- **1:1 chats** with the meat proxy for each lane  
- **Group rooms** for swarm discussion (a “Board Meeting” room existed for gut-checks; those standing meetings were cancelled — design work now goes through **dr eggbot**)  
- **STOP/VETO** from Chief of Staff can interrupt another bot’s background work for duplicates, lane conflicts, or a recorded meat proxy “no”  
- **Inbound SMS** hits Tech Master’s webhook and gets routed (appointments → calendar lane, etc.) without auto-replying unless asked  
- **Outbound SMS** is always a handoff: specialist builds the exact body → Tech Master sends → specialist shows the meat proxy the body  

---

## What’s deliberately *not* in this swarm

- No “do my job for me at work” coding bots on this personal account  
- No paper-trading / investment bot (retired)  
- No checkout, money moves, or medical advice without the meat proxy  
- No church/calendar spam treated as appointments  
- No standing digests on Chief of Staff (peeled to specialists)  
- No Twilio/SMS or GitHub/Pages ownership outside **Tech Master**  
- Health content stays private — this writeup only names the *job*, not the data  

---

## Why someone might care

This is a concrete answer to “what would I actually *do* with a bunch of Grok Bots?”:

- Split life into **lanes** with clear owners  
- Put the boring recurring stuff on **schedules**  
- Keep a **meat proxy in the loop** for spend, trash, and outbound messages  
- Grow the team by peeling a job off the orchestrator into a specialist (morning digest, spouse texts, inbox, school, tool updater, re:Invent all started that way)

If you want to try it: start with **one** bot and **one** routine you already wish someone would just handle — then split only when the chat gets crowded.

---

_Generated for sharing · no phone numbers, emails, addresses, account logins, SIDs, dollar amounts, or medical specifics included_
