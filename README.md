# Nori ACARS — download

**[Download NoriACARS-Setup.exe (latest)](https://github.com/txsty364/altivus-test/releases/latest/download/NoriACARS-Setup.exe)**

Run it, sign in with your Nori account (email + password), pick your simulator, and it installs.
Windows may say "Windows protected your PC" because the installer isn't code-signed yet:
click **More info → Run anyway**.

This repository only hosts the installer. Questions: ask on the Nori Discord.

---

# Testing Nori ACARS with MSFS (and Prepar3D)

Nori ACARS connects to Microsoft Flight Simulator 2020/2024 and Prepar3D through **SimConnect**. This
connection is new and hasn't run against a real simulator yet. This guide covers setting it up and the
first test flight.

## 1. Set up (once)

1. **SimConnect.dll is usually found automatically.** Nori ACARS uses the copy that other MSFS tools already
   installed (MobiFlight Connector, Little Navmap, FSUIPC7, Volanta, Navigraph…). The recording window shows
   "SimConnect ✓" when it's found. Only if it says *not found*, get it from the free MSFS SDK:
   - In MSFS: *Options › General Options › Developers* → turn on **Developer Mode**.
   - In the developer menu bar that appears: *Help › SDK Installer* → download and run the SDK installer
     (the "Core" part is enough).
   - The SDK installs to `C:\MSFS 2024 SDK` or `C:\MSFS SDK` and sets the `MSFS2024_SDK` / `MSFS_SDK`
     environment variable. Nori ACARS looks there automatically. Restart Nori ACARS after installing.
   - Elsewhere? In `%LOCALAPPDATA%\NoriACARS\settings.json`, set
     `"SimConnectDll": "D:\\path\\to\\SimConnect SDK\\lib\\SimConnect.dll"`.
   - Prepar3D: install the Prepar3D SDK and point `SimConnectDll` at its `SimConnect.dll`.
2. **Choose the simulator.** The installer does this when you pick MSFS. Otherwise, in `settings.json`, set
   `"Simulator": "msfs2024"` (or `"msfs2020"`, or `"p3d"`) and restart Nori ACARS.
3. Start the simulator and load a flight. The top-left corner should turn green: **Simulator connected**.
   *Details* shows what was read: simulator version, the aircraft, its add-on, and which variables are
   available or missing.

If it doesn't connect, hover the status text for the reason ("SimConnect.dll wasn't found", "isn't
running", …). The log is under *Details › Open logs*.

## 2. First flight checklist

Fly the aircraft Nori will use and check these in *Details*. Note anything that's wrong (screenshots help).

| Check | Expected |
|---|---|
| Aircraft name, type and registration | Match the loaded aircraft; add-on recognised (Fenix, FlyByWire, iniBuilds, PMDG) |
| Position, altitude, speeds | Match the sim's own readouts |
| **Pitch** | Positive when the nose is up |
| **Bank** | Positive when the right wing is down |
| Heading | True heading, 0–360 |
| On ground | Flips at liftoff and touchdown |
| Fuel and weight | In kg, sensible values |
| Gear, flaps, speed brake, parking brake | Follow the levers |
| Pause and sim rate | Pause shows paused; rate matches |

**Lights, autopilot, seatbelt signs, APU, battery and external power** are read from the simulator's stock
variables. On the default aircraft they're trusted. On add-ons (Fenix, FlyByWire, PMDG…) they show as
**unverified**, because those aircraft often don't move the stock variables. The next step, aircraft
profiles, maps each add-on's own switches from recorded flights.

## 3. Aircraft test recording (the A319 / A320 / A321 / A350 laps)

This records everything the cockpit does during one guided lap, so Nori can work out which value is
which switch on each aircraft.

**Before you start**
- The recording window checks for the free **MobiFlight WASM module**. If it isn't installed, press
  **Install**: it downloads the official module from MobiFlight's GitHub into your MSFS Community folder.
  Restart MSFS afterwards. Without it, only the simulator's standard variables are recorded, which miss
  most Fenix/iniBuilds switches.
- Staff can watch your recording **live** while you fly ("Live: connected" in the window).
- Load the aircraft **cold & dark at a gate** with a runway nearby for a quick lap.

**Recording**
1. Nori ACARS → **Details** → **Aircraft test recording…** A small window opens and stays on top.
2. Check the script: *Airbus A319 / A320 / A321 lap* or *Airbus A350 lap* (picked automatically from the
   aircraft type).
3. Press **Start recording**. It finds MobiFlight and lists the aircraft's L-vars (a few seconds), then shows
   "Recording N variables". **Changing now** shows values moving as you click things: if that list
   moves when you flip a switch, it's working.
4. Do the step shown, then press **Done** (or **Ctrl+Alt+N** without leaving the sim; you'll hear a ding).
   - Step doesn't exist on this aircraft? **Skip** (Ctrl+Alt+S).
   - Pressed Done too early? **Back** (Ctrl+Alt+B) and do it again.
   - Anything odd? Type a **note** before pressing Done.
   - Wait about two seconds between doing a step and pressing Done, and don't do two steps at once.
5. At the end press **Stop & save**. The recording is **sent to Nori automatically** ("Sent to Nori ✓").
   If sending fails, press **Send again**, or use **Open folder** and send us the `.nori-capture.jsonl.gz`
   file another way. Do one recording per aircraft (A319, A320, A321, A350).

## 4. What's not done yet

- Aircraft profiles for add-ons (L-vars), built from these recordings.
- Reading MSFS/P3D scenery as a backup for taxiway data (the Perf page uses real-world data meanwhile).
- Shipping SimConnect.dll with the installer, so pilots don't need the SDK.
