# Carnot Cycle Widget — Spec (Lecture Version)

Last updated: 10 Nov 2026

## Purpose and scope

A single-page, click-through view of the four strokes of the ideal-gas Carnot cycle, with an engine / heat pump toggle. It supports the whiteboard derivation in the PHYS3113 interview lecture (UNSW, 20 Nov 2026) rather than replacing it.

This is the sparing lecture version. A separate full version (sliders, presets, T–S diagram, Otto and other cycles) will be built later from the same code; see Out of scope.

## Lecture use

The widget appears in three short visits, each under 3 minutes, between whiteboard work.

1. **Build the cycle (~8–16 min):** step through A→B→C→D after reversibility is introduced, alongside or just after the board derivation of η = 1 − T_C/T_H.
2. **Run it backwards (~19–25 min):** flip to heat pump mode during Carnot's theorem, and use the COP to resolve the opening "400% efficient" puzzle.
3. **Close the cycle (~25–30 min):** show the completed ledger (Q_H = W + Q_C); the Q/T ratio is worked on the whiteboard.

## Layout

One 16:9 screen with four regions, scaled to fill the window; nothing scrolls.

| Region | Position | Contents |
| --- | --- | --- |
| Header | Top strip | Step title (e.g. "A→B: isothermal expansion at T_H"), step counter (1 of 4), mode toggle at right |
| PV diagram | Left, ~55% width | The four strokes labelled A–D, a moving state dot, axes p and V without numeric ticks |
| Cylinder | Right, ~45% width | Piston and gas; the block underneath changes: hot reservoir, insulation or cold reservoir; Q and W arrows |
| Ledger | Bottom strip | Running Q_H, W, Q_C as bars with values, plus a net-work total at the end of the cycle |

On narrow screens (under ~700 px, i.e. phones) the PV diagram and cylinder stack vertically.

## Steps (engine mode)

Six states: an overview, four strokes, and a completed cycle. Labels A–D follow the existing lecture notes.

| State | Stroke | Block under cylinder | Heat | Work | Gas temperature |
| --- | --- | --- | --- | --- | --- |
| 0 Overview | Full cycle faint, dot at A | Hot reservoir | — | — | T_H |
| 1 A→B | Isothermal expansion | Hot reservoir | Q_H in | Out | Stays T_H |
| 2 B→C | Adiabatic expansion | Insulation | None | Out | Falls T_H → T_C |
| 3 C→D | Isothermal compression | Cold reservoir | Q_C out | In | Stays T_C |
| 4 D→A | Adiabatic compression | Insulation | None | In | Rises T_C → T_H |
| 5 Complete | Enclosed area shaded, labelled "net W" | Hot reservoir | Ledger totals | Net out | T_H |

On entering a stroke, the dot and piston animate together along it (about 1.5 s). Completed strokes stay solid, the current one is thick, and future ones stay faint. The header gives a one-line note per stroke, e.g. "ΔU = 0, so all heat in becomes work out."

## Engine / heat pump toggle

Heat pump mode runs the identical cycle anticlockwise: same curves, same energies, every arrow reversed. That sameness is the teaching point.

- **Engine (clockwise):** A→B→C→D. Readout: η = W/Q_H = 1 − T_C/T_H.
- **Heat pump (anticlockwise):** A→D→C→B. A→D adiabatic expansion cools the gas to T_C; D→C isothermal expansion absorbs Q_C from the cold side; C→B adiabatic compression heats it to T_H; B→A isothermal compression delivers Q_H to the hot side. Readout: COP = Q_H/W = T_H/(T_H − T_C).
- Switching mode returns to state 0 with the direction arrows already flipped on the faint cycle.
- The ledger relabels W as "in" and Q_C as "absorbed".

## Physics model

1 mol of monatomic ideal gas (γ = 5/3) between T_H = 600 K and T_C = 300 K, with V_B/V_A = 2. These give η = 50% and COP = 2, and a cycle wide enough to read when projected.

Curves are computed, not sketched: isotherms from pV = nRT, adiabats from pV^γ = const. The adiabats fix the other two corners:

```latex
\frac{V_C}{V_B} = \frac{V_D}{V_A} = \left(\frac{T_H}{T_C}\right)^{1/(\gamma-1)} = 2^{3/2} \approx 2.83
```

| Quantity | Value |
| --- | --- |
| Volumes V_A, V_B, V_C, V_D (relative) | 1, 2, 5.66, 2.83 |
| Q_H = nRT_H ln 2 | 3.46 kJ |
| Q_C = nRT_C ln 2 | 1.73 kJ |
| W = Q_H − Q_C | 1.73 kJ |
| Q_H/T_H and Q_C/T_C | 5.76 J/K each |

All values are computed in code from T_H, T_C, γ and V_B/V_A, so the full version can expose them as sliders without rework.

## Controls

Everything works from a presentation clicker, which sends Page Down/Up or arrow keys, so no trip to the laptop is needed.

| Action | Keys | On-screen |
| --- | --- | --- |
| Next state | →, Page Down, Space | Next button |
| Previous state | ←, Page Up | Back button |
| Reset to state 0 | Home | Reset button |
| Toggle engine / heat pump | M | Toggle in header |
| Full screen | F | Icon in header |

Other keys a clicker may send, such as B for a blank screen, are ignored. Stepping back jumps without animation.

## Visual design

Built for a lecture-theatre projector: light background, high contrast, readable from the back row.

- **Type:** labels at least 24 px at 1080p, header 36 px or larger.
- **Colour:** hot = red, cold = blue, insulation = grey hatching. Every colour also carries a text label ("HOT 600 K"), so nothing depends on colour alone.
- **Arrows:** Q and W arrow widths are proportional to energy, so Q_H visibly splits into W and Q_C.
- **Theme:** light theme fixed regardless of the system's dark mode, because projectors wash out dark themes.
- **Motion:** respect reduced-motion settings by jumping between states instead of animating.

## Build and hosting

One self-contained `index.html`: inline SVG, vanilla JavaScript, no libraries, no CDN, no build step. It must run offline from a double-clicked file.

- **Repo:** `carnot-cycle` containing `index.html`, a `README.md` with a GIF and the key controls, and an MIT `LICENSE`.
- **Hosting:** GitHub Pages from `main`, root folder. The repo goes public early, so the live page can be tested well before 20 Nov.
- **Code shape:** a physics module (state points, curve sampling, energies) kept separate from rendering, so the full version reuses it.
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

- **Temperatures:** fixed at 600 K / 300 K; no presets in this version.
- **Ledger:** values shown in kJ alongside the bars.
- **Entropy:** no T–S view and no entropy labels; that waits for the full version.
- **Publishing:** repo goes public early.
