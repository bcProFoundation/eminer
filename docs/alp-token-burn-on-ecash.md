# Evaluation: ALP for intentional token burn on eCash (temple / White Lotus)

**Verdict: Yes — ALP is suitable and currently the right token layer for a burn-as-respect product on eCash.** Prefer ALP over legacy SLP. Do not plan on CashTokens (those are BCH-native; eCash would need a hard fork). TBP (Token Burn *Protection*) is a different problem and is largely unnecessary on eCash.

**Supply policy correction:** A **fixed supply is a poor fit** for Eastern memorial / rebirth symbolism. When every token is burned, offerings cannot continue. Prefer a **perpetual mint baton** so tokens can be **reborn** as new offerings are made. Track **cumulative burned** (merit) separately from **circulating** supply.

---

## 1. Correction: CashTokens vs ALP on eCash

| Protocol | Chain | Role |
|----------|-------|------|
| **CashTokens** | Bitcoin Cash (BCH) | Consensus-native tokens; **not** available on eCash without a hard fork |
| **SLP / eToken** | eCash (legacy) | OP_RETURN meta-protocol; Chronik-indexed |
| **ALP (Augmented Ledger Protocol)** | eCash | SLP successor (`SLP2` LokadID over **eMPP**); Chronik-indexed; intentional **BURN** supported in `ecash-lib` |

Earlier guidance that recommended CashTokens for eCash was wrong. For eCash apps, **ALP is the modern token path**.

Official ALP spec: [bitcoin-abc `doc/standards/alp.md`](https://github.com/Bitcoin-ABC/bitcoin-abc/blob/master/doc/standards/alp.md) (also published on ecashbuilders Notion).

---

## 2. What the linked TBP page is (and is not)

[Token burn protection (TBP)](https://ecashbuilders.notion.site/Token-burn-protection-TBP-protocol-14e9edc6e92f80abbb36ec1ca70adfe9) is about **accidental** burns: non-token wallets spending SLP/ALP UTXOs as plain sats and destroying tokens.

The same page states:

> This spec is only intended to be deployed on blockchains where non-token wallets are common. **eCash (XEC) wallets are already very aware of tokens, so this spec is not needed there.**

So for a temple on eCash:

- **TBP ≠ intentional offering burn**
- You do **not** need TBP to ship incense/flower burns
- You **do** need intentional burn encoding + indexing (ALP + Chronik + wallet UX)

---

## 3. Does ALP support intentional burns?

**Yes.**

1. **Protocol design:** ALP SEND verifies `∑ inputs ≥ ∑ outputs`. Destroying the difference is a burn (same family as SLP). ALP also defines an explicit **BURN** section type (envisioned in the spec; implemented in tooling).
2. **`ecash-lib`:** `alpBurn(tokenId, tokenType, burnAtoms)` builds intentional burn pushdata.
3. **`ecash-wallet` / Cashtab path:** ALP standard tokens support GENESIS / MINT / SEND / **BURN**. ALP can burn an exact amount and keep change in one flow; SLP often needs chained txs for exact intentional burns.
4. **Chronik:** Default token index covers **SLP and ALP**, with safety checks so XEC-only apps do not accidentally spend token UTXOs.

For a memorial offering, a burn tx can also carry **extra eMPP pushdata** (temple id, person id, offering type) beside the ALP BURN/SEND section — something SLP’s single-OP_RETURN monopoly made awkward.

---

## 4. Fit for “online temple / White Lotus” burns

| Requirement | ALP fit |
|-------------|---------|
| Intentional, attributable destruction of value | Strong — explicit burn + Chronik history |
| Branded offering token (e.g. White Lotus) | Strong — GENESIS with ticker/name/url/data |
| Regenerative / rebirth supply (never “runs out”) | Strong — keep mint baton(s) alive; ALP allows multiple batons |
| Metadata: who / which temple / flower vs incense | Strong — eMPP multi-section + app OP_RETURN |
| Wallet / indexer support on eCash | Strong — Chronik + Cashtab/`ecash-lib` stack |
| Consensus enforcement like CashTokens | **No** — still indexer rules; trust Chronik + wallet discipline |
| Accidental burn by random XEC wallet | Low risk on eCash (token-aware ecosystem); still educate users |

**Recommended token policy for offerings (rebirth / regenerative)**

Do **not** close the mint baton. ALP was explicitly improved over SLP to allow **multiple mint batons** and ongoing `MINT` while a baton input is present ([ALP spec](https://ecashbuilders.notion.site/ALP-a862a4130877448387373b9e6a93dd97)).

Preferred cycle:

```
devotee pays XEC (or holds tokens)
        ↓
   ALP MINT (rebirth) — baton stays alive
        ↓
   ALP BURN as offering + memorial metadata
        ↓
 cumulative burned ↑   circulating can stay small
```

Practical variants (pick one trust model):

| Model | How rebirth works | Trust | Spiritual fit |
|-------|-------------------|-------|----------------|
| **A. Mint-at-offering (recommended v1)** | App/temple holds baton; each offering mints then burns (or mints to user who burns) | Temple/app key | Simple “reborn when remembered” |
| **B. Multi-temple batons** | ALP multi-baton: one baton per temple/region | Each temple | Federated White Lotus network |
| **C. Permissionless PoW remint** | Mist/eminer-style covenant on the baton; anyone remints by work | Rules in script | Strongest “no earthly owner”; harder to build |
| **D. Burn-coupled remint** | Policy remints in proportion to recent burns / demand | Policy + baton custody or covenant | Closest to Lotus founder economics |

Product metrics to show in the temple UI:

- **Cumulative offerings burned** (never shrinks) — the spiritual ledger  
- **Circulating tokens** (can be near zero) — not the point of the ritual  
- **Alive mint baton(s)** — proof the flower can bloom again  

**Alternative (even simpler):** burn **native XEC** with OP_RETURN memorial tags (closest to current Lotus Temple XPI burns). Use ALP when you want a **named sacred token** with an explicit rebirth (mint) story.

---

## 5. ALP vs other burn options on eCash

| Approach | Pros | Cons |
|----------|------|------|
| **ALP intentional burn** | Branded token; exact burn; eMPP metadata; maintained stack | Meta-protocol; need Chronik; mint policy discipline |
| **Legacy SLP burn** | Older ecosystem familiarity | Worse endian/overflow/ghost-output footguns; weaker multi-protocol; exact burns harder |
| **Burn XEC only** | Simplest UX; max liquidity; no token mint politics | No separate “White Lotus” unit; offering is just cash |
| **CashTokens** | Consensus-native | **Not on eCash** without hard fork |
| **New L1 (White Lotus chain)** | Full monetary control | Ops burden; liquidity cold start |

---

## 6. Risks / implementation notes

1. **Indexer dependency:** Invalid ALP sections are discarded by indexers; consensus still moves the sats. Always build with Chronik validation before broadcast.
2. **Mint baton custody:** An open baton is *required* for rebirth, but whoever holds it can inflate supply. Mitigations: mint-only-at-offering (no free airdrops), rate limits, multi-sig/temple federation, or later a PoW/covenant baton (model C).
3. **OP_RETURN size:** Practical ALP output count is capped (~29 under current policy); fine for burns (usually 0–1 token change outputs).
4. **UX:** Cashtab users can hold/burn ALP; in-app temple wallet should use `ecash-lib` / `ecash-wallet` rather than hand-rolled SLP.
5. **Do not confuse TBP with product burns:** TBP is accidental-burn *prevention* for non-token chains; your product needs intentional burn *expression*.

---

## 7. Bottom line

**ALP is suitable — and is the best current eCash token protocol — for building intentional token burns for a White Lotus / online temple.**

Ship path:

1. Genesis an ALP token **with mint baton(s) kept alive** (rebirth enabled).  
2. Offering flow = `MINT` (rebirth) → intentional `BURN` (+ memorial metadata via eMPP), or user-held tokens then burn.  
3. Index with Chronik; show **cumulative burned** as the temple’s eternal record.  
4. Skip TBP, CashTokens, fixed-supply, and a new L1 unless requirements change.
