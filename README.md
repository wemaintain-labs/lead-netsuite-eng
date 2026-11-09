# WeMaintain: Lead NetSuite Engineer case study

Hi, and thanks for taking the time!

This repository is a slice of WeMaintain's NetSuite: a recreated environment
built to mirror how our setup looks and behaves, a year of data across our
four countries, and the documents you would inherit in your first week.
It is the material for the next step of our conversation, set up like a
working session we would have internally.

## The situation

WeMaintain maintains lifts, escalators and automatic doors in France, the UK,
Singapore and Hong Kong. NetSuite runs our finance and supply chain. Until now
an external partner configured and ran it for us. We are bringing it in-house,
and you are the person taking it over.

In December 2025 and again in October 2026, Hong Kong's stock had to be
adjusted down by large amounts. It is the first thing the board asks about
when NetSuite comes up. The partner has put a fix on the table.

The short timeline:

- **Late 2025:** Hong Kong starts tracking its stock in NetSuite. It has used
  NetSuite for invoicing and bills since early 2025.
- **December 2025:** year-end stock counts in every country, Hong Kong's first
  in NetSuite. Its adjustments make the board pack.
- **January to September 2026:** smaller adjustments here and there.
- **18–20 October 2026:** Hong Kong recounts, and a second large write-down
  follows.
- **26 October 2026:** the partner submits their fix proposal.
- **30 October 2026:** the partner sends their handover notes.
- **2 November 2026:** the data in this repository is extracted.

Imagine you have just joined as our Lead NetSuite Engineer. Here is your first
assignment, as it landed on your Slack:

> Hey! The Hong Kong write-offs are still the first thing the board asks
> about: what happened, can it happen again, and what are we doing so it
> doesn't? From now on, that question is yours. The partner left us a
> proposal before handing over (it's in the repo); what we do with it is
> your call too. Let's take 60 minutes with David (CTO), Marine (VP
> Operations) and me (Tim, CIO) to go through your take.

That meeting is the interview. Prepare for it the way you would if this were
your job.

## What is in the repository

| File | What |
| --- | --- |
| `01-partner-fix-proposal.md` | The partner's proposed fix, dated 26 Oct 2026. |
| `02-partner-handover.md` | The partner's handover notes on how our NetSuite is set up. |
| `03-ops-finance-exploration.md` | A review of stock and the close that Ops and Finance ran this autumn. |
| `04-field-notes.md` | Short interviews with people who work with the system every day. |
| `05-data-guide.md` | How the data is organised, and its known quirks. Start here before opening the data. |
| `data/netsuite.sqlite` | One year of NetSuite data (Nov 2025 to Nov 2026; stock adjustments from Jan 2025), all four countries. |
| `data/platform_hk_parts_used.csv` | An export from our own maintenance platform: parts technicians recorded on Hong Kong jobs. |

**About the data.** It is recreated to mimic a real NetSuite: people,
suppliers, clients and buildings are fictional, and amounts and volumes are
illustrative. It is deliberately realistic, including whatever is odd,
incomplete or inconsistent. The documents were written by people with a point
of view, at different times, from different snapshots, so their figures will
not always match what you compute, or add up with each other.

## Opening the data

`data/netsuite.sqlite` is a single SQLite file: no server, nothing to install
beyond a client. With the `sqlite3` command line (preinstalled on macOS and
most Linux distributions):

```bash
sqlite3 data/netsuite.sqlite
.tables
.headers on
.mode column
```

DBeaver, DB Browser for SQLite, Python (`import sqlite3`) or pandas work just
as well. `data/platform_hk_parts_used.csv` opens in anything that reads CSV.

## A tour

1. Read `05-data-guide.md`: every column is stored as text, dates are
   `DD/MM/YYYY`, and a few signs are inverted.
2. Look at one purchase order and what was received against it:

   ```sql
   select po_number, trandate, vendor, line, item, item_type, quantity, quantityreceived, rate, currency
   from po_lines where po_number = 'PO-FR-010967';

   select receipt_number, trandate, line, item, quantity, receipt_location
   from receipt_lines
   where po_id = (select po_id from po_lines where po_number = 'PO-FR-010967');
   ```

   You will see the received quantity stored as a negative number, the
   receipt's header row at `line = 0`, and a shipping service next to the
   stock items.

## How to prepare

- Treat the repository as self-contained. Where something is unclear, make a
  call, write down the assumption, and keep going: we expect you to fill the
  blanks yourself. Which questions you would have asked is part of the
  conversation.
- If something truly blocks your work on the case, ask us at marine@wemaintain.com,
  tim@wemaintain.com and david@wemaintain.com.
- Use any tool you like, AI included; we do.
- Spend the time you find useful. There is a lot here: go as deep as you think
  the problem deserves.
- No slides needed. Notes, a document or a repository are all fine, as long as
  they help you talk us through it.

## Before the meeting

Share what you have prepared before we meet:

- **On a Git repository:** a private GitHub repository with access for
  `Timothep`, `MarineAstorg` and `Superd22`.
- **Otherwise:** a document shared with marine@wemaintain.com,
  tim@wemaintain.com and david@wemaintain.com.

We go through it first, and we book the 60 minutes once we have. If we don't
think it's ready for the conversation, we'll tell you why.

## About the meeting

You drive the first half: what happened, what you make of the partner's
proposal, and what you would put in place. We spend the second half on the decisions
you made and the ones you did not. We will ask you to show us the records
behind your claims, and we will throw in a few new facts along the way, as
colleagues would.

See you soon!
