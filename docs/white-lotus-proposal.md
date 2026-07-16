# Proposal: White Lotus — Ergon-like ritual coin

**Recommendation: build White Lotus as an Ergon-like ALP token on eCash first. Do not launch an L1 unless the token proves ritual demand and outgrows eCash rails.**

---

## 1. Decision

| Option | Role |
|--------|------|
| **A. White Lotus on eCash (ALP + PoW remint)** | **Primary — ship this** |
| **B. White Lotus L1 (eCash/Ergon-style fork)** | Contingency only — after product-market fit |

Ritual need (vàng mã sacrifice + rebirth + commons) is satisfied by **issuance rules**, not by owning a blockchain. eCash already gives maintenance, Chronik, wallets, and Agora. An L1 buys fork-fairness and sovereign monetary policy at the cost of becoming a chain operator.

---

## 2. Why A over B (now)

| Criterion | ALP on eCash | New L1 |
|-----------|--------------|--------|
| Ergon-like `mint ∝ work` | Yes — PoW mint-baton covenant | Yes — `GetBlockSubsidy ∝ difficulty` |
| Vàng mã burn + rebirth | Yes — burn + perpetual baton | Yes — native burn + subsidy |
| Holder-capture vs Lotus `log(D)` | Avoided if mint is linear in token work | Avoided if subsidy is linear in `D` |
| Ops burden | App + covenant + miner | Full node, miners, explorers, upgrades forever |
| User acquisition of offering | XEC → Agora/desk → burn | Need exchange/liquidity for new coin |
| Maintenance of base chain | Bitcoin ABC / eCash | You |
| Fork-energy fairness | Soft (token PoW) | Strong (L1 work) |
| Time to a working temple | Much shorter | Much longer |

Lotus Temple already showed: **elastic but inelastic issuance (`log D`) + burns** enriched holders. Fix issuance, don’t fork a nation-state chain on day one.

---

## 3. Product architecture (Option A)

### 3.1 Narrative

- **White Lotus (hoa sen trắng)** — Vietnamese mourning / purity symbol  
- Cycle: *effort remints the flower → devotee burns it → merit is public → flower can bloom again*  
- Cumulative burned = spiritual ledger; circulating supply is secondary  

### 3.2 Token

| Item | Choice |
|------|--------|
| Host | eCash (XEC) |
| Protocol | **ALP** (`SLP2` / eMPP) |
| Ticker (example) | `WLOTUS` / `WLTS` |
| Mint authority | **Permissionless PoW covenant** holding the mint baton |
| Burn | Intentional `alpBurn` + memorial metadata (person / temple / offering tier) |
| Indexer | Chronik |
| Liquidity | Agora + optional temple desk (XEC ↔ token) |

### 3.3 Issuance (Ergon-like MVP → v2)

**MVP (ship first)**

- Fixed token PoW difficulty (tunable)  
- Fixed atoms per successful remint  
- CLTV / pacing so remints don’t spam  
- **No supply cap** — baton never dies  
- Optional mild Moore decay on mint atoms later  

**v2 (if burns + hashrate justify it)**

- Token-local DAA  
- `mintAmount ∝ work(D)` (Ergon linear analogue)  
- Moore/Koomey decay on the proportionality constant  

This is the ritual-critical difference from Lotus Temple: issuance must **answer work**, not crawl like `log(D)`.

### 3.4 App flow

```
Devotee opens memorial page
  → acquires WLOTUS (Agora / desk / gifted)
  → chooses offering (flower / incense / candle = burn tiers)
  → alpBurn + eMPP memorial payload
  → Chronik + API update cumulative merit

Parallel:
  Miners race for baton → remint → sell/provide liquidity
```

Reuse `app-lotus-temple` UX; retarget settlement from XPI burns to ALP burns on eCash.

### 3.5 Optional dual baton

ALP allows **multiple mint batons**:

1. **PoW baton** — permissionless rebirth (canonical)  
2. **Temple baton** — cold-start / emergency only, policy-limited, ideally time-locked or multi-sig  

Prefer retiring (2) once miners + liquidity exist.

---

## 4. Option B — L1 (when, and only when)

Consider a White Lotus / Ergon-like L1 **only if**:

1. Temple has sustained burn volume and cultural adoption  
2. eCash fee / policy / tooling constraints block the ritual  
3. You accept permanent chain ops (or a funded commons to run them)  
4. You want hard fork-fairness (“energy can’t be counted twice”) as a first-class property  

Then: fork a maintained UTXO codebase (eCash lineage or Bitcoin Static ideas), set **`subsidy ∝ difficulty` + Moore decay**, **0% founder fund**, fee burn optional, genesis branded White Lotus. Port the temple app to native burns.

Until then, L1 is premature optimization of sovereignty.

---

## 5. Phased delivery (Option A)

| Phase | Deliverable |
|-------|-------------|
| **0** | Spec: covenant rules, mint formula, burn metadata LOKAD, baton policy |
| **1** | GENESIS + Chronik indexing + temple burn UI on eCash (custodial remint OK for dogfood) |
| **2** | PoW remint covenant + miner (eminer → Chronik/`ecash-lib`) |
| **3** | Agora market + public cumulative-burn explorer |
| **4** | Tune toward `mint ∝ D` if data supports it |
| **5** | Revisit L1 only with evidence from 1–4 |

---

## 6. Explicit non-goals (v1)

- USD stablecoin  
- Fixed max supply  
- Lotus-style `log(D)` inelastic subsidy  
- Launching a new L1 to “build community faster”  
- Relying on TBP (not needed on eCash)  

---

## 7. One-line decision

**White Lotus = Ergon-like ALP on eCash (PoW remint + burn-as-vàng-mã). L1 only after the ritual works.**
