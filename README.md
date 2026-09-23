# Proof of Presence

**Anti-Ghost-Worker System for NREGA**

*Build with Bharat 4.0 · National Level Hackathon*

🔗 **Live demo:** https://anish-2007-hub.github.io/Proof-of-presence-NREGA/

NMMS checks a worker in with one photo and one GPS ping, then checks them out the same way. **Proof of Presence** is a layer that keeps confirming, through nearby phones, that a worker is actually on site for the whole shift, even with zero signal.

---

## The Problem

NREGA guarantees up to 100 days of paid work a year to any rural household that asks for it, funded by tens of thousands of crores every year. But:

- **Ghost workers:** CAG audits and RTI replies keep turning up muster rolls with names logged for work that was never done, or workers checking in and slipping away early while being paid for the full day.
- **Only one moment is checked:** apps like NMMS verify a worker with a photo and GPS ping at check-in and check-out. Nothing in between is verified.
- **Collusion is easy, detection is hard:** a worker and a supervisor, or a small group, only need to coordinate around one check-in moment.
- **Connectivity reality:** most worksites have patchy or no signal, so any solution must work offline first.

## The Solution

Instead of one more check-in app, Proof of Presence verifies presence continuously through peer confirmation.

- Workers' phones (or cheap BLE tags for workers without smartphones) quietly form a local network at the worksite.
- Nearby devices ping each other over Bluetooth/NFC at random moments through the shift.
- A worker's presence is built from dozens of peer confirmations, not a single login.
- Everything runs offline and syncs automatically once there's signal.

### Why it's fraud-resistant

- Spoofing one phone's GPS is easy. Getting several nearby devices to fake being close together all day means organising a group.
- Phones stacked together to fake presence produce near-zero-distance signatures that get flagged.
- Gaps are **never auto-rejected**. They go to a human reviewer.
- It sits on top of the existing biometric check-in, so current compliance doesn't change.
- Honest workers aren't penalised for a dead battery. The system gives the benefit of the doubt by default.

## Live Simulation

The demo simulates one worksite through a 9:00–17:00 shift, with 10 workers and 15-minute presence slots.

| Scenario                          | What it shows                                                                                          |
| --------------------------------- | ------------------------------------------------------------------------------------------------------ |
| **Honest day**                    | Everyone stays on site and all workers verify.                                                         |
| **Ghost worker + early leaver**   | One worker checks in and leaves; another slips away in the afternoon. Both are flagged or sent to review. |
| **Collusion (phones stacked)**    | Absent workers' phones are left next to a real worker's. The near-zero-distance signature exposes them. |
| **Dead battery**                  | A present worker's phone dies for ~1.5 hours. They go to review with the benefit of the doubt.         |

**How to use it**

1. Choose a scenario.
2. Press **Play** (or drag the slider) to move through the shift. Orange lines show peer pings.
3. Press **Signal found — Sync** to upload the day's data.
4. Read each worker's timeline and status (Verified / Review / Flagged) and the **block officer review queue**.

> The data is fully simulated. It demonstrates the scoring logic and does not use real Bluetooth or any real worker information.

### Scoring rules (MVP)

- A slot counts as **confirmed** if at least one nearby peer phone pinged the worker during it.
- **Verified:** at least 85% of slots confirmed, with no gap longer than 30 minutes.
- **Review:** 60–85% confirmed, or a short gap that resumed.
- **Flagged:** under 60% confirmed, or a phone-stacking signature.
- Every flag goes to a block officer. Wages are held for review, never auto-rejected.

## How It Works

1. **Enrollment:** one-time signup with the Aadhaar-linked NREGA ID.
2. **Check-in:** the same biometric/geo check-in as today.
3. **Continuous pinging:** nearby phones (or ₹150–300 BLE tags) exchange random pings all shift.
4. **Local aggregation:** each phone stores peer confirmations offline until it picks up signal.
5. **Presence scoring:** once synced, the backend rebuilds each worker's presence timeline for the day.
6. **Flag & review:** unexplained gaps go to the block officer before wages are released.

## Planned Tech Stack

| Layer              | Technology                                                            |
| ------------------ | --------------------------------------------------------------------- |
| Mobile / Field     | React Native / Android (Kotlin), native BLE proximity APIs, SQLite    |
| Fallback hardware  | Low-cost BLE beacon tags (~₹150–300) issued by the site supervisor    |
| Backend            | Node.js (Express) / FastAPI, PostgreSQL, sync-on-connectivity design  |
| Anomaly detection  | Rule-based gap thresholds (MVP); lightweight anomaly-scoring model (stretch) |
| Dashboard          | React + Recharts: per-worker timeline, per-worksite supervisor view   |
| Integration        | Designed to plug into NMMS and reuse the Aadhaar-linked NREGA ID      |

> The hosted demo is a single-file front-end prototype. The stack above is the intended production architecture.

## Feasibility & Competitors

- Nearly every Android phone field staff carry supports BLE and NFC, so most people need no new hardware.
- Cheap BLE tags cover workers without smartphones.
- Can be piloted at a single Gram Panchayat worksite, with a block-level MGNREGA officer reviewing flags.

| Existing approach        | Limitation                                                                        |
| ------------------------ | --------------------------------------------------------------------------------- |
| NMMS (govt.)             | Geo-tagged photo at check-in/out only; nothing in between is verified.            |
| Biometric attendance     | One snapshot in time; easy to game by leaving right after marking present.        |
| Generic workforce trackers | Built for offices with constant Wi-Fi; break down at rural sites with no signal. |

**Our gap to fill:** as far as we could find, nothing built for NREGA verifies presence continuously through peer confirmation instead of a single checkpoint.

## Running Locally

The demo is one static HTML file, with no build step or dependencies.

```bash
git clone https://github.com/anish-2007-hub/Proof-of-presence-NREGA.git
cd Proof-of-presence-NREGA

# open index.html directly, or serve it:
python3 -m http.server 8000
# then visit http://localhost:8000
```

## References

- CAG audit reports on MGNREGA
- NMMS (National Mobile Monitoring Software)
- RTI-sourced data and news investigations on muster-roll gaps
- Ministry of Rural Development, MGNREGA guidelines

## Team

- **Team name:** [Your Team Name]
- **Members:** [Member 1, Member 2, Member 3, Member 4]
- **College:** [Your College Name]

## License

Add a license of your choice (e.g. MIT) before publishing.
