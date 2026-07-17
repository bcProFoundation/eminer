# How to publish (Environment is locked)

This Cloud Agent Environment can only push to existing **`bcProFoundation/*`** repos.  
It cannot gain write access to `danaverse/*` or `nghiacc/*` while the Environment repo list is locked.

Local `main` is complete. Artifacts are ready:

- `/opt/cursor/artifacts/wlotus/wlotus-main.bundle`
- `/opt/cursor/artifacts/wlotus/wlotus-source.zip`

---

## Option A — Push via `bcProFoundation` (agent can finish)

1. Create an **empty** public repo: https://github.com/organizations/bcProFoundation/repositories/new  
   - Name: **wlotus**  
   - **No** README / .gitignore / license  
2. Reply: **`bcProFoundation/wlotus created, push`**  
3. Agent pushes `main` there.  
4. Optional: GitHub → Settings → **Transfer ownership** → `danaverse`  
   (or leave it under `bcProFoundation`)

## Option B — You push the bundle to `danaverse` (2 minutes)

On a machine logged in as a `danaverse` owner:

```bash
git clone https://github.com/bcProFoundation/wlotus.git
cd wlotus
git pull /path/to/wlotus-main.bundle main
git push -u origin main
```

Or from a fresh folder:

```bash
mkdir wlotus && cd wlotus
git init -b main
git pull /path/to/wlotus-main.bundle main
git remote add origin https://github.com/bcProFoundation/wlotus.git
git push -u origin main
```

## Option C — PAT from your laptop against local clone

If you copy `/home/ubuntu/wlotus` (or the zip) to your machine:

```bash
cd wlotus
git remote add origin https://github.com/bcProFoundation/wlotus.git
git push -u origin main
```

---

**Recommendation:** Option A if you want the agent to keep iterating on the covenant in this Environment; transfer to `danaverse` afterward if branding requires it.
