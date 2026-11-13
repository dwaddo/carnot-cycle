# Carnot Cycle Widget — Spec (Lecture Version)

Last updated: 10 Nov 2026 (revised to match the built widget)

## Purpose and scope

A single-page, click-through view of the four strokes of the ideal-gas Carnot cycle, with an engine / heat pump toggle. It supports the whiteboard derivation in the PHYS3113 interview lecture (UNSW, 20 Nov 2026) rather than replacing it.

This is the sparing lecture version. A separate full version (sliders, presets, T–S diagram, Otto and other cycles) will be built later from the same code; see Out of scope.

On-screen text is kept to a minimum: labels are algebraic, and the explanation is given by the lecturer, not the widget.

## Lecture use

A 20-minute lecture. The widget appears in about three short visits, each under a minute, between whiteboard work.

1. **Engine strokes, before the derivation:** step through A→B→C→D (strokes 1–4), then return to the board to derive η = 1 − T_C/T_H.
2. **Complete screen, after η:** press Next once more for the completed cycle: the net-work area, Q_H = |W| + Q_C and η = 50%.
3. **Heat pump COP, resolving the hook:** press M for heat pump mode and step through to Complete for the home heat pump's Carnot COP ≈ 9.2 (15 °C outdoor air → 50 °C hot water). This resolves the opening "400% efficient" puzzle: a real unit at COP ≈ 4 is well inside the Carnot limit.

## Layout

One 16:9 screen (designed at 1920×1080) with four regions, scaled to fill the window; nothing scrolls.

| Region | Position | Contents |
| --- | --- | --- |
| Header | Top strip | Step title (e.g. "A→B: isothermal expansion at T_H"), a terse note during strokes, step counter ("Stroke 1 of 4"), mode toggle and full-screen icon at right |
| PV diagram | Left, 50% width | Schematic cycle, A–D labelled, a moving state dot, axes p and V without numeric ticks, legend, "schematic, not to scale" |
| Cylinder | Right, 50% width | Gas temperature readout, piston and gas, a sliding contact bar underneath, Q and W arrows |
| Ledger | Bottom strip | Running Q_H and Q_C bars, net work bar at the end, W₁…W₄ record, assumptions line, closing equations |

On narrow screens (under 700 px wide, i.e. phones in portrait) the regions stack vertically and the page fits one screen height: header and ledger are compacted and the PV diagram and cylinder share the remaining height. A CSS media query decides this, not the JavaScript window width, because phone browsers zoom out to fit a 1920 px stage and then report a misleadingly wide window. Only very short screens (under 560 px visible) fall back to scrolling. Phones in landscape get the scaled 16:9 view.

## PV diagram

A schematic, not to scale, drawn in the conventional textbook shape. The real cycle on linear axes is too thin to read, so the curves are drawn from the same formulas (isotherms pV = const, adiabats pV^γ = const) with exaggerated drawing parameters (T_H/T_C = 2, γ = 2.6, V_B/V_A = 2). The drawing feeds no number.

- Axes p and V, offset to fit the cycle, no ticks; small italic note "schematic, not to scale".
- Legend: "T_H isotherm (ΔU = 0)", "T_C isotherm (ΔU = 0)", "adiabat (Q = 0)". The isotherms are red and blue, the adiabats dark grey.
- Corner labels A–D; a midpoint arrowhead on each stroke shows the direction of travel (clockwise for the engine, anticlockwise for the heat pump).
- Completed strokes are solid, the current one is thick, and future ones are faint.
- At the end of the cycle the enclosed area is shaded purple and labelled "net work".

## Cylinder

- **Gas temperature** readout at top left ("T = 600 K"), coloured between blue and red; the gas tint follows it.
- **Piston** height follows the schematic volume, in step with the PV dot.
- **Contact bar** under the cylinder: one horizontal bar with HOT (left, "HOT T_H = 600 K"), INSULATION (centre, grey hatching) and COLD (right, "COLD T_C = 300 K"). When the contact changes, the bar slides (about 0.6 s) to put the right segment under the cylinder before the stroke starts.
- **Arrows** appear once the bar has settled, in "word (symbol)" form:
  - heat through the base: "Heat in (Q_H)", "Heat out (Q_C)", in the reservoir's colour;
  - work at right, purple: "Work out (W₁)", "Work in (W₃)"; on adiabats the bracket also gives ΔU, matching the subtitle: "Work out (W₂ = −ΔU)" on expansion, "Work in (W₄ = ΔU)" on compression.
- All work labels are convention-free: W_k names a stroke's work, shown as a magnitude with a direction (in/out), so they read correctly under the course text's convention (Hoch: W is work done on the gas).
- No arrows in the overview or at the end of the cycle; the totals live in the ledger.

## Steps (engine mode)

Six states: an overview, four strokes, and a completed cycle. Labels A–D follow the existing lecture notes.

| State | Stroke | Under cylinder | Heat | Work | Gas temperature | Header note |
| --- | --- | --- | --- | --- | --- | --- |
| 0 Overview | Full cycle faint, dot at A | HOT | — | — | T_H | none |
| 1 A→B | Isothermal expansion | HOT | Heat in (Q_H) | Work out (W₁) | Stays T_H | ΔU = 0: heat in = work out |
| 2 B→C | Adiabatic expansion | INSULATION | None | Work out (W₂ = −ΔU) | Falls T_H → T_C | Q = 0: work out = −ΔU |
| 3 C→D | Isothermal compression | COLD | Heat out (Q_C) | Work in (W₃) | Stays T_C | ΔU = 0: work in = heat out |
| 4 D→A | Adiabatic compression | INSULATION | None | Work in (W₄ = ΔU) | Rises T_C → T_H | Q = 0: work in = ΔU |
| 5 Complete | Area shaded, "net work" | Unchanged from stroke 4 | — | — | T_H | none |

The complete state's title is "Cycle complete: net work out = enclosed area" ("net work in" for the heat pump). Heat pump strokes use the same subtitle rules, chosen by expansion or compression.

On entering a stroke the bar slides if needed, then the dot and piston animate together along the stroke (about 1.5 s) while the ledger fills in.

On the final Next (stroke 4 → complete) the reservoir does not move. Over about 0.9 s the purple net-work area fades in, the net work bar grows, and the net work value and closing equations fade in. Stepping back, Reset and mode switches always jump without animation.

## Engine / heat pump toggle

Heat pump mode runs the cycle anticlockwise on the same schematic curves, with every arrow reversed. It uses realistic home temperatures, so its energies are smaller than the engine's.

- **Engine (clockwise):** A→B→C→D between T_H = 600 K and T_C = 300 K. Readout: η = |W|/Q_H = 1 − T_C/T_H.
- **Heat pump (anticlockwise):** A→D→C→B for a home air-source heat pump, from 15 °C outdoor air (T_C = 288 K) to 50 °C hot water (T_H = 323 K). A→D adiabatic expansion cools the gas to T_C; D→C isothermal expansion absorbs Q_C from the cold side; C→B adiabatic compression heats it to T_H; B→A isothermal compression delivers Q_H to the hot side. Readout: COP = Q_H/|W| = T_H/(T_H − T_C).
- The overview title is "Carnot heat pump (home)", with the note "Outdoor air 15 °C → hot water 50 °C".
- Switching mode returns to state 0, with the direction arrows already flipped on the faint cycle and the reservoir labels showing the mode's temperatures.
- The ledger relabels Q_H as "delivered", Q_C as "absorbed" and the net work as "in".
- Ledger bars and arrow widths use one energy scale across both modes, so the heat pump's small net work is visibly small next to Q_H.

## Ledger

- **Bars:** "Q_H in" and "Q_C out" fill as the strokes run ("Q_H delivered" / "Q_C absorbed" for the heat pump). "Net work out" ("Net work in") shows "—" until the cycle closes, then grows to its value. Values are in kJ beside the bars.
- **Work record:** five boxes, W₁ (A→B) … W₄ (D→A) in run order, then the net. Convention-free: each box shows a magnitude and a direction, with no signs and no ΣW_k notation.
  - Engine: "W₁ (A→B) 3.46 kJ out", "W₂ (B→C) 3.74 kJ out", "W₃ (C→D) 1.73 kJ in", "W₄ (D→A) 3.74 kJ in"; net box "Net work out 1.73 kJ".
  - Heat pump: "W₁ (A→D) 0.44 kJ out", "W₂ (D→C) 1.66 kJ out", "W₃ (C→B) 0.44 kJ in", "W₄ (B→A) 1.86 kJ in"; net box "Net work in 0.20 kJ".
  - The adiabatic works cancel: equal amounts out and in.
- **Assumptions line**, always shown: "n = 1 mol, γ = 5/3, V_B/V_A = V_C/V_D = 2", generated from the physics inputs (V_C/V_D is computed, not assumed).
- **At the end of the cycle:** "Q_H = |W| + Q_C", then "η = |W|/Q_H = 1 − T_C/T_H = 50%" or "COP = Q_H/|W| = T_H/(T_H − T_C) = 9.23". |W| is the net work magnitude, so the equations hold whatever sign convention W carries.

## Physics model

1 mol of monatomic ideal gas (γ = 5/3, C_v = 3R/2) with V_B/V_A = 2. Two temperature pairs: the engine runs between T_H = 600 K and T_C = 300 K; the heat pump between T_H = 323 K and T_C = 288 K.

Isotherms follow pV = nRT and adiabats pV^γ = const. The adiabats fix the other two corners:

```latex
\frac{V_C}{V_B} = \frac{V_D}{V_A} = \left(\frac{T_H}{T_C}\right)^{1/(\gamma-1)}
```

Energies, as magnitudes: on an isotherm ΔU = 0 and the heat exchanged equals the work exchanged, nRT |ln(V_end/V_start)| (heat in and work out on expansion; work in and heat out on compression). On an adiabat Q = 0 and the work exchanged equals |ΔU| = nC_v|T_end − T_start| (work out = −ΔU on expansion, work in = ΔU on compression). Energies depend only on n, the temperatures, γ and the volume ratio, never on absolute volume or on the drawing.

Internally the physics module signs Q as heat into the gas and W as work done by the gas. The display shows only magnitudes with directions, so nothing on screen depends on that choice; this matches the course text (Hoch), where W is work done on the gas.

| Quantity | Engine (600 K / 300 K) | Heat pump (323 K / 288 K) |
| --- | --- | --- |
| Volumes V_A, V_B, V_C, V_D (relative) | 1, 2, 5.66, 2.83 | 1, 2, 2.38, 1.19 |
| Q_H = nRT_H ln 2 | 3.46 kJ | 1.86 kJ |
| Q_C = nRT_C ln 2 | 1.73 kJ | 1.66 kJ |
| \|W\| = Q_H − Q_C (net work) | 1.73 kJ | 0.20 kJ |
| Adiabatic work nC_v(T_H − T_C) | 3.74 kJ | 0.44 kJ |
| η = 1 − T_C/T_H | 50% | — |
| COP = T_H/(T_H − T_C) | 2 | 9.23 |
| Q_H/T_H and Q_C/T_C | 5.76 J/K each | 5.76 J/K each |

All values are computed in code from T_H, T_C, γ and V_B/V_A, so the full version can expose them as sliders without rework.

A console self-check runs on load (and on demand with `carnotSelfCheck()`). It compares the computed engine values with this table, and checks:
- the first law on every stroke in both modes;
- V_C/V_D = V_B/V_A;
- η and COP against their Carnot formulas;
- the home heat pump's Q_C ≈ 1.66 kJ, Q_H ≈ 1.86 kJ, net work ≈ 0.20 kJ and COP = 323/35 ≈ 9.23.

## Controls

Everything works from a presentation clicker, which sends Page Down/Up or arrow keys, so no trip to the laptop is needed.

| Action | Keys | On-screen |
| --- | --- | --- |
| Next state | →, Page Down, Space | Next button |
| Previous state | ←, Page Up | Back button |
| Reset to state 0 | Home | Reset button |
| Toggle engine / heat pump | M | Toggle in header |
| Full screen | F | Icon in header |

Other keys a clicker may send, such as B for a blank screen, are ignored. Stepping back jumps without animation. A key pressed mid-animation finishes the current step instantly, then acts. Space on a focused button cannot trigger twice.

## Visual design

Built for a lecture-theatre projector: light background, high contrast, readable from the back row.

- **Type:** labels at least 24 px at 1080p, header 36 px or larger.
- **Colour:** hot = red, cold = blue, insulation = grey hatching, work = purple. Every colour also carries a text label ("HOT T_H = 600 K"), so nothing depends on colour alone.
- **Arrows:** Q and W arrow widths are proportional to energy, on one scale across both modes, so the equal heat and work on an isotherm show as equal widths.
- **Theme:** light theme fixed regardless of the system's dark mode, because projectors wash out dark themes.
- **Motion:** respect reduced-motion settings by jumping between states instead of animating or fading.

## Build and hosting

One self-contained `index.html`: inline SVG, vanilla JavaScript, no libraries, no CDN, no build step. It must run offline from a double-clicked file.

- **Repo:** `carnot-cycle` containing `index.html`, a `README.md` with a GIF (still to record) and the key controls, and an MIT `LICENSE`.
- **Hosting:** GitHub Pages from `main`, root folder, live at dwaddo.github.io/carnot-cycle. The repo is public, so the live page can be tested well before 20 Nov.
- **Code shape:** a physics module (state points, curve sampling, energies, no DOM access) kept separate from the self-check and from rendering, so the full version reuses it.
- **Testing:** full screen on the lecture MacBook in Chrome and Safari at 1920×1080, plus one phone.
- **Fallback:** a screenshot of each state in the slide deck, in case the browser fails on the day.

## Out of scope (full version)

These stay on slides or out of the lecture, and move into the later full version.

- **Possible extension:** a Q/T line in the ledger showing Q_H/T_H = Q_C/T_C (5.76 J/K each), as a lead-in to entropy
- T_H and T_C sliders, and presets (power station, air conditioner, heat pump water heater, MRI magnet at 4 K, dilution fridge)
- T–S diagram view
- Refrigerator mode with COP_cool = T_C/(T_H − T_C)
- Other cycles: Otto, Diesel, Stirling, Brayton
- Finite-time and irreversible engines, including Curzon–Ahlborn efficiency
- Quiz or prediction prompts for self-study

## Decisions

- **Temperatures:** engine fixed at 600 K / 300 K; heat pump fixed at home values, 323 K / 288 K (50 °C hot water, 15 °C outdoor air); no presets in this version.
- **PV diagram:** schematic and not to scale, algebraic labels only; numbers appear only in the ledger, the gas temperature readout and the reservoir labels.
- **Sign convention:** none on screen. All work is shown as a magnitude with a direction (in/out), and the closing equations use |W|, to match the course text (Hoch, W = work done on the gas) without asking the audience to track signs.
- **Final step:** completing the cycle leaves the reservoir in place and shows no cylinder arrows; only the ledger totals and the net-W shading ease in.
- **Ledger:** values shown in kJ alongside the bars, with the assumptions (n, γ, volume ratios) stated beside them.
- **On-screen text:** minimal; explanation is spoken.
- **Entropy:** no T–S view and no entropy labels; that waits for the full version.
- **Publishing:** repo is public and served from GitHub Pages.
