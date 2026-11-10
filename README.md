# carnot-cycle

A single-page lecture widget for the ideal-gas Carnot cycle. It steps through the four strokes (A→B→C→D) on a schematic PV diagram, showing the piston, the reservoir or insulation, and heat and work arrows sized by energy. A ledger records Q_H, Q_C and the work on each stroke, W₁…W₄, and closes with Q_H = W + Q_C and η = 1 − T_C/T_H. A toggle runs the identical cycle backwards as a heat pump, with COP = T_H/(T_H − T_C). Every number is computed from T_H = 600 K, T_C = 300 K, γ = 5/3 and V_B/V_A = 2 (1 mol of monatomic ideal gas). The PV diagram is drawn schematically, not to scale. Everything is in one self-contained `index.html` with no libraries, so it also runs offline from a double-clicked file. Open the browser console to see a self-check of the computed values against the spec.

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
