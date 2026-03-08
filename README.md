# Game Master

A digital Game Master for tabletop RPG adventurers. Play D&D the real way — pen, paper, and dice — while the website handles the DM role so every human at the table gets to be a player.

## What It Does

Game Master replaces the need for a human Dungeon Master. It narrates the story, describes scenes, runs encounters, and adjudicates dice rolls — all through the browser. Players keep everything else: their character sheets, physical dice, and the social experience of playing together around a table.

**The flow:**
1. Pick an Adventure or Campaign from the menu
2. The GM narrates the opening scene
3. Players discuss and choose an action
4. The GM tells you what to roll and which skill to use
5. Roll your physical dice, add your modifier, enter the total
6. The GM reveals the outcome and the story continues

## Adventures vs Campaigns

| | Adventures | Campaigns |
|---|---|---|
| Length | 1–3 hours | Multiple sessions |
| Saves progress | Yes | Yes, per session |
| Best for | Drop-in sessions, new players | Ongoing groups |

## Adding Your Own Content

Adventures and campaigns are plain JSON files — no coding required.

### Adding an Adventure

1. Create a file in `adventures/your-adventure-id.json`
2. Add the id to `adventures/manifest.json`

**Adventure structure:**
```json
{
  "id": "your-adventure-id",
  "title": "Your Adventure Title",
  "type": "adventure",
  "description": "A short description shown on the menu.",
  "recommended_players": "2-4",
  "estimated_time": "2-3 hours",
  "difficulty": "Beginner",
  "starting_scene": "scene_001",
  "scenes": {
    "scene_001": {
      "title": "Scene Title",
      "narrative": "What the GM reads aloud to the players.",
      "dm_note": "Private note for whoever is running the screen (not shown to players by default).",
      "suggested_actions": [
        {
          "label": "What the action button says",
          "requires_roll": false,
          "next_scene": "scene_002"
        },
        {
          "label": "Try to sneak past the guard",
          "check": "Stealth",
          "dc": 13,
          "success_scene": "scene_002_success",
          "failure_scene": "scene_002_fail",
          "success_bonus": "Extra flavour text shown on a success."
        }
      ]
    }
  }
}
```

**Action types:**

| Field | Description |
|---|---|
| `requires_roll: false` + `next_scene` | No roll needed, goes straight to next scene |
| `check` + `dc` + `success_scene` + `failure_scene` | Skill check — routes differently on success/failure |
| `success_bonus` | Optional extra text shown to players on a success |
| `is_end: true` | Marks the final scene — shows the end screen |

### Adding a Campaign

1. Create a folder `campaigns/your-campaign-id/`
2. Add `campaign.json` (metadata) and one JSON file per session
3. Add the id to `campaigns/manifest.json`

**campaign.json structure:**
```json
{
  "id": "your-campaign-id",
  "title": "Campaign Title",
  "type": "campaign",
  "description": "A short description.",
  "recommended_players": "3-5",
  "difficulty": "Intermediate",
  "sessions": [
    { "id": "session-1", "title": "Session One Title", "file": "session-1.json" },
    { "id": "session-2", "title": "Session Two Title", "file": "session-2.json" }
  ]
}
```

Each session file follows the same scene structure as an adventure.

## Difficulty Values

Use one of these in your JSON for correct badge styling:

- `"Beginner"` — green
- `"Intermediate"` — yellow
- `"Hard"` — red

## Hosting

This is a fully static site — no backend, no build step. It runs directly from GitHub Pages.

**To enable GitHub Pages:**
1. Go to your repo → Settings → Pages
2. Source: Deploy from a branch → `main` / `/ (root)`
3. Save — the site will be live at `https://jbaxmeyer-personal.github.io/Game-Master/`

## Project Structure

```
Game-Master/
├── index.html              # Home page — pick Adventure or Campaign
├── game.html               # The GM interface
├── style.css               # D&D parchment theme
├── js/
│   ├── main.js             # Menu logic (loads manifests, renders cards)
│   └── engine.js           # GM engine (scenes, rolls, saves, branching)
├── adventures/
│   ├── manifest.json       # List of adventure IDs to load
│   └── goblin-cave.json    # Sample adventure (22 branching scenes)
└── campaigns/
    ├── manifest.json       # List of campaign IDs to load
    └── lost-mine/
        ├── campaign.json   # Campaign metadata + session list
        ├── session-1.json  # Goblin Arrows
        └── session-2.json  # Phandalin
```
