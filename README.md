# MS Cookbook Vote

Mobile-friendly voting page for narrowing Marley Spoon picks each week. Brandon and his wife each tap recipes they're OK with, hit Send, and it shares a clean text summary via the Android/iOS share sheet.

**Live page:** https://nesint00-rgb.github.io/MS-cookbook-vote/

## How it works

- `index.html` — the voting page (single self-contained file)
- `current.json` — the shortlist data, refreshed weekly

The page reads `current.json` on load with a cache-bust, so updates show up the next time anyone opens it. Selections persist in localStorage per device, and stale selections (recipes no longer in the shortlist) are automatically dropped when the JSON changes.

## Weekly update workflow

Each week:

1. Run the slot-narrowing exercise with Claude (drop disqualifiers, trim per slot until ~6–10 per slot).
2. Edit `current.json`:
   - Update `weekOf` (ISO date) and `displayDate` (human-readable).
   - Replace each slot's `recipes` array with the trimmed shortlist.
   - Set `inBox: true` on whichever recipes are currently selected in the MS cart for that week.
3. Commit and push to `main`. GitHub Pages rebuilds in ~1 minute.

Wife refreshes the page on her phone, sees the new week, votes, hits Send.

## Data shape

```jsonc
{
  "weekOf": "2026-05-13",
  "displayDate": "Wednesday, May 13",
  "slots": [
    {
      "id": "beef",            // stable slug, used to track selections
      "name": "Beef",
      "emoji": "🥩",
      "subtitle": "optional clarifier shown after slot name",
      "recipes": [
        {
          "title": "Recipe Name",
          "subtitle": "with sides & sauce",
          "time": "30–40 min",
          "cuisine": "Latin",
          "badges": ["Premium +$4.99/serv", "Optional Spice"],
          "inBox": true        // shows ⭐ — currently in MS cart
        }
      ]
    }
  ]
}
```

## Disqualifiers (standing)

These recipes never make it into the shortlist:
- Vegan / vegetarian (when filling the meat slots)
- Prepared meals (heat-and-eat boxes)
- *(Add more as discovered)*

## Slots

- 🥩 **Beef** — one beef-based dish per week
- 🐔 **Chicken** — incl. turkey
- 🐟🐖 **Other** — shrimp, fish, pork, lamb

Cuisine across slots must NOT overlap (no week with two Latin dishes, etc.).
