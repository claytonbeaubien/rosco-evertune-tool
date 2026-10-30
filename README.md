# Rosco EverTune Routing Tool

EverTune install calculator from [Rosco Guitars](https://roscoguitars.com), an accredited EverTune installer in BC, Canada.

Enter the string height at the bridge and the body thickness, and the tool recommends a model and risers, draws the section at the centreline and a top view, checks candidacy, and gives the per-guitar router depths for the F / T template sequence.

**Live tool:** https://claytonbeaubien.github.io/rosco-evertune-tool/

## What it does (v0.1)

- **Recommend mode** picks F / T or G and sizes the top riser and back cover riser. **Manual mode** lets you try the other bridge or your own riser numbers and see what changes.
- **Section drawing** at the centreline with the F7 routing geometry, one saddle on the string, the saddle window, risers and the footplate. Top view with pocket, holes and saddles for 6, 7 or 8 strings.
- **Verdicts:** candidacy stack, saddle window, top riser, back of the body.
- **Router depth table** for the F / T sequence with done checkboxes. Prints clean.

## Measuring string height

String height at the bridge only means something at a known action:

1. Before the old bridge comes off, set one reference string (high E) to a known 12th-fret action. 1/16" (1.6 mm) is standard.
2. Measure from the original body top to the underside of that string at the scale line. Never off a recess floor or a riser top.
3. That number is the target. After install, the EverTune saddle at mid-window should reproduce it, so the action can go both ways.

## The depth rule

EverTune measures all depths from the top of the guitar, even for bottom routes:

```
effective_body   = body_thickness + top_riser
router_cut_depth = effective_body - spec_depth_from_top
```

A top riser adds its height to every backside depth. The back cover riser goes on after and never changes a router setting.

## Where the numbers come from

Every rule is tagged in the tool:

- **ET**: published by EverTune (Bridge Buying Guide, F&T and G Overall Dimensions, F&T Template Instructions, F7 Routing Dimensions).
- **RG**: Rosco bench practice, where it goes beyond or differs from what EverTune published.

Where EverTune publishes nothing, the tool says so rather than inventing a number.

## Not yet in v0.1

- G model depths (the G section is schematic; EverTune publishes no G routing sheet).
- F6, T and F8 routing sheets (the drawing uses F7 geometry; depths are the same).
- Public candidacy questions (old bridge, construction, carve, pickguard), imperial toggle, shareable links.

## Always verify on the bench

Published thresholds are nominal and some numbers are not published at all. Measure the guitar in front of you and check the result before you cut.

## Deployment

Single self-contained `index.html`, no build step, no dependencies. GitHub Pages from `main`, root.

---

Built by [Rosco Guitars Ltd.](https://roscoguitars.com), Falkland, BC. Not affiliated with EverTune.
