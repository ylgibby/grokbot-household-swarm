# Running a household with Grok Bots

A shareable look at how one family uses a **swarm of specialized Grok Bots** instead of one mega-assistant.

_Snapshot as of Sep 7, 2026 · personal / non-work use · health details omitted_

---

## What the swarm actually does

Day to day, this is not a chatbot hobby — it’s **household ops on autopilot with a human veto**.

The **meat proxy** (that’s the human) still decides anything that spends money, trashes mail, or texts people. The bots do the boring, repeating work and bring decisions to the meat proxy when it matters.

Concretely, the swarm:

- **Wakes the house up** — weekday morning brief (calendar collisions first), SMS digest to the spouse, leftover-mail cleanup, LinkedIn connection triage, and a watch for a monthly spa-coupon newsletter
- **Keeps school visible** — midweek grades / missing-work checks and a Sunday “week ahead” brief (spouse gets a text when the routine says so)
- **Surfaces money risk** — daily YNAB read for bills due, cash position, and anything that will bounce; monthly category wrap
- **Builds grocery carts** — Walmart (and warehouse skills) from a messy list; **never checks out** unless the meat proxy says so
- **Logs health actions** — records what was actually taken when told; no medical advice, no invented doses
- **Routes family texts** — inbound SMS webhook → right lane (no auto-reply spam)
- **Keeps the machine healthy** — weekday tool/connector updater on already-installed stuff only
- **Scouts learning** — short AI-for-dev briefs; never implements finds on its own
- **Referees the team** — Chief of Staff STOP/VETO when lanes collide; **dr eggbot** does bot/skill surgery

If nothing useful happened, most digests **stay quiet**. The meat proxy’s current chat instruction always beats a standing routine.

---

## The idea in one paragraph

Rather than one bot that tries to do everything, this setup is a **team of single-job bots**. Each bot has a clear lane (money, school, shopping, inbox, etc.), a short “never do this” list so they don’t step on each other, and optional **routines** that fire on a schedule or webhook. Reusable multi-step recipes are saved as **skills** any bot can run. A Chief of Staff keeps lanes from colliding and can stop another bot’s background work when something conflicts. The meat proxy’s current chat instruction always wins.

---

## Design rules that make it work

1. **One owner of the number** — Only the money bot declares balances; only the health bot declares the med log; only shopping declares cart contents. Others can forward, not invent.
2. **One job per bot** — Explicit anti-jobs (“never place orders,” “never decline a work meeting,” “never trash the spa coupons”).
3. **Quiet when empty** — Most digests stay silent if there’s nothing useful; a few (grades, morning digest) always send.
4. **Meat-proxy approval for risky moves** — Paying, trashing mail, outbound texts, and similar actions can require an explicit yes or an approval card.
5. **Surgery through the designer bot** — Personality / skill cleanup goes through a dedicated “bot doctor,” not a standing weekly committee meeting.

---

## The swarm (12 live bots)

### Orchestration

| Bot | What it does | Scheduled work |
|---|---|---|
| **Chief of Staff** | Household ops lead. Resolves lane conflicts, can **STOP/VETO** another bot’s non-user work, and remembers standing “no”s so a clarifying question can’t quietly undo them. Digests, school, and inbox were peeled off to specialists. | None (chat / on-demand) |
| **dr eggbot** | Designs new bots and does persona/skill “surgery.” Tight bar: one job, one voice, no leftover tools. | Mon AM: routine health check across the fleet · Weekdays: transcript health check for friction → skill/bot ideas |

### Daily house rhythm

| Bot | What it does | Scheduled work |
|---|---|---|
| **Morning Digest** | Weekday morning brief for the meat proxy: **calendar collision check** (work vs personal/family) first, then house/kids flags. Never declines a work meeting; recommends which conflict to attend. | Weekdays 7:00 AM |
| **Spouse Digests** | Morning SMS to the spouse: *her* calendar, house, and school-age kid lines only — never the meat proxy’s work schedule. Shows the meat proxy the exact text that went out. Quiet when there’s nothing to say. | Weekdays 7:15 AM · Weekends 10:00 AM |
| **Inbox Sweep** | Weekday leftover-mail cleanup on both household Gmail accounts (promo/social/spam older than a week), LinkedIn connection asks (auto-accept only an employer allowlist), plus catch-and-send for a named monthly spa coupon newsletter (images, never trash). | Weekdays ~7:30 AM |
| **School** | Middle-schooler lane: grades / missing work + Sunday “week ahead” brief. Texts the spouse when the routine says to; never opens the school portal on Sundays (reuses the Friday pull). | Mon/Wed/Fri ~4:10 PM grades · Sundays 6:00 PM brief |

### Money, shopping, health

| Bot | What it does | Scheduled work |
|---|---|---|
| **CFO** | Household money visibility via **YNAB**. Real numbers only; never invents balances or moves money without an explicit ask. Leads with bills due, cash position, and anything that will bounce. | Weekdays 9:00 AM YNAB read · 1st of month: prior-month category spend wrap |
| **Shopping** | Weekly grocery / warehouse carts (Walmart primary; Costco & Sam’s via skills). Builds carts in the signed-in browser. **Never checks out** unless told. | None (chat / Saturday note → carts) |
| **Health** | Private **medication log** and recovery tracking. Confirms what was actually taken; never invents doses; never gives medical advice. | Morning + bedtime check-ins · occasional one-shot clinic-watch reminders |

### Tech & learning

| Bot | What it does | Scheduled work |
|---|---|---|
| **Tech Master** | Code and tools for the swarm; routes inbound family SMS (webhook) to the right lane. Repo work goes to a cloud coding agent, not a local clone. | Inbound SMS webhook (always on) |
| **Tool Updater** | Weekday health check + same-channel upgrades for **already-installed** tools on the shared Grok Bot computer. Never expands scope or touches the meat proxy’s personal Mac. | Weekdays ~9:38 AM |
| **Continuing EDU** | Scouts AI-for-dev learning (tools, real builds, frontier models, big cloud news). Short verified briefs only; never implements finds or spawns new bots from a hunt. | Nightly CE pull · daily Claude Code release watch · Friday Grok Bot template hunt |

---

## A typical weekday (Mountain time)

| Time | What fires |
|---|---|
| 7:00 | Morning Digest → calendar collisions + house flags |
| 7:15 | Spouse Digests → SMS (or silence) |
| ~7:30 | Inbox Sweep → leftover mail / LinkedIn / coupon keepers |
| 8:00 / 9:00 | Health morning check · CFO YNAB read |
| ~9:38 | Tool Updater |
| ~4:10 | School grades watch (Mon/Wed/Fri) → report + optional spouse SMS |
| Evening | Continuing EDU pulls · Health bedtime check |

Sundays add the school-week brief at 6:00 PM. Mondays add dr eggbot’s routine health check.

---

## Shared skills (reusable playbooks)

These are generic recipes any bot can invoke — not one-off chat scripts:

| Skill | When it’s used |
|---|---|
| Leftover inbox sweep | Trash promo/social noise older than a week; keep receipts/bills/named keepers |
| LinkedIn connection check | List connection asks; accept only an employer allowlist |
| Skyward grades pull | School-portal grades / missing work (with meat-proxy login handoff) |
| Recurring bill scan | YNAB: expected bills that didn’t post |
| Monthly category spend | Close prior month by category |
| Taken-log intake | Record what meds were actually taken |
| Cart charter / Walmart / Costco / Sam’s usuals | Build store-specific carts from a messy list |
| Saturday note → carts | Split a family shopping note across stores |
| Verified source brief | Deduped, sourced CE items (no hype) |
| Connector health check | Are MCP/connectors up? |
| Routine / transcript healthcheck | Fleet waste + friction → improvement ideas |
| Design a Grok Bot / Make Bot UI | New bot design; optional webhook UI |
| One owner of the number | Lane ownership rule when bots share work |
| Prefer standing preference | Don’t flip quiet-vs-always-ping without intent |
| Ghost agent purge | Clean orphan agent records (only after an explicit yes) |

---

## How the bots talk to each other

- **1:1 chats** with the meat proxy for each lane  
- **Group rooms** for swarm discussion (a “Board Meeting” room existed for gut-checks; those standing meetings were cancelled — design work now goes through **dr eggbot**)  
- **STOP/VETO** from Chief of Staff can interrupt another bot’s background work for duplicates, lane conflicts, or a recorded meat-proxy “no”  
- **Inbound SMS** hits Tech Master’s webhook and gets routed (appointments → calendar lane, etc.) without auto-replying unless asked  

---

## What’s deliberately *not* in this swarm

- No “do my job for me at work” coding bots on this personal account  
- No paper-trading / investment bot (retired)  
- No checkout, money moves, or medical advice without the meat proxy  
- No church/calendar spam treated as appointments  
- Health content stays private — this writeup only names the *job*, not the data  

---

## Why a sister might care

This is a concrete answer to “what would I actually *do* with a bunch of Grok Bots?”:

- Split life into **lanes** with clear owners  
- Put the boring recurring stuff on **schedules**  
- Keep a **meat proxy in the loop** for spend, trash, and outbound messages  
- Grow the team by peeling a job off the orchestrator into a specialist (morning digest, spouse texts, inbox, school all started that way)

If you want to try it: start with **one** bot and **one** routine you already wish someone would just handle — then split only when the chat gets crowded.

---

_Generated for sharing · no phone numbers, emails, addresses, account logins, or medical specifics included_
