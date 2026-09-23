# McGuirk Hire — Operator App wireframes

Interactive wireframe of the **McGuirk Hire operator phone app**, prepared by
DMC Consultancy Ltd. One self-contained page, no build, no dependencies, no
backend — everything runs in the browser tab.

What it covers: **plant machinery job cards** worked through on the phone, the
**GA1 thorough examination** certificates behind every lifting appliance, and a
**fleet** view that puts both against the machine they belong to.

There is no office backend in this set — the phone app only, as asked.

## Running it

No Node on the machine, so serve the folder with Python:

```bash
python3 -m http.server 8791 --directory public
```

Then open http://localhost:8791

With Node installed, `npm start` does the same through `server.js` (that is what
Railway runs).

## Signing in

PIN is **1111**; tap the name to go straight in. The list in the rail beside the
handset signs in too, so the demo can be driven from outside the phone while it
is up on a screen.

| Who | Access | What they are for |
| --- | --- | --- |
| Johnny McGuirk — Yard Manager | Full | Everything: raises hires, extends them, takes them back, raises and closes job cards |

**One account for now.** The two access levels are still built in and everything
that reads them still works — a *general access* person fills in dockets and
works the cards they are given, but cannot extend a hire, hand a card to somebody
else, or close one with a PO. To put one back, add a row to the `employees` list
at the top of the script with `access:'general'`; the screens and the rules
behind them pick it up with no other change.

## 1 · Job cards — four steps

`Start → Work → Parts → End`, on the MPF pattern.

- **Start** is a read-only brief: machine, meter, where, who to ask for, the
  fault as reported and parts already allocated. The fitter never re-enters what
  the yard already set.
- **Work** — what was carried out, labour hours, and a 10-row service checklist,
  each row pass / defect / n-a with a note on anything failed.
- **Parts** — allocated parts can be struck off if they were not used; anything
  off the shelf gets added.
- **End** — before and after photos, signed on site, then away to the office.
  A card sent is **Awaiting PO** and the machine goes back on the list; a full
  access account adds the PO to close it.

States: `Unassigned · Assigned · Draft · Awaiting PO · Complete`. Every step
saves as it is left, so a card put down mid-job is a Draft with the work on it.
Unassigned cards put a banner on Home.

## 2 · Plant Machinery GA1

Search the fleet, pick a machine, read its **report of thorough examination**.

The GA1 applies to **lifting appliances**, and that distinction is built in: an
excavator or a telehandler needs one, a dumper, roller, compressor or genset does
not. So *"no GA1 on file"* and *"not a lifting appliance"* are different answers —
one wants chasing, the other is simply correct — and the screen says which.

- **Search** on plant number, model, serial or certificate number
- **Filters** — needs attention / in date / due soon / out of date / no GA1 needed
- Every machine shows its certificate number, next due date and standing:
  *245 days left*, *due in 25 days*, *lapsed 30 days ago*
- Anything **out on site on a lapsed GA1** is called out at the top of the screen
- Tapping a machine gives the report in force — examined on, next due, interval,
  **safe working load**, whether it is safe to operate, any defects noted, and the
  competent person who signed it — plus earlier reports on the same machine
- **View the certificate** opens it laid out as **Form GA1**, headed by the
  examining company with the McGuirk mark as the owner's file copy. Print works

The seed covers every case: three in date, **MH-202** falling due inside the
month, **MH-530 lapsed and out on a customer's site**, and six machines correctly
outside the regime.

## 3 · Fleet

Every machine, searchable on plant number, model, serial or site, filtered by
**GA1 attention**, **open cards**, or where the machine is. Each row carries its
status, its GA1 standing and any open cards.

Opening a machine gives its details — model, serial, year, hour meter, fuel,
where it is standing and since when — and then its paperwork:

- **GA1 — thorough examination.** The report in force with its certificate
  number, dates, standing and safe working load, superseded reports underneath,
  each opening the printed Form GA1. A machine outside the lifting-appliance
  rules says so instead of showing a gap.
- **Job cards** raised on that machine, newest first, each opening the card —
  and a button to raise a new one against it.

## Also in the app

Clock in / out and a **New job card** shortcut sit at the top of Home, and
**Profile** carries the access level and a **Reset the demo data** button.

Four screens: Home, Job Cards, Plant Machinery GA1, Fleet.

## Known limits of a wireframe

- **Nothing is saved.** Data lives in the tab, reseeds on reload, and dates shift
  to whatever day it is opened, so the story reads the same whenever it is shown.
- **Photos are stand-ins.** Tapping Before or After on a job card drops a
  labelled placeholder so the demo flows in one tap, rather than making anybody
  find a photo of a digger mid-pitch.
- **Signatures on the dockets that were already on file** are generated. The ones
  you draw are yours.
- **The yard's own address, phone and VAT number are placeholders.** They are in
  one place — the `YARD` object at the top of the script — and print on the GA1,
  so they want swapping for the real ones before this is shown.
- The logo is **rebuilt as vector** from the artwork supplied, so it prints
  sharp. Worth swapping for the real file if they have it as SVG or EPS.

## Not in this set, by decision

**On hire / off hire has been removed**, and the dockets it produced went with it
— they were its records and there was nothing else in them. It is all in the git
history (`a4b816a` and earlier) if it is ever wanted back.

The **GA2 weekly check** was built and then removed at the same time; it is in
`a4b816a` too.

GA1 certificates can be read and printed but **not added** — the reports come
from the examining company.

## What would sharpen it

- **Their service checklist.** The 10 rows on a job card are a sensible service
  list; if the workshop already has its own, that goes in instead.
- **A real GA1 from their examining company.** The printed form follows the
  statutory headings, but matching their inspector's actual layout would make it
  unarguable.
