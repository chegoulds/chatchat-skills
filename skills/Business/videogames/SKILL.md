---
id: videogames
name: Videogames
description: A skill to lookup video game information and compare prices across multiple stores.
category: Lifestyle
requires: []
examples:
  - "Help me with videogames."
  - "Use videogames for this task."
author: "ivanheral"
version: "1.0.1"
---

# Video Game Skill 🎮

This skill allows OpenClaw to search for games, view Steam details, check ProtonDB compatibility, estimate playtime with HowLongToBeat, and find the best prices using CheapShark.

## Tools

### `scripts/game_tool.py`

This Python script interacts with multiple game APIs (Steam, CheapShark, ProtonDB).

**Usage:**

1.  **Search for deals (CheapShark):**
    ```bash
    python3 scripts/game_tool.py deals "Game Name"
    ```

2.  **Check Compatibility (ProtonDB):**
    ```bash
    python3 scripts/game_tool.py compatibility <APPID>
    ```

3.  **Get Game Duration (HLTB):**
    ```bash
    python3 scripts/game_tool.py duration "Game Name"
    ```

4.  **View details & Specs (Steam):**
    ```bash
    python3 scripts/game_tool.py details <APPID>
    ```

## Notes
- The script requires Python 3.
- No external library installation required.
