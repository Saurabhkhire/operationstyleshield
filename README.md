# StyleShield — Setup Guide
## Frontier Tower Hackathon | AI Safety & Evaluation Track

---

### Quick Start (5 minutes)

**Prerequisites:** Python 3.10+, Node.js 18+, npm

**1. Clone the repo**
```bash
git clone https://github.com/Saurabhkhire/operationstyleshield.git
cd operationstyleshield
git checkout experimental/combined-pipeline
```

**2. Install Python dependencies**
```bash
pip3 install flask flask-cors numpy pandas scikit-learn scipy
```

**3. Start the backend**
```bash
python3 api.py
```
You should see: `StyleShield API starting on http://localhost:5002`

**4. In a new terminal, start the frontend**
```bash
cd frontend
npm install
npm run dev
```
Opens at `http://localhost:5173` (or next available port — check terminal output)

**5. Use the app**
- Click **"Load demo dataset"** to analyze the bundled 53-account dataset
- Or click **"Upload CSV"** to analyze your own data
- Watch the console stream real analysis output
- Explore clusters, click accounts, view evidence
- Click **"Reveal ground truth"** to see detection accuracy
- Switch to **"Timeline Simulation"** tab to see 48hr flooding impact

---

### CSV Format

If you want to test with your own data, the CSV needs three columns:

```
account_id,post_text,posting_hour
jane_doe_42,"honestly this coffee shop is amazing lol",9
some_user_99,"Certainly the benefits are clear. Moreover the quality is high.",14
```

- `account_id` — any string, one per account
- `post_text` — the post content
- `posting_hour` — 0-23, hour of day posted

Each account should have 3+ posts for reliable analysis.

---

### What You're Seeing

StyleShield analyzes writing patterns across accounts to detect coordination. It does NOT use labels, metadata, or content moderation. Detection is purely structural:

- **Coordinated networks** = groups of accounts with suspiciously similar writing fingerprints
- **Organic (noise)** = accounts with unique writing that don't match anyone else
- **Model inference** = best guess at whether the coordination is LLM-generated or human-scripted

The demo dataset contains 53 accounts with realistic usernames. Some are real humans (2015 Twitter data). Some are AI-generated. Some are stealth bots designed to look human. The system doesn't know which is which — it detects coordination from writing style alone.

---

### Project Structure

```
operationstyleshield/
├── api.py                  # Flask backend
├── core/                   # Analysis engine
│   ├── Styleshield_script.py    # Base multi-metric scorer
│   ├── enhanced_extractor.py    # 40+ stylometric features
│   └── enhanced_pipeline.py     # Full pipeline with DBSCAN
├── frontend/               # React dashboard
├── demo/                   # Demo datasets
│   ├── demo_environment_anonymized.csv    # Balanced (53 accounts)
│   ├── demo_ground_truth.json             # Answer key
│   ├── demo_flood_environment.csv         # Bot-heavy (81% bots)
│   └── demo_flood_ground_truth.json
└── docs/                   # Documentation
```
