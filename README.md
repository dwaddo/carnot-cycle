# carnot-cycle

A single-page lecture widget for the ideal-gas Carnot cycle. It steps through the four strokes (A→B→C→D) on a schematic PV diagram, showing the piston, the reservoir or insulation, and heat and work arrows sized by energy. A ledger records Q_H, Q_C and the work on each stroke, W₁…W₄, and closes with Q_H = |W| + Q_C and η = 1 − T_C/T_H. A toggle runs the cycle backwards as a home heat pump (15 °C outdoor air to 50 °C hot water, T_C = 288 K and T_H = 323 K), with every arrow reversed and COP = T_H/(T_H − T_C) ≈ 9.23. Every number is computed from T_H, T_C, γ = 5/3 and V_B/V_A = 2 for 1 mol of monatomic ideal gas; the engine uses T_H = 600 K and T_C = 300 K. Work labels are convention-free (a magnitude plus "in" or "out"), so they match the course text's convention, where W is work done on the gas. The PV diagram is drawn schematically, not to scale. Everything is in one self-contained `index.html` with no libraries, so it also runs offline from a double-clicked file. Open the browser console to see a self-check of the computed values against the spec.

**Live:** https://dwaddo.github.io/carnot-cycle

<!-- TODO: add demo GIF here -->

## Controls

Works with a presentation clicker.

| Action | Keys | On-screen |
| --- | --- | --- |
| Next state | →, Page Down, Space | Next button |
| Previous state (no animation) | ←, Page Up | Back button |
| Reset to overview | Home | Reset button |
| Toggle engine / heat pump | M | Toggle in header |
| Full screen | F | Icon in header |

Other keys (such as B for a blank screen) are ignored.

## License

MIT, see [LICENSE](LICENSE).
