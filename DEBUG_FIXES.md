# Playlist Chaos - Debugging & Fixes Report

## Summary

Fixed 5 critical bugs in the AI-generated Playlist app that were causing incorrect song classification, unreliable search, broken stats, and buggy lucky pick functionality.

---

## Bugs Fixed

### 1. **Chill Keyword Detection Bug** (Line 74)
**Problem:** Chill keywords were being checked against song *title* instead of *genre*.

**Impact:** Songs with "ambient", "lofi", or "sleep" genres would never be correctly identified as Chill unless they also had very low energy, causing misclassification.

**Example:** A song titled "Heavy Metal" with genre "ambient" should be Chill → Now correctly classified.

---

### 2. **Inverted Search Logic** (Line 171)
**Problem:** Search condition was backwards - checking if song value is IN query instead of query IN value.

**Impact:** Case-insensitive searches were completely broken. Searching for "AC" wouldn't find "AC/DC".

**Example:** With 22 songs in the default list, searching "AC" returns 0 results (broken) → Now returns 1 result (AC/DC).

---

### 3. **Average Energy Calculation Bug** (Line 124)
**Problem:** Only summing Hype song energies while dividing by total songs across all playlists.

**Impact:** Average energy statistic is grossly skewed toward high-energy songs.

**Example:** If you have [Hype: energy 9], [Chill: energy 1], [Mixed: energy 5]:
- Wrong: (9 / 3) = 3.0
- Correct: (9 + 1 + 5 / 3) = 5.0

---

### 4. **Hype Ratio Calculation Bug** (Line 119)
**Problem:** Total was set to only Hype count instead of all songs, breaking the ratio.

**Impact:** Hype Ratio always showed 1.0 (100%) regardless of actual distribution.

**Example:** With 8 Hype and 14 other songs:
- Wrong: 8/8 = 1.00 (100%)
- Correct: 8/22 = 0.36 (36%)

---

### 5. **Lucky Pick Empty List Crash** (Line 196)
**Problem:** random.choice() crashes on empty lists instead of returning None.

**Impact:** Clicking "Feeling lucky" with an empty "Hype" or "Chill" playlist crashes the app.

**Example:** Starting a new profile with profile settings that exclude all Hype songs → App crashes on lucky pick.

---

## All Bugs Verified

All five bugs were manually tested and confirmed to be fixed:

- Chill keyword detection: PASS
- Search logic: PASS
- Average energy calculation: PASS
- Hype ratio calculation: PASS
- Lucky pick empty list handling: PASS

---

## How to Run the App

```bash
pip install -r requirements.txt
streamlit run app.py
```

The app will open in your browser at http://localhost:8501

---

## Files Modified

- playlist_logic.py - All 5 bug fixes applied
- .gitignore - Added to prevent temp files from being committed

---

## Behavior Verification Checklist

After these fixes, the app now correctly:

- Classifies ambient/lofi/sleep genres as Chill
- Searches case-insensitively with partial matching
- Calculates average energy across all songs
- Shows accurate hype ratio as a percentage
- Handles empty playlists without crashing
- Follows the intended behavior documented in README.md
