# Rosco EverTune Tool

Two-stage EverTune tool. Work out whether a guitar takes an EverTune and which model, then get the per-guitar router depths and template sequence for the actual install.

**Live tool:** _(GitHub Pages, not deployed yet)_

## Stage 1 - Candidacy

One question at a time, and it only asks what your answers actually require.

1. String height at the bridge (with a diagram, because this is the one people measure wrong)
2. Body thickness at the bridge
3. The old bridge
4. Body construction and top carve
5. String count and scale

Then it branches. An **F** gets asked about the pickguard, because that decides whether the 1/4" web comes out or gets chamfered. A **G** gets asked for the scale-line-to-bezel distance, because that is the only thing separating Standard from Long. Neither is shown to the other. String spacing is last and skippable.

If the combined-thickness gate fails, it stops right there rather than marching you through six more questions for a guitar that was never a candidate.

The result leads with the verdict and three numbers. Everything else is collapsed until you want it, except blockers and the carved-top fork, which open by default. Answer chips across the top jump straight back to any question to change one thing.

## Stage 2 - Install

Gated behind `?tech=1` or a passcode, because the routing sequence is shop IP.

Computes every router depth for the body thickness you entered, then walks the correct template sequence as a checklist grouped into Prep / Top routes / Back routes / Assembly, each showing its own progress. Branches on model, pickguard, existing trem, riser and top carve. Ticks save in the browser.

**Model override.** Auto follows string height, but Force F is the carved-top fork: F model plus a riser on a G-range guitar, usually to hide old TOM holes. Forcing F with no riser height set throws a hard stop, since the riser is the thing making up the height.

### The depth rule

EverTune measures **all depths from the top of the guitar, even for bottom routes**. So:

```
router_cut_depth = body_thickness - spec_depth_from_top
```

Top routes plunge straight to the number instead. If a front riser is fitted, its height is added to every backside depth, because the riser lifts the bridge and the pocket has to follow it down.

## Two sides

**Front end** is for anyone. Measure two things, answer a few plain questions, get a plain answer. It never asks for an action figure, never shows a router depth, never asks the customer to work anything out. Where a guitar could go either way it asks which they *want*, with pictures, and treats it as a look-and-cost choice rather than a technical one.

**Install mode** is behind `?tech=1` and carries everything numeric: the riser calculator, the per-guitar router depths, the template sequence, and a dimension quick reference off the EverTune drawings.

## The F & T string-height window

`.350"` to `.532"` — **8.89mm to 13.51mm**, midpoint **11.20mm**.

These are not in any EverTune text. They are dimension callouts on the F&T overall-dimensions drawing, which is why every text search comes up empty. The arithmetic confirms them: the sheet's `2.098" / 53.3mm` "limiting dimension for guitar thickness" is exactly `1.750"` minimum body **plus** `.350"` minimum string height. **The combined gate is just the sum of the two minimums.**

Two consequences the tool now enforces:

- **A floor.** Strings below 8.89mm at the bridge cannot take any EverTune, whatever the body thickness. That needs a neck angle change first, and it is a different job. The old combined-only gate would have passed a thin-string guitar on a thick body.
- **A real ceiling.** The F tops out at 13.51mm. The Buying Guide rounds that to 13mm / half an inch for the F-vs-G call.

The string-height question shows a live gauge of where the guitar lands in that window.

## Limiting dimension for guitar thickness

Each bridge has its own figure, marked on its overall-dimensions drawing. This is the gate `body thickness + string height` has to beat:

| | Inch | mm |
|---|---|---|
| F & T | 2.098" | 53.29 |
| G, typical install | 2.396" | 60.85 |
| G, extremely thin guitars | 2.093" | 53.16 |

The G having two is the useful part: between 53.16 and 60.85 it still goes in, but only as the awkward route-through-body version.

## Front riser calculator (install mode)

Sizes the Rosco front riser plate from one measurement:

1. Drop the existing bridge until the strings just touch fret 12, measure at the bridge. That is the geometric floor, zero action.
2. Action is **assumed at .0625"**. Customers do not know what action they want, so the tool does not ask. Adjustable on the bench.

Fret 12 is the midpoint of the string, so raising the bridge end by X lifts the string over fret 12 by about X/2. To buy action A you need **2A** at the bridge:

```
string height at bridge, set up  =  fret-12 touch height  +  2 x target action
riser thickness                  =  that  -  11.20mm  (middle of the F window)
```

Aiming at the middle leaves adjustment both ways. The tool also gives the full window of riser thicknesses that physically work, and refuses to size one under 1mm because there is no stock worth making that thin.

One click applies the calculated riser to the depth table, so the back routes move with it.

**Setup cross-check.** If the measured string height is also on file, those two numbers together imply the action the guitar is *currently* set up at. The tool reports it. Outside the recommended band it says so, and says which way the model call is likely to be skewed, because a guitar with the bridge cranked up reads as G when it is really an F.

## Data tiers

Every rule in the tool is tagged:

- **ET** - published EverTune spec, cited
- **RG** - Rosco bench practice, where it goes beyond or differs from what EverTune published
- **GAP** - EverTune publishes nothing here, so the tool says so rather than inventing a number

There are more GAPs than you would expect. No published string-height min/max per model, no E-to-e spacing figure, no riser thickness method, no neck angle spec, no carved-top rule, no 12th-fret action numbers.

## Open questions

The tool tracks its own unknowns. The panel at the bottom lists every unresolved item, lights it amber when the guitar you just entered actually depends on it, and takes notes that save in the browser. Tick one off when it is settled. **Copy all as markdown** dumps the lot with your answers.

Two of them block v0.1: the G Standard vs Long threshold (EverTune's own pages say 0.55"/14mm in one place and 5/8" in another) and whether BOT 4 is F&T or F only.

## Deployment

Single self-contained `index.html`. No build step, no dependencies. Served via GitHub Pages from `main`, same pattern as the Shim Generator and the Custom String Calculator.

## Always verify on the bench

Published thresholds are nominal and several key numbers are not published at all. Measure the guitar, and check the result against what is in front of you before you cut.

---

Built for [Rosco Guitars Ltd.](https://roscoguitars.com) - EverTune accredited installer


## Source drawings

`reference/` holds the EverTune PDFs the rules come from:

- `FT_overall.pdf` / `G_overall.pdf` — overall dimensions. The string-height window, the limiting dimensions, E-to-e spacing.
- `FT_inst.pdf` / `G_inst.pdf` — template instructions. The sequence, every depth, the depth formula.
- `F7_route.pdf` — 7-string routing dimensions.

The dimension callouts on the overall-dimension sheets are **vector graphics, not text**. Text extraction returns the title block and nothing else, which is why so much of this looked unpublished until the drawings were read as drawings.
