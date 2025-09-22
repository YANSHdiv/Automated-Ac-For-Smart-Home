# 🌬️ Automated AC

An intelligent **Tkinter GUI** app that works on different **agents** to suggest optimal AC settings based on your room conditions, preferences, and energy limits. It remembers similar situations, estimates energy use, and even plans for comfort & efficiency!

---

## 📌 Features

✅ User-friendly GUI with **Tkinter**  
✅ Random simulation of **room temperature & humidity**  
✅ Memory-based reuse of past scenarios (reflex agent)  
✅ Goal-based planning for comfort & energy savings  
✅ Estimates **hourly & daily energy consumption**  
✅ Handles AC type, tonnage, compressor, ISEER, room size, people, heat, movement, timing, position  
✅ Shows **previous vs. new output** side-by-side  
✅ Stores all data in motoko canister.

---

## 🧠 How it Works

1. **Inputs:** You enter AC specs, room details, people count, heat sources, energy limit, and more.
2. **Simulation:** Generates random room temp & humidity.
3. **Decision Logic:**  
   - Suggests AC temp based on thresholds.
   - Adjusts mode (Cool, Dry, Eco, Sleep).
   - Sets fan speed & flap direction.
   - Compares with past scenarios for reuse.
   - Adjusts if estimated energy exceeds your budget.
4. **Output:** Displays suggestions & energy use estimate.

---

## 🔑 Conditions & Information Used

- **AC Type:** Window, Split, Cassette, Portable
- **Tonnage:** 0.8 to 4.0 tons
- **ISEER:** User input
- **Compressor Type:** Reciprocating, Inverter Rotary, Fixed Speed Rotary, Scroll
- **Room Size, Number of People, External Heat**
- **Affordable Units:** Units/day limit
- **Person Movement:** Yes/No
- **Timing:** 24-hour format
- **Position:** Clock angle (e.g., 11o)

**Decision Rules Include:**
- Room temp ≥ 38°C → AC temp = 18°C  
- Humidity > 60% → Dry Mode  
- Zero people → Eco Mode  
- No movement at night → Sleep Mode  
- People count & temp decide fan speed  
- Clock position decides flap direction  
- Base combo units adjust total energy estimate  
- If units > budget, adjust AC temp & mode to save energy

**⚙️ 6. Base Unit Consumption Logic (AC Type + Compressor Type)**

This logic adds a base energy consumption value depending on the AC + compressor combo:

-Split AC + Inverter Rotary Compressor → 10 kWh/day

-Cassette AC + Inverter Rotary Compressor → 11 kWh/day

-Split AC + Scroll Compressor → 11 kWh/day

-Cassette AC + Scroll Compressor → 12 kWh/day

-Window AC + Scroll Compressor → 13 kWh/day

-Window AC + Reciprocating Compressor → 14 kWh/day

-Portable AC + Fixed Speed Rotary Compressor → 15 kWh/day

-Explanation: Different hardware combinations consume different base power.
 


*Many of the logics are used instead of a hardware sensor.

---

## Setup

Use Linux for ease (due to Motoko) or in Window run by WSL.

### Create Virtual Environment (optional but recommended)
```bash
python -m venv venv
source venv/bin/activate   # On Linux/Mac
venv\Scripts\activate      # On Windows
```
### Install Dependencies
```bash
pip install -r requirements.txt
tkinter
requests
```
⚠️ tkinter usually comes pre-installed with Python. If not, install via system package manager (e.g., sudo apt-get install python3-tk).



To get started, you might want to explore the project directory structure and the default configuration file. Working with this project in your development environment will not affect any production deployment or identity tokens.

To learn more before you start working with `smart_ac`, see the following documentation available online:

- [Quick Start](https://internetcomputer.org/docs/current/developer-docs/setup/deploy-locally)
- [SDK Developer Tools](https://internetcomputer.org/docs/current/developer-docs/setup/install)
- [Motoko Programming Language Guide](https://internetcomputer.org/docs/current/motoko/main/motoko)
- [Motoko Language Quick Reference](https://internetcomputer.org/docs/current/motoko/main/language-manual)

If you want to start working on your project right away, you might want to try the following commands:

```bash
cd smart_ac/
dfx help
dfx canister --help
```

## Running the project locally

If you want to test your project locally, you can use the following commands:

```bash
# Starts the replica, running in the background
dfx start --background

# Deploys your canisters to the replica and generates your candid interface
dfx deploy
```

Once the job completes, your application will be available at `http://localhost:4943?canisterId={asset_canister_id}`.

If you have made changes to your backend canister, you can generate a new candid interface with

```bash
npm run generate
```

at any time. This is recommended before starting the frontend development server, and will be run automatically any time you run `dfx deploy`.

If you are making frontend changes, you can start a development server with

```bash
npm start
```

Which will start a server at `http://localhost:8080`, proxying API requests to the replica at port 4943.

### Note on frontend environment variables

If you are hosting frontend code somewhere without using DFX, you may need to make one of the following adjustments to ensure your project does not fetch the root key in production:

- set`DFX_NETWORK` to `ic` if you are using Webpack
- use your own preferred method to replace `process.env.DFX_NETWORK` in the autogenerated declarations
  - Setting `canisters -> {asset_canister_id} -> declarations -> env_override to a string` in `dfx.json` will replace `process.env.DFX_NETWORK` with the string in the autogenerated declarations
- Write your own `createActor` constructor
