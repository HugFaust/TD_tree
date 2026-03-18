# TD Signal Lab — Architecture

## Project Structure

td-signal-lab/
│
├── config/                        # All rules live here — no logic, just definitions
│   ├── setups/
│   │   ├── setup_1.json
│   │   ├── setup_2.json
│   │   └── setup_3.json
│   ├── countdowns/
│   │   ├── td_sequential.json
│   │   └── td_combo.json
│   └── cancellation_rules/
│       ├── rule_A.json
│       └── rule_B.json
│
├── engine/                        # Logic that reads configs and runs the signal
│   ├── setup_engine.py
│   ├── countdown_engine.py
│   └── cancellation_engine.py
│
├── combinations/
│   └── combo_registry.py          # Enumerates all valid combinations
│
├── data/
│   └── raw/                       # Price data goes here
│
├── backtest/
│   └── runner.py                  # Runs each combo against data
│
└── visualisation/
    └── dashboard.py               # Displays results per combo


## Key Principle

Separate config from logic. Every rule — what constitutes each setup, 
what parameters apply to each countdown, what triggers a cancellation — 
lives in config/ as a JSON file. The engine reads those files and executes 
them. This means:

- Adding a new rule = add a new config file, touch nothing else
- Changing a parameter = edit one file, re-run everything
- Git tracks rule changes cleanly and separately from code changes


## Workflow

1. Define all rules in config/
2. combo_registry.py automatically generates every valid combination
3. runner.py loops through every combo, applies it to price data, logs results
4. dashboard.py lets you filter, compare and visualise outcomes by combo


## Build Order

Always build and verify one full combo end-to-end before expanding:
Setup 1 + TD Sequential + 8 vs Last + one cancellation rule → working signal → then scale.