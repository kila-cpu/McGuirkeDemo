# McGuirk Hire — Operator App wireframes

Interactive wireframe of the **McGuirk Hire operator phone app**, prepared by
DMC Consultancy Ltd. One self-contained page, no build, no dependencies, no
backend — everything runs in the browser tab.

What it covers: **job cards** worked through on the phone — on plant or on a
vehicle, each with its own paperwork — the **GA1 thorough examination**
certificates behind every lifting appliance, and a **fleet** view that puts both
against the machine they belong to.

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

## 1 · Job cards — plant or vehicle

Raising a card starts with the one question that changes everything after it:
**is the job on plant or on a vehicle?** The two carry different paperwork, so
the form, the job types and the checklist all follow from that answer. The list
filters on it too — Everything / Plant / Vehicles.

### The card itself — four steps

`Start → Work → Parts → End`, on the MPF pattern.

- **Start** is a read-only brief: the machine or the reg, the meter or the
  odometer, where it is, the job as raised, and parts already allocated. On a
  vehicle it also carries the **last 12 week check** and the **CVRT date**, and
  says so in red if either has lapsed. The fitter never re-enters what the yard
  already set.
- **Work** — what was carried out, labour hours, and then **the checklist that
  belongs to this job** (below).
- **Parts** — allocated parts can be struck off if they were not used; anything
  off the shelf gets added.
- **End** — before and after photos, signed on site, then away to the office.
  A card sent is **Awaiting PO**; a full access account adds the PO to close it.
  Closing a vehicle card puts it back on the road, and closing a **12 week
  check** files that date as the vehicle's new one.

States: `Unassigned · Assigned · Draft · Awaiting PO · Complete`. Every step
saves as it is left.

### Plant job types

Service, Repair, Breakdown, Inspection, Damage repair — with the 10-row service
checklist on the Work step.

### Vehicle job types

**12 week check · Tyres** · Service · Repair · Breakdown · CVRT preparation ·
Driver defect.

Two of them carry their own paperwork:

**12 week check** — the preventative maintenance inspection, **35 points over
seven sections**, banded and walked in the order it is actually carried out:
driver's controls and cab, braking system, steering and suspension, wheels and
tyres, lighting and electrical, body/fuel/exhaust, and a road test. Each point is
pass / defect / n-a, and a defect needs a note.

**Tyres** — a row per wheel, off the vehicle's own axle layout: a 6x2 tractor
unit gives eight wheels, an 8x4 tipper twelve, a van four, a tri-axle trailer
six. Each row takes a **tread depth and a pressure**, and reads back a verdict
against the **legal minimum for that class of vehicle** — 1.0 mm over 3.5 t,
1.6 mm below it. Anything under is red and named as below the limit; anything
within 1.5 mm of it is amber and marked Monitor. Each wheel also carries an
action: no action, replaced, repaired, swapped, or pressure adjusted. The step
will not pass until every wheel has been read.

Both print through onto the card, so a finished 12 week check reads back in its
sections and a finished tyre job reads back wheel by wheel.

### The vehicles

Six on the books — a Scania 6x2 tractor unit, a Volvo 8x4 tipper, a DAF 7.5t
beavertail plant carrier, an Iveco 7.2t tipper, a Transit van and a tri-axle low
loader trailer. Each carries its odometer, its last 12 week check, its CVRT date
and its axle layout. The seed has one **12 week check overdue**, one **CVRT
lapsed**, and a tyre job with a wheel **below the legal limit**.

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

Home carries a **New job card** shortcut, a count of open cards, GA1s out of
date and machines in the yard, then the three screens. **Profile** carries the
access level and a **Reset the demo data** button.

There is no clock in / out and no alert banner on Home: the counts and the tile
badges already say what wants doing.

Four screens: Home, Job Cards, Plant Machinery GA1, Fleet. The tab bar carries
Home, Cards, Fleet and Profile — GA1 is reached from its Home tile and from any
machine in the fleet.

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

- **Their service checklist and their 12 week inspection sheet.** The 10 service
  rows and the 35 inspection points are sensible lists; if the workshop already
  has its own sheets, those go in instead.
- **A real GA1 from their examining company.** The printed form follows the
  statutory headings, but matching their inspector's actual layout would make it
  unarguable.
