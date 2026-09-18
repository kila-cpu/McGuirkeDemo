# McGuirk Hire — Operator App wireframes

Interactive wireframe of the **McGuirk Hire operator phone app**, prepared by
DMC Consultancy Ltd. One self-contained page, no build, no dependencies, no
backend — everything runs in the browser tab.

What it covers: **on hire and off hire dockets** signed at the machine, and
**plant machinery job cards** worked through on the phone. The two are wired to
each other, so a defect found on a docket raises a card, and a card closed puts
the machine back on the hire list.

There is no office backend in this set — the phone app only, as asked. The app is
the two features and the dockets they produce: no fleet register and no timesheet.

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

## 1 · On hire docket — six steps

`Details → Readings → Condition → Damage → Photos → Sign`

- **Details** — machine (only what is standing in the yard), customer, site,
  dates, order ref. Self drive or operated; an operated hire will not go through
  without the operator named on it. Choosing the customer offers their own site
  and fills in the contact already on file.
- **Readings** — hour meter and fuel level. The meter cannot read less than the
  hours already against the machine.
- **Condition** — 15-row walk-round, each row **pass / defect / n/a**. A defect
  needs a note, and a docket completed with defects on it raises a job card
  rather than sending the machine out broken.
- **Damage** — tap *Add damage*, then tap the spot on a silhouette of that
  machine type. The pin takes an area, a type and a note. What is not marked
  here is what the customer answers for when it comes back.
- **Photos** — four fixed angles (front, offside, nearside, rear) so two dockets
  on the same machine are comparable. Meter shot optional.
- **Sign** — conditions of hire accepted, customer signs, driver signs.

Completing it creates the hire *and* the docket together, moves the machine on
hire to that site, and opens the printable docket.

## 2 · Off hire — the period decides the date

There is no request step and no office confirmation. **Every hire goes out on an
agreed period** — days, weeks or months, set on the on hire docket — so the date
it comes off charge is known before the lorry leaves the yard. The app works the
due-back date out as the period is typed and prints it on the on hire docket.

Any machine that is out can be off hired. `Readings → Condition → Damage →
Photos → Sign`, read against the on hire docket the whole way through:

- the off hire date **defaults to the agreed due-back date** (including any
  extension), and can be moved on to the day it was actually collected if the
  extra days are chargeable
- hours used comes off the two meter readings
- fuel down from the level it went out on is flagged as a refuelling charge
- days beyond the agreed period — extensions included — are called out as
  **chargeable** on the docket
- damage already marked shows **in grey**; anything added is **new this hire**
  and marked chargeable
- the on hire photographs are shown underneath the new ones

Completing it closes the hire, brings the machine back to the yard, and — if
anything failed or any new damage was marked — puts it **off road** rather than
back on the hire list and raises a **damage card against the customer**.

### Past due, and extending

A hire that runs beyond its agreed period is **still on hire and still on
charge** — being late is a fact about a live hire, not a state of its own. Those
float to the top of the register, get a red *n days over* pill, count on Home,
have their own filter, and put the banner on Home.

From there the office has two ways out: **collect it**, or **extend it**.

**Extend** takes an extra period in the same units, shows the new due-back date
as it is typed, and records **who asked for it, who agreed it, the date, and an
order ref** for the extra period — customers often issue a new PO for it. The
original agreed period is left alone and the extension sits beside it, so the
hire reads *2 weeks + 3 days* rather than losing what was signed for. A hire can
be extended more than once and the whole history is on it.

Extending moves the date the machine comes off charge. It does not wipe days that
have already run over — the form says so on a hire that is already late.

**Extending is a full access action.** It is a commercial decision, so a driver
or fitter does not get the button, and the rule behind it refuses them too.

The seed shows both cases: **MH-530** is six days over and never extended, and
**MH-421** has been extended by three days already and is still two days over.

## 3 · Job cards — four steps

`Start → Work → Parts → End`, on the MPF pattern.

- **Start** is a read-only brief: machine, meter, where, who to ask for, the
  fault as reported, parts already allocated, and the docket the card came off if
  it came off one. The fitter never re-enters what the yard already set.
- **Work** — what was carried out, labour hours, and a 10-row service checklist.
- **Parts** — allocated parts can be taken off if they were not used; anything
  off the shelf gets added.
- **End** — before and after photos, signed on site, then away to the office.
  A card sent is **Awaiting PO** and the machine goes back on the hire list; a
  full access account adds the PO to close it.

States: `Unassigned · Assigned · Draft · Awaiting PO · Complete`. Every step
saves as it is left, so a card put down mid-job is a Draft with the work on it.
Unassigned cards put a banner on a full access home screen.

## 4 · Plant Machinery GA1

Search the fleet, pick a machine, read its **report of thorough examination**.

The GA1 applies to **lifting appliances**, and that distinction is built in: an
excavator or a telehandler needs one, a dumper, roller, compressor or genset does
not. So *"no GA1 on file"* and *"not a lifting appliance"* are different answers —
one wants chasing, the other is simply correct — and the screen says which.

- **Search** on plant number, model, serial or certificate number
- **Filters** — needs attention / in date / due soon / out of date / no GA1 needed
- Every machine shows its certificate number, next due date and standing:
  *245 days left*, *due in 25 days*, *lapsed 30 days ago*
- Anything **out on hire on a lapsed GA1** is called out at the top of the screen
  and on the hire itself, because that is the one that matters
- Tapping a machine gives the report in force — examined on, next due, interval,
  **safe working load**, whether it is safe to operate, any defects noted, and the
  competent person who signed it — plus earlier reports on the same machine
- **View the certificate** opens it laid out as **Form GA1**, headed by the
  examining company with the McGuirk mark as the owner's file copy. Print works.

The hire detail carries a GA1 line for its machine, so a lapsed cert shows up
where somebody is about to send the machine out again.

The seed covers every case: three in date, **MH-202** falling due inside the
month, **MH-530 lapsed and out on hire with a customer**, and six machines
correctly outside the regime.

## 5 · The printable docket

Two pages, in the reader the paper would come out of:

1. Letterhead, docket and hire refs, customer, machine, readings, the full
   condition check with defects in red, the damage schedule with a chargeable
   column, both signatures, and the conditions of hire.
2. The four condition photographs, the damage pins drawn on the machine — grey
   for what was already there, red for what is new — and, on an off hire docket,
   a *read against the on hire docket* summary.

**Print really prints.** The pages are cloned out of the handset and sent to the
printer, so the client can hold the thing. Email and download say what they
would do instead of pretending.

## Also in the app

**Dockets** — everything signed, on file, filtered on hire / off hire, each one
opening the printable document. A docket is also reachable from the hire it
belongs to and from any job card raised off one.

Clock in / out sits at the top of Home, and **Profile** carries the access level
and a **Reset the demo data** button.

Five screens: Home, On Hire / Off Hire, Job Cards, Dockets, Plant Machinery GA1.

## Known limits of a wireframe

- **Nothing is saved.** Data lives in the tab, reseeds on reload, and dates shift
  to whatever day it is opened, so the story reads the same whenever it is shown.
- **Photos are stand-ins.** Tapping an angle drops a labelled placeholder so the
  demo flows in one tap. *From the phone* opens the real camera / file picker and
  that path works — the placeholder is only so nobody has to find a photo of a
  digger mid-pitch.
- **Signatures on the dockets that were already on file** are generated. The ones
  you draw are yours.
- **The yard's own address, phone and VAT number are placeholders.** They are in
  one place — the `YARD` object at the top of the script — and print on every
  docket, so they want swapping for the real ones before this is shown.
- The logo is **rebuilt as vector** from the artwork supplied, so it prints
  sharp. Worth swapping for the real file if they have it as SVG or EPS.

## Not in this set, by decision

Hired-in plant and rates/spend are out of scope. The GA1 screen reads
certificates and prints them; **uploading or recording a new examination is not
built**, since the reports come from the examining company.

## What would sharpen it

- **Their actual paper docket.** The printable page is modelled on what a plant
  hire docket carries, not on McGuirk's own book. With a photo of the real one it
  can be matched field for field.
- **Their condition checklist.** The 15 rows are a sensible handover walk-round;
  if the yard already has its own list, that goes in instead.
