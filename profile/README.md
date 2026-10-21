# sleepypod

Self-hosted control, scheduling, automation, and biometrics for Pod mattress covers (Pod 3, 4, and 5). Core runs locally on the Pod's embedded Linux; companion apps and training tools run on your own devices. No cloud account, no internet required.

**Video tour:** [See sleepypod Core in action](https://sleepypod.github.io/core/tour/)

**Docs:** [sleepypod.github.io](https://sleepypod.github.io/) · **Live demo:** [sleepypod.vercel.app](https://sleepypod.vercel.app) · **Community:** [Discord](https://discord.gg/UMmv5R6MXa) · **License:** AGPL-3.0

## Repositories

### [sleepypod Core](https://github.com/sleepypod/core)

The server that runs on the Pod. A local web app for per-side temperature, schedules, vibration alarms, and daily maintenance; Autopilot rules that react to signals and conditions, with backtests against recorded nights; on-device biometrics (heart rate, HRV, breathing rate, sleep staging) from the Pod's own sensors. Opt-in bridges for Home Assistant (MQTT) and Apple Home (HomeKit), and a Model Context Protocol server so an AI agent can read or control the Pod.

**Stack:** TypeScript, Next.js, React, tRPC, SQLite, Drizzle, Python biometrics sidecars

### [sleepypod iOS](https://github.com/sleepypod/ios)

Native iOS companion for temperature control and sleep tracking. Radial dial, schedule curves, biometrics charts, on-device sleep stage classification, and system health. Writes each night's stages and vitals to Apple Health and compares any night with what an Apple Watch recorded. Finds the Pod automatically over mDNS. A TestFlight beta is in progress; until then, build it from source with Xcode.

**Stack:** Swift 6, SwiftUI, Swift Charts · iOS 26+

### [sleepypod Dial](https://github.com/sleepypod/m5-rotary-dial)

A bedside controller built on the M5Stack Dial (ESP32-S3) for one side of the bed. Turn to set a target, click for off, and a red-on-black night theme after 10 pm. Includes a two-part printable enclosure, also on [MakerWorld](https://makerworld.com/en/models/3365781-sleepypod-dial-enclosure-for-m5stack-dial).

**Stack:** C++, PlatformIO, ESP32-S3

### [sleepypod Training](https://github.com/sleepypod/training)

A local workspace for training and evaluating sleep-stage models. Watch training progress live, reopen historical runs, and compare Pod, candidate, baseline, and Apple Watch stages side by side. Includes explanations of technical terms, an OKF knowledge base, and portable model exports. Reads data from the Pod's local API and saves candidates on your computer without activating them.

**Stack:** Python, scikit-learn, NumPy, SciPy, TypeScript · [Training guide](https://sleepypod.github.io/developers/training/) · [Screenshots and setup](https://github.com/sleepypod/training#training-workspace)

### [sleepypod.github.io](https://github.com/sleepypod/sleepypod.github.io)

The documentation site: product guides, developer reference, and real captures of every client.

**Stack:** Next.js, Nextra

## Ecosystem

```mermaid
graph TD
    subgraph Pod ["Pod Hardware"]
        HW["Sensors / Pumps / Heaters"]
        DAC["dac.sock"]
    end

    subgraph OnPod ["Running on Pod"]
        CORE["sleepypod Core"]
        BIO["Biometrics Sidecars"]
    end

    BROWSER["Browser"]
    IOS["sleepypod iOS<br/><i>Swift · iOS</i>"]
    DIAL["sleepypod Dial<br/><i>C++ · ESP32-S3</i>"]
    HA["Home Assistant"]
    HOME["Apple Home"]
    AGENT["AI agent"]
    TRAINING["sleepypod Training<br/><i>Local computer · Python</i>"]

    BROWSER -- "HTTP" --> CORE
    IOS -- "Local API" --> CORE
    DIAL -- "Local API" --> CORE
    HA <-- "MQTT" --> CORE
    HOME <-- "HomeKit" --> CORE
    AGENT -- "MCP" --> CORE
    TRAINING -- "Read-only local API" --> CORE

    CORE <-- "Unix Socket" --> DAC
    CORE --> BIO
    DAC --- HW
```

All communication stays on your local network. Core keeps the Pod behind a LAN-only firewall policy, and the HomeKit and MQTT bridges are off until you enable them. sleepypod collects no data; see the [privacy policy](https://sleepypod.github.io/privacy/).

## Getting Started

Start with the [getting started guide](https://sleepypod.github.io/getting-started/), or try the [live demo](https://sleepypod.vercel.app) first. Coming from free-sleep? Follow the [migration guide](https://sleepypod.github.io/core/migrating-from-free-sleep/). See the [core README](https://github.com/sleepypod/core#readme) for installation details.
