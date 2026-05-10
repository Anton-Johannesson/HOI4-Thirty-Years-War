# Thirty Years' War HoI4 Mod — Directory Structure Guide

A reference document describing the purpose, contents, and conventions for every folder in the mod.

---

## `common/`

The `common/` directory is the heart of the mod's game logic. It defines the rules, entities, and systems that govern gameplay — countries, ideologies, technologies, and more. Each subfolder corresponds to a specific game system.

---

### `common/country_tags/`

**Purpose:** Registers every country tag that exists in the mod.

Each `.txt` file maps a 3-letter tag to its country definition file. All countries must be declared here before they can appear anywhere else in the mod.

**File:** `tyw_countries.txt`

```
BOH = "countries/Bohemia.txt"
PAL = "countries/Palatinate.txt"
BAV = "countries/Bavaria.txt"
HAB = "countries/Habsburg Austria.txt"
SWE = "countries/Sweden.txt"
DEN = "countries/Denmark.txt"
FRA = "countries/France.txt"
SPA = "countries/Spain.txt"
DUT = "countries/Dutch Republic.txt"
SAX = "countries/Saxony.txt"
BRA = "countries/Brandenburg.txt"
```

**Naming convention:** Use the vanilla `tyw_` prefix on the filename itself, but country tags are always exactly 3 uppercase letters.

---

### `common/countries/`

**Purpose:** Defines the visual identity of each country — color on the map, graphical culture (unit models), and flag file reference.

One `.txt` file per country, named to match what you declared in `country_tags/`.

**Example — `Bohemia.txt`:**

```
color = { 0.8 0.2 0.1 }
graphical_culture = western_european_gfx
graphical_culture_2d = western_european_2d
```

**Tips:**
- Colors use RGB values from 0.0 to 1.0
- `graphical_culture` controls which 3D unit models appear in the 3D view
- Available cultures: `western_european_gfx`, `eastern_european_gfx`, `middle_eastern_gfx`, etc.

---

### `common/ideologies/`

**Purpose:** Replaces the vanilla ideologies (democratic, fascist, communist, neutrality) with period-appropriate religious and political factions.

**File:** `tyw_ideologies.txt`

Planned ideologies for this mod:

| Tag | Name | Represents |
|-----|------|------------|
| `catholic` | Catholic | Habsburg loyalists, Catholic League |
| `lutheran` | Lutheran | Protestant Union, North German princes |
| `calvinist` | Calvinist | Dutch Republic, Huguenots, Palatinate |
| `reformed` | Reformed | Zwinglian states, Swiss cantons |

Each ideology block defines:
- `color` — map tint for countries of this ideology
- `war_impact_on_world_tension` — aggression modelling
- `faction_influence` — how aggressively AI pursues alliances

---

### `common/national_focus/`

**Purpose:** Contains focus trees for every major power. Focus trees drive a country's narrative, unlocking events, modifiers, war justifications, and decisions.

One `.txt` file per country (or grouped by alliance bloc).

**Planned focus trees:**

| File | Country | Key branches |
|------|---------|-------------|
| `tyw_focus_bohemia.txt` | Bohemia | Accept the Defenestration, Call on the Protestant Union, Seek English Aid |
| `tyw_focus_habsburg.txt` | Habsburg Austria | Crush the Rebellion, Restore Imperial Authority, Edict of Restitution |
| `tyw_focus_sweden.txt` | Sweden | Enter the War, Secure Pomerania, Death of Gustavus Adolphus |
| `tyw_focus_france.txt` | France | Cardinal Richelieu's Reforms, Subsidise Protestants, Open Intervention |
| `tyw_focus_spain.txt` | Spain | Maintain the Road of Flanders, Support the Emperor, Decline of Empire |
| `tyw_focus_dutch.txt` | Dutch Republic | Eighty Years' War, Sea Beggars, Commercial Dominance |

Each focus requires:
- `id`, `icon`, `x`/`y` position
- `cost` (in weeks, vanilla default is 70 days = 10 weeks)
- `prerequisite` chain
- `completion_reward` block (event, modifier, or manpower bonus)

---

### `common/technologies/`

**Purpose:** Defines the entire tech tree, replacing vanilla WW2-era technology with Pike & Shot era equivalents.

**Planned tech categories:**

| File | Category | Examples |
|------|----------|---------|
| `tyw_land_doctrine.txt` | Land doctrine | Tercio, Linear Tactics, Swedish Brigade |
| `tyw_infantry_tech.txt` | Infantry weapons | Arquebus, Musket, Pike formations |
| `tyw_artillery_tech.txt` | Artillery | Siege cannon, field artillery train |
| `tyw_cavalry_tech.txt` | Cavalry | Reiters, Cuirassiers, Dragoons |
| `tyw_navy_tech.txt` | Naval | Galleon, Fluyt, Galley (Mediterranean) |
| `tyw_engineering_tech.txt` | Engineering | Star forts, field entrenchments |

Each technology entry specifies:
- Prerequisites (prior tech required)
- Research cost (in days)
- Bonuses granted (`breakthrough`, `defense`, `attack` modifiers)
- Year unlock (prevents ahistorical early research)

---

### `common/ideas/`

**Purpose:** National spirits, advisors, and country-specific modifiers that persist throughout a playthrough.

**File:** `tyw_ideas.txt`

**Examples:**

- **Bohemia:** *Protestant Revolt* — stability penalty, mobilisation speed bonus
- **Habsburg Austria:** *Imperial Prestige* — diplomatic influence bonus
- **Sweden:** *Gustav Adolf's Military Reforms* — attack bonus for infantry
- **France:** *Richelieu's Centralization* — political power gain, reduced internal faction influence
- **Spain:** *Silver from the New World* — monthly income bonus, inflation risk

Advisor slots should be renamed to period roles: **General**, **Chancellor**, **Treasurer**, **Bishop** (replaces the vanilla political advisor types).

---

### `common/decisions/`

**Purpose:** Player-triggered actions that don't fit into focus trees — recurring choices, war escalation options, diplomatic manoeuvres.

**File:** `tyw_decisions.txt`

**Planned decisions:**

- *Call a General Council* — costs political power, temporarily stabilises the empire
- *Issue Letters of Marque* — unlocks privateering modifiers for naval powers
- *Demand War Contributions* — post-victory option to extract reparations
- *Invoke the Imperial Ban* — Habsburg-only; strips rebel princes of their titles

Each decision requires `visible`, `available`, `cost`, and `effect` blocks.

---

## `events/`

**Purpose:** Scripted narrative events that fire based on in-game triggers. Events drive the historical phases of the war.

Each `.txt` file groups related events by phase or theme.

| File | Phase | Key events |
|------|-------|-----------|
| `tyw_start.txt` | Opening | Defenestration of Prague (1618), Formation of the Protestant Union |
| `tyw_bohemian_phase.txt` | Bohemian Phase 1618–1625 | Battle of White Mountain, Flight of the Winter King |
| `tyw_danish_phase.txt` | Danish Phase 1625–1629 | Christian IV enters, Treaty of Lübeck, Edict of Restitution |
| `tyw_swedish_phase.txt` | Swedish Phase 1630–1635 | Gustavus Adolphus lands, Sack of Magdeburg, Battle of Breitenfeld, Death of Gustav Adolf |
| `tyw_french_phase.txt` | French Phase 1635–1648 | French open intervention, Treaty of Westphalia |
| `tyw_peace.txt` | Peace | Westphalian negotiations, sovereignty clauses, end conditions |

**Event structure example:**

```
country_event = {
    id = tyw_start.1
    title = tyw_start.1.t
    desc = tyw_start.1.d
    picture = GFX_defenestration_of_prague

    trigger = {
        tag = BOH
        date > 1618.5.23
    }

    option = {
        name = tyw_start.1.a
        add_stability = -0.10
        set_country_flag = defenestration_occurred
    }
}
```

All event title/desc strings reference keys defined in `localisation/`.

---

## `history/`

Defines the actual starting state of the world when the scenario loads (date: 1618.5.23 — the day of the Defenestration of Prague).

---

### `history/countries/`

**Purpose:** Sets up each country's starting conditions: ruling ideology, political power, national focuses unlocked, technologies researched, and starting leaders.

One `.txt` file per country, named `TAG - CountryName.txt`.

**Example — `BOH - Bohemia.txt`:**

```
capital = 75              # Province ID for Prague
oob = "BOH_1618"          # Order of battle file
set_research_slots = 3

set_politics = {
    ruling_party = calvinist
    last_election = "1617.1.1"
    election_frequency = 48
    elections_allowed = no
}

set_stability = 0.45
set_war_support = 0.30

create_country_leader = {
    name = "Frederick V"
    portrait = GFX_portrait_frederick_v
    expire = "1632.1.1"
    ideology = calvinist
}

set_technology = {
    tercio_formation = 1
    arquebus = 1
    siege_cannon = 1
}
```

---

### `history/states/`

**Purpose:** Defines every state on the map — which country owns it, which countries have core claims, victory point locations, and civilian/military factory counts.

One `.txt` file per state, named `{ID} - {StateName}.txt`.

**Example — `75 - Bohemia.txt`:**

```
state = {
    id = 75
    name = "STATE_BOHEMIA"
    manpower = 500000

    history = {
        owner = BOH
        controller = BOH
        add_core_of = BOH
        add_core_of = HAB
        victory_points = { 5765 5 }  # Prague
    }

    provinces = { 5765 9372 1234 ... }

    buildings = {
        infrastructure = 2
        industrial_complex = 1
    }
}
```

**Note:** Province IDs must match actual IDs in the base game map (or your custom map if you modify it).

---

## `map/`

**Purpose:** Modifies the game map — province ownership, terrain, supply nodes, strategic regions.

> ⚠️ **Leave this empty until everything else works.** Map modding is the most complex and error-prone part of HoI4 modding. Corrupted map files crash the game immediately with no useful error message. Only begin map work once countries, events, and focus trees are functional.

When you are ready, the key files are:

- `definition.csv` — Province ID → RGB colour mapping
- `provinces.bmp` — The actual province map image
- `terrain.txt` — Terrain type per province
- `strategicregions/` — Defines supply regions and air zones

---

## `localisation/`

**Purpose:** All text that appears in-game — country names, event titles, focus names, ideology labels, idea descriptions — is defined here as key-value pairs.

All files must be **UTF-8 with BOM** encoding. In VS Code: bottom-right corner → click encoding → *Save with Encoding* → `UTF-8 with BOM`.

### `localisation/english/`

One `.yml` file per system, prefixed `tyw_`.

**File naming:**

| File | Contains |
|------|---------|
| `tyw_countries_l_english.yml` | Country names, adjectives |
| `tyw_ideologies_l_english.yml` | Ideology names, subtypes |
| `tyw_events_l_english.yml` | Event titles and body text |
| `tyw_focus_l_english.yml` | Focus tree names and descriptions |
| `tyw_ideas_l_english.yml` | National spirit names and tooltips |
| `tyw_decisions_l_english.yml` | Decision names and tooltips |
| `tyw_states_l_english.yml` | State and province names |

**Example — `tyw_events_l_english.yml`:**

```yaml
l_english:
 tyw_start.1.t:0 "The Defenestration of Prague"
 tyw_start.1.d:0 "Protestant nobles have thrown the King's Catholic governors from the windows of Prague Castle. The act of rebellion is open — war seems inevitable."
 tyw_start.1.a:0 "Embrace the revolt. Bohemia will be free."
```

---

## `gfx/`

**Purpose:** All image assets — country flags, focus tree icons, event pictures, leader portraits.

---

### `gfx/flags/`

Country flags in `.tga` format.

| Size | Use | Dimensions |
|------|-----|-----------|
| Normal | Diplomacy screen, country select | 82 × 52 px |
| Small | Top bar, ledger | 41 × 26 px |
| Medium | War screen | 41 × 26 px |

File naming: `{TAG}.tga`, `{TAG}_small.tga`, `{TAG}_medium.tga`

Free `.tga` export is available in GIMP (File → Export As → `.tga`).

---

### `gfx/interface/`

Focus tree icons and technology icons in `.dds` format (DXT5 compression).

- Focus icons: 90 × 90 px, named `GFX_focus_{name}.dds`
- Technology icons: 100 × 100 px

Use **Paint.NET** with the DDS plugin or **GIMP** with the DDS plugin for `.dds` export.

---

## Naming Conventions (Summary)

| Rule | Example |
|------|---------|
| Prefix all mod files with `tyw_` | `tyw_countries.txt` |
| Country tags: 3 uppercase letters | `BOH`, `HAB`, `SWE` |
| Event IDs: `filename.number` | `tyw_start.1` |
| Localisation keys: match event/focus IDs | `tyw_start.1.t` |
| History files: `TAG - Name.txt` | `BOH - Bohemia.txt` |
| State files: `{ID} - {Name}.txt` | `75 - Bohemia.txt` |

---

## Recommended Build Order

1. `common/country_tags/` + `common/countries/` — make countries exist
2. `history/countries/` — give them a starting state
3. `history/states/` — assign ownership of provinces
4. `localisation/` — add names so nothing shows as `KEY_NOT_FOUND`
5. `gfx/flags/` — add flags so the UI doesn't show black squares
6. `common/ideologies/` — replace vanilla ideologies
7. `common/national_focus/` — add focus trees one country at a time
8. `events/` — chain historical events to focus completions
9. `common/technologies/` — rework the tech tree
10. `common/ideas/` + `common/decisions/` — add national spirits and decisions
11. `map/` — edit the map last, once everything else is stable
