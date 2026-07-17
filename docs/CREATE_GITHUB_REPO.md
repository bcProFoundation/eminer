# Publish status

## Blocker on `bcProFoundation/wlotus`

The Cursor GitHub App on `bcProFoundation` is set to **Selected repositories** and currently includes only:

- `eminer`, `lixi`, `local-ecash`, `lotusd`

**`wlotus` is not selected**, so this agent gets HTTP 403 on push even though the empty repo exists.

### Fix (org admin, ~30 seconds)

1. Open https://github.com/organizations/bcProFoundation/settings/installations  
2. Click **Cursor** (or the Cursor Cloud GitHub App) → **Configure**  
3. Under **Repository access** → **Only select repositories** → **Add** → **wlotus**  
   (or switch to **All repositories**)  
4. Save  
5. Reply: **wlotus added to Cursor app, push again**

---

## Temporary copy (available now)

Full scaffold pushed as an orphan branch on eminer (does not mix with eminer history):

https://github.com/bcProFoundation/eminer/tree/cursor/wlotus-scaffold-d6bb

Mirror into `bcProFoundation/wlotus` yourself:

```bash
git clone --branch cursor/wlotus-scaffold-d6bb https://github.com/bcProFoundation/eminer.git wlotus-tmp
cd wlotus-tmp
git remote set-url origin https://github.com/bcProFoundation/wlotus.git
git push -u origin cursor/wlotus-scaffold-d6bb:main
```
