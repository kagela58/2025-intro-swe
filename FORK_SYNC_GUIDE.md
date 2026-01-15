# 🔄 Guide for Syncing Your Fork Safely / Vodič za sigurno sinkroniziranje forka

[English](#english) | [Hrvatski](#hrvatski)

---

## English

### Understanding the Problem

When you fork a repository and make changes, your fork can become "out of sync" with the original (upstream) repository. When you try to click the **"Sync fork"** button on GitHub, you might see:

- ❌ "Can't automatically sync" error
- ⚠️ Warning about discarding commits or losing data
- 🔀 Message about conflicts

**Don't panic!** This guide will help you sync your fork safely without losing any of your work.

---

### Quick Reference: When to Use Each Method

| Scenario | Recommended Method | Safety Level |
|----------|-------------------|--------------|
| No local changes, just want updates | GitHub "Sync fork" button | ✅ Safe |
| Have uncommitted changes in Codespace | Method 1: Commit first, then sync | ✅ Safe |
| Fork is behind but no conflicts | Method 2: Git fetch and merge | ✅ Safe |
| Fork has conflicts with upstream | Method 3: Manual conflict resolution | ⚠️ Requires care |
| Complete mess, want fresh start | Method 4: Backup and recreate | ✅ Safe (if backed up) |

---

### Method 1: Safe Sync with Uncommitted Changes (RECOMMENDED)

**Use when:** You have changes in your fork that you want to keep.

#### Step 1: Save your work first
```bash
# Check what files you've changed
git status

# Stage all your changes
git add .

# Commit your changes
git commit -m "My work in progress - saving before sync"

# Push to YOUR fork
git push origin main
```

#### Step 2: Add upstream remote (one-time setup)
```bash
# Check if upstream is already configured
git remote -v

# If you don't see "upstream", add it:
git remote add upstream https://github.com/nibzard/2025-intro-swe.git

# Verify it was added
git remote -v
```

You should now see:
```
origin    https://github.com/YOUR_USERNAME/2025-intro-swe.git (fetch)
origin    https://github.com/YOUR_USERNAME/2025-intro-swe.git (push)
upstream  https://github.com/nibzard/2025-intro-swe.git (fetch)
upstream  https://github.com/nibzard/2025-intro-swe.git (push)
```

#### Step 3: Fetch and merge updates
```bash
# Get the latest changes from upstream
git fetch upstream

# Merge them into your main branch
git merge upstream/main

# If no conflicts, push to your fork
git push origin main
```

✅ **Success!** Your fork is now synced and your changes are preserved.

---

### Method 2: Using GitHub's Sync Fork Button (When Safe)

**Use when:** You have no uncommitted changes and no conflicts.

#### Steps:
1. Go to your fork on GitHub: `https://github.com/YOUR_USERNAME/2025-intro-swe`
2. Look for the message: "This branch is X commits behind nibzard:main"
3. Click **"Sync fork"** button
4. Click **"Update branch"**

#### Then update your Codespace:
```bash
# In your Codespace terminal:
git pull origin main
```

✅ This is the easiest method when it works!

---

### Method 3: Handling Conflicts

**Use when:** Git says you have conflicts during merge.

#### Step 1: Start the sync
```bash
git fetch upstream
git merge upstream/main
```

#### Step 2: If you see conflicts
Git will tell you which files have conflicts:
```
CONFLICT (content): Merge conflict in students/YOUR_USERNAME/lab1/intro.py
```

#### Step 3: Open the conflicted file
Look for markers like this:
```python
<<<<<<< HEAD
# Your version of the code
student = Student("Your Name", "sophomore")
=======
# Upstream version of the code  
student = Student("Example Name", "freshman")
>>>>>>> upstream/main
```

#### Step 4: Fix the conflict
- **Keep your version:** Delete the upstream section and markers
- **Keep upstream version:** Delete your section and markers
- **Keep both:** Combine them thoughtfully

After fixing:
```python
# Final version (example - keeping your version)
student = Student("Your Name", "sophomore")
```

#### Step 5: Complete the merge
```bash
# Stage the resolved files
git add students/YOUR_USERNAME/lab1/intro.py

# Complete the merge
git commit -m "Merge upstream changes, resolved conflicts"

# Push to your fork
git push origin main
```

✅ Conflicts resolved!

---

### Method 4: Nuclear Option - Start Fresh (Last Resort)

**Use when:** Everything is too complicated and you want a clean start.

⚠️ **WARNING:** Only use this if you've backed up your work!

#### Step 1: Backup your work
```bash
# Create a backup branch with ALL your work
git add .
git commit -m "Backup before reset"
git branch backup-$(date +%Y%m%d)
git push origin backup-$(date +%Y%m%d)
```

#### Step 2: Reset to upstream
```bash
# Fetch upstream
git fetch upstream

# HARD reset to match upstream (DESTRUCTIVE!)
git reset --hard upstream/main

# Force push to your fork
git push --force origin main
```

#### Step 3: Re-apply your changes
```bash
# Switch to your backup
git checkout backup-YYYYMMDD

# Copy your files to a safe location outside the repo
# Then switch back and manually re-add them
git checkout main
# Copy your files back and commit
```

---

### Common Issues and Solutions

#### Issue 1: "Permission denied" when pushing
**Solution:** Check your authentication
```bash
# If using HTTPS, you may need to authenticate
gh auth login

# Or check your remote URL
git remote -v
```

#### Issue 2: "Your branch is ahead of origin/main"
**Solution:** This is normal! Just push:
```bash
git push origin main
```

#### Issue 3: "Divergent branches"
**Solution:** Choose your strategy:
```bash
# Option A: Rebase (cleaner history)
git pull --rebase upstream main

# Option B: Merge (preserves all history)
git pull upstream main
```

#### Issue 4: Codespace is stuck/broken
**Solution:**
1. Save your work: copy files to your local computer or another location
2. Delete the Codespace on GitHub
3. Create a new Codespace
4. Restore your files

---

### Prevention Tips

✅ **Best Practices:**
1. **Sync regularly** - Don't wait weeks between syncs
2. **Commit often** - Small, frequent commits are easier to manage
3. **Work in your folder only** - Stay in `students/YOUR_USERNAME/`
4. **Pull before you push** - Always `git pull` before starting work
5. **Use branches** - For experimental work, create a branch:
   ```bash
   git checkout -b my-experiment
   ```

---

### Visual Workflow

```
┌─────────────────────────────────────────────────────────┐
│  UPSTREAM (nibzard/2025-intro-swe)                      │
│  Main course repository                                  │
└──────────────────┬──────────────────────────────────────┘
                   │
                   │ Fork (one time)
                   │
                   ↓
┌─────────────────────────────────────────────────────────┐
│  YOUR FORK (yourname/2025-intro-swe)                    │
│  Your copy on GitHub                                     │
└──────────────────┬──────────────────────────────────────┘
                   │
                   │ Clone / Codespace
                   │
                   ↓
┌─────────────────────────────────────────────────────────┐
│  LOCAL/CODESPACE                                         │
│  Where you make changes                                  │
└─────────────────────────────────────────────────────────┘

Sync process:
1. Fetch from UPSTREAM → LOCAL  (git fetch upstream)
2. Merge in LOCAL               (git merge upstream/main)
3. Push LOCAL → YOUR FORK       (git push origin main)
```

---

### Getting Help

If you're still stuck:
1. Read the exact error message carefully
2. Copy the error and paste it into your AI assistant (Claude, ChatGPT, etc.)
3. Ask in the course forum with:
   - The exact error message
   - What you tried
   - Screenshots if helpful
4. Attend office hours

---

## Hrvatski

### Razumijevanje problema

Kada forkate repozitorij i napravite izmjene, vaš fork može postati "neusklađen" s originalnim (upstream) repozitorijem. Kada pokušate kliknuti gumb **"Sync fork"** na GitHubu, možete vidjeti:

- ❌ Grešku "Can't automatically sync"
- ⚠️ Upozorenje o odbacivanju commit-a ili gubitku podataka
- 🔀 Poruku o konfliktima

**Ne paničarite!** Ovaj vodič će vam pomoći da sigurno sinkronizirate fork bez gubitka vašeg rada.

---

### Brzi pregled: Kada koristiti koju metodu

| Scenarij | Preporučena metoda | Razina sigurnosti |
|----------|-------------------|------------------|
| Nema lokalnih promjena, samo želim ažuriranja | GitHub "Sync fork" gumb | ✅ Sigurno |
| Imam necommitane promjene u Codespace-u | Metoda 1: Prvo commit, pa sync | ✅ Sigurno |
| Fork zaostaje ali nema konflikata | Metoda 2: Git fetch i merge | ✅ Sigurno |
| Fork ima konflikte s upstreamom | Metoda 3: Ručno rješavanje konflikata | ⚠️ Zahtijeva pažnju |
| Potpuni kaos, želim početi iznova | Metoda 4: Backup i ponovno kreiranje | ✅ Sigurno (ako imam backup) |

---

### Metoda 1: Sigurna sinkronizacija s necommitanim promjenama (PREPORUČENO)

**Koristite kada:** Imate promjene u forku koje želite zadržati.

#### Korak 1: Prvo spremite svoj rad
```bash
# Provjerite koje ste datoteke promijenili
git status

# Stage sve svoje promjene
git add .

# Commitajte svoje promjene
git commit -m "Moj rad u tijeku - spremam prije sync-a"

# Pushajte na VAŠ fork
git push origin main
```

#### Korak 2: Dodajte upstream remote (jednokratno postavljanje)
```bash
# Provjerite je li upstream već konfiguriran
git remote -v

# Ako ne vidite "upstream", dodajte ga:
git remote add upstream https://github.com/nibzard/2025-intro-swe.git

# Provjerite je li dodan
git remote -v
```

Sada biste trebali vidjeti:
```
origin    https://github.com/VAŠE_KORISNIČKO_IME/2025-intro-swe.git (fetch)
origin    https://github.com/VAŠE_KORISNIČKO_IME/2025-intro-swe.git (push)
upstream  https://github.com/nibzard/2025-intro-swe.git (fetch)
upstream  https://github.com/nibzard/2025-intro-swe.git (push)
```

#### Korak 3: Fetch i merge ažuriranja
```bash
# Dohvatite najnovije promjene s upstreama
git fetch upstream

# Merge-ajte ih u svoj main branch
git merge upstream/main

# Ako nema konflikata, pushajte na svoj fork
git push origin main
```

✅ **Uspjeh!** Vaš fork je sada sinkroniziran i vaše promjene su sačuvane.

---

### Metoda 2: Korištenje GitHub "Sync Fork" gumba (Kada je sigurno)

**Koristite kada:** Nemate necommitane promjene i nema konflikata.

#### Koraci:
1. Idite na svoj fork na GitHubu: `https://github.com/VAŠE_KORISNIČKO_IME/2025-intro-swe`
2. Potražite poruku: "This branch is X commits behind nibzard:main"
3. Kliknite gumb **"Sync fork"**
4. Kliknite **"Update branch"**

#### Zatim ažurirajte svoj Codespace:
```bash
# U terminalu vašeg Codespace-a:
git pull origin main
```

✅ Ovo je najjednostavnija metoda kada radi!

---

### Metoda 3: Rješavanje konflikata

**Koristite kada:** Git kaže da imate konflikte tijekom merge-a.

#### Korak 1: Započnite sinkronizaciju
```bash
git fetch upstream
git merge upstream/main
```

#### Korak 2: Ako vidite konflikte
Git će vam reći koje datoteke imaju konflikte:
```
CONFLICT (content): Merge conflict in students/VAŠE_KORISNIČKO_IME/lab1/intro.py
```

#### Korak 3: Otvorite datoteku s konfliktom
Potražite oznake poput ovih:
```python
<<<<<<< HEAD
# Vaša verzija koda
student = Student("Vaše Ime", "sophomore")
=======
# Upstream verzija koda
student = Student("Primjer Ime", "freshman")
>>>>>>> upstream/main
```

#### Korak 4: Popravite konflikt
- **Zadržite svoju verziju:** Obrišite upstream sekciju i oznake
- **Zadržite upstream verziju:** Obrišite svoju sekciju i oznake
- **Zadržite oboje:** Promišljeno ih kombinirajte

Nakon popravka:
```python
# Finalna verzija (primjer - zadržavanje vaše verzije)
student = Student("Vaše Ime", "sophomore")
```

#### Korak 5: Završite merge
```bash
# Stage riješene datoteke
git add students/VAŠE_KORISNIČKO_IME/lab1/intro.py

# Završite merge
git commit -m "Merge upstream promjena, riješeni konflikti"

# Pushajte na svoj fork
git push origin main
```

✅ Konflikti riješeni!

---

### Metoda 4: Nuklearna opcija - Počnite iznova (Posljednje utočište)

**Koristite kada:** Sve je previše komplicirano i želite čist početak.

⚠️ **UPOZORENJE:** Koristite samo ako ste napravili backup svog rada!

#### Korak 1: Backup vašeg rada
```bash
# Kreirajte backup branch sa SVIM vašim radom
git add .
git commit -m "Backup prije reseta"
git branch backup-$(date +%Y%m%d)
git push origin backup-$(date +%Y%m%d)
```

#### Korak 2: Reset na upstream
```bash
# Fetch upstream
git fetch upstream

# HARD reset da se podudara s upstreamom (DESTRUKTIVNO!)
git reset --hard upstream/main

# Force push na vaš fork
git push --force origin main
```

#### Korak 3: Ponovno primijenite svoje promjene
```bash
# Prebacite se na svoj backup
git checkout backup-YYYYMMDD

# Kopirajte svoje datoteke na sigurno mjesto izvan repoa
# Zatim se vratite i ručno ih ponovno dodajte
git checkout main
# Kopirajte svoje datoteke natrag i commitajte
```

---

### Česti problemi i rješenja

#### Problem 1: "Permission denied" pri pushanju
**Rješenje:** Provjerite svoju autentifikaciju
```bash
# Ako koristite HTTPS, možda se trebate autentificirati
gh auth login

# Ili provjerite svoj remote URL
git remote -v
```

#### Problem 2: "Your branch is ahead of origin/main"
**Rješenje:** Ovo je normalno! Samo pushajte:
```bash
git push origin main
```

#### Problem 3: "Divergent branches"
**Rješenje:** Odaberite svoju strategiju:
```bash
# Opcija A: Rebase (čišća povijest)
git pull --rebase upstream main

# Opcija B: Merge (čuva svu povijest)
git pull upstream main
```

#### Problem 4: Codespace je zaglavljen/pokvaren
**Rješenje:**
1. Spremite svoj rad: kopirajte datoteke na lokalno računalo ili drugo mjesto
2. Obrišite Codespace na GitHubu
3. Kreirajte novi Codespace
4. Vratite svoje datoteke

---

### Savjeti za prevenciju

✅ **Najbolje prakse:**
1. **Sinkronizirajte redovno** - Ne čekajte tjednima između sinkronizacija
2. **Commitajte često** - Mali, česti commit-ovi lakši su za upravljanje
3. **Radite samo u svojoj mapi** - Ostanite u `students/VAŠE_KORISNIČKO_IME/`
4. **Pull prije push-a** - Uvijek `git pull` prije početka rada
5. **Koristite branch-eve** - Za eksperimentalni rad, kreirajte branch:
   ```bash
   git checkout -b moj-eksperiment
   ```

---

### Vizualni prikaz rada

```
┌─────────────────────────────────────────────────────────┐
│  UPSTREAM (nibzard/2025-intro-swe)                      │
│  Glavni repozitorij kursa                               │
└──────────────────┬──────────────────────────────────────┘
                   │
                   │ Fork (jednom)
                   │
                   ↓
┌─────────────────────────────────────────────────────────┐
│  VAŠ FORK (vaseime/2025-intro-swe)                      │
│  Vaša kopija na GitHubu                                  │
└──────────────────┬──────────────────────────────────────┘
                   │
                   │ Clone / Codespace
                   │
                   ↓
┌─────────────────────────────────────────────────────────┐
│  LOKALNO/CODESPACE                                       │
│  Gdje pravite promjene                                   │
└─────────────────────────────────────────────────────────┘

Proces sinkronizacije:
1. Fetch s UPSTREAM → LOKALNO  (git fetch upstream)
2. Merge u LOKALNOM             (git merge upstream/main)
3. Push LOKALNO → VAŠ FORK     (git push origin main)
```

---

### Traženje pomoći

Ako ste još uvijek zaglavili:
1. Pažljivo pročitajte točnu poruku o grešci
2. Kopirajte grešku i zalijepite je u svoj AI asistent (Claude, ChatGPT, itd.)
3. Pitajte na forumu kursa sa:
   - Točnom porukom o grešci
   - Što ste pokušali
   - Screenshots ako je korisno
4. Dođite na konzultacije

---

### Dodatne napomene

**Za instruktore:**
- Razmislite o zaštiti `main` brancha da spriječite force push
- Razmotrite korištenje branch protection rules
- Educirajte studente o važnosti redovite sinkronizacije

**Za studente:**
- Ne bojte se praviti greške - to je dio učenja
- Pitajte za pomoć rano, ne čekajte da se problem pogorša
- Vježbajte git naredbe u testnom repozitoriju

---

## Quick Command Reference / Brze naredbe

```bash
# Check status / Provjera stanja
git status
git remote -v

# Save your work / Spremanje rada
git add .
git commit -m "your message / vaša poruka"
git push origin main

# Setup upstream (once) / Postavi upstream (jednom)
git remote add upstream https://github.com/nibzard/2025-intro-swe.git

# Sync from upstream / Sinkronizacija s upstreama
git fetch upstream
git merge upstream/main
git push origin main

# Handle conflicts / Rješavanje konflikata
git status  # see conflicted files / vidi konfliktne datoteke
# (edit files / uredi datoteke)
git add .
git commit -m "resolved conflicts / riješeni konflikti"
git push origin main
```

---

**Zadnje ažuriranje / Last updated:** 2025-01-15
**Verzija / Version:** 1.0
