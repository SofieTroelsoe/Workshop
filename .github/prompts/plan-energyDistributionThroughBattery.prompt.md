## Plan: Battery Simulation Delivery

Create a beginner-friendly, modular battery simulation workflow from the README requirements: produce daily wind and home demand inputs, simulate battery charge/discharge with stop conditions, and present daily textual + visual output with pause/resume control.

**Steps**
1. Confirm and lock input datasets and field names from the README (`Windturbine_Daily_Produced.xlsx`, homes usage file) and normalize columns to `date`, `Windturbine_Produced_kwh_day`, `homes_shared_energy_usage_kwh_day`.
2. Implement data preparation helpers that parse dates (`YYYY-MM-DD`), coerce numeric values, and skip rows with missing values.
3. Implement battery simulation logic for each day: compute `surplus`, clamp battery range, calculate `Energy_Lost_Day_kwh`, and track cumulative loss.
4. Implement simulation stop rules:
- stop with `No_Energy_Produced` when daily wind production is 0
- stop with `Energy_Ran_Out` when `wind + battery < homes demand`
5. Implement display/update layer:
- textual daily output in exact order
- HTML visual panel (wind, battery, homes, arrows, loss labels)
- pause/resume interaction that preserves current day view while paused
6. Implement orchestration function/cell: initialize state, launch async loop with 1-second day ticks, cancel prior running task before restart.
7. Validate behavior: verify field order, rounding, date format, missing-data skip behavior, and both stop conditions using sample rows.
8. Align docs with implemented behavior: update README inconsistencies and state final authoritative battery capacity and input file mapping.

**Relevant files**
- `README.md` — source requirements and terminology to align.
- `notebooks/Windturbine_Daily_Produced.xlsx` — daily wind production input.
- `notebooks/Total_Usage_Houses.xlsx` (or selected homes workbook) — daily home demand input.
- `notebooks/Battery.ipynb` — modular simulation, controls, and visualization implementation.

**Verification**
1. Run notebook cells in order and confirm one simulated day advances per second.
2. Confirm output fields are exactly:
- `date`
- `Windturbine_Produced_kwh_day`
- `homes_shared_energy_usage_kwh_day`
- `Battery_Percentage`
- `Energy_Lost_Day_kwh`
3. Confirm formatting: kWh values 2 decimals, battery percentage integer, date `YYYY-MM-DD`.
4. Force test scenarios:
- wind = 0 -> `No_Energy_Produced`
- `wind + battery < demand` -> `Energy_Ran_Out`
- missing wind/home values -> day skipped
5. Pause/resume manually and verify simulation resumes from current state.

**Decisions**
- Included scope: core simulation, stop rules, daily output, pause/resume, visualization.
- Excluded scope: expanding to additional notebooks/agents unless requested.
- Open conflict to resolve: README lists battery capacity as both `100,000` and up to `2,500,000`.
