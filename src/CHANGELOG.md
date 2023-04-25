# Changelog

All notable changes to this project will be documented in this file. This includes new features in-game, such as plugins or mechanics, changes to existing features, plugins, mechanics, or additions/removals to the backend or frontend of the website or databases involved in the project, as well as changes to this documentation. 

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [v2.1.3] - 2023-04-25 - 

## [v2.1.2] - 2023-03-03 - 2023-03-07
### Added
- Docs:
  - Added new [Economy](mech/economy.md) related pages:
    - [Banking](mech/economy/banking.md)
  - Fleshed out [combat](mech/combat.md) related pages:
    - [Artifacts](mech/combat/artifacts.md) and [Artifact List](mech/combat/artifact-list.md)
    - [Damage Types](mech/combat/damagetypes.md)

### Changed
- Docs:
  - Restructuring of certain sections:
    - NPC
    - Nodes
  - Renamed `PlayerStats` -> [Datum](core/datum.md)

### Fixed
- Docs:
  - Fixed incorrect page being displayed for [PlayerStats](core/playerstats.md)

## [v2.1.1] - 2023-1-24 - 2023-02-27

### Added
- Docs:
  - ~~Began writing of the project overview~~. (DONE!)
  - Added various economy related pages.
  - Begun implementation of page tags such as "Unimplemented", "Implemented" and "Currently being developed".
    - For now they are just text tags.
  - Created new Misc. section.
    - Currently includes "Lore" and "Quest" pages.
  - Added "philosophy" section to the introduction page.
  - Added page for [land claiming](mech/nation-system.md).
  - Added page explaining [nodes](mech/nodes.md).
    - Page to explain the [territories](mech/nodes/territories.md) better.
    - Explain using [handles over IDs](mech/nodes/handles-over-ids.md).
  - Economy related pages:
    - [Resources](mech/economy/resources.md).
    - [Market](mech/economy/market.md).
    - [Banking](mech/economy/banking.md).
    - Started explaining the [API](mech/economy/API.md)
      - Class [Hierarchy](mech/economy/api/hierarchy.md)
  - [Cults](mech/cults.md) page.
    - Explanation of [cult objects](mech/cults/objects.md).
    - Breakdown of [Altars](mech/cults/altars.md)
    - Explanation of [Transmutations](mech/cults/transmutations.md)
  - [Player Stats](misc/playerstats.md) page and diagram.
  - Added a [project timeline](administration/project-timeline.md) and associated pages:
    - [Phase 1](administration/timeline/phase-1.md).
    - [Housekeeping](administration/housekeeping.md) is meant to serve as a guideline to keep code uniform and encourage best practices to keep the project organized.
    - Also added [Coding Standard](administration/coding-standard.md), [Optimization](administration/optimization.md) and [Commits](administration/commits.md)
  - Added a "Core" section discussing the core components that make up the backbone of the project.
    - [Common Library](core/common-lib.md) page outlining the framework and its functionality.
### Changed
- Docs:
  - Changed structure of the table of contents.
    - Moved the Map section underneath the Mechanics section as it made more sense.
    - Moved "Lore" page to no longer be nested underneath the Map section and instead moved under Misc.
    - Moved "Claiming System" page to be nested under Nodes instead of its own section as it makes more sense.
    - Moved "PlayerStats" to the core section as it makes more sense.
  - Updated footer on first page.
  - Fixed various small formatting errors .
  - Updated [contributions](misc/contributions.md) page.
  - Changed `Administration` -> `General Development`
    - Better reflection of what the section is meant for.

### Removed
- Docs:
  - Removed Nation Objects page as it isn't necessary.
  
## [v2.1.0] - 2023-1-2 - 2023-1-23
### Added 
- Web:
  - Seperate repositories for better version control via git.
    - Created seperate repo to hold the documentation.
    - Created seperate repo for homepage.
  - Started work on ``/craftviewer`` and ``/market``.

- Discord:
  - In process of creating seperate Discord server exclusively for developmental purposes.
  - Arya
    - Creation of seperate repository for Arya's core module for better version controlling.

- Server:
  - Upgraded server hardware to 6 cores and 16 GB RAM.
  - Set up ``rdiff-backup`` in-order to periodically backup the map.
  - Pruned uneccesary/redundant testing environments.

- Map:
  - Began work on "Port Xile" as well as plotting out the starting point for "Bettesville".   
  
  - "Port Xile":
    - Market area completed, needs polishing/decorating.
    - Social area half finished, feels empty atm.
    - Main fortress near complete needs interior/rooms.
    - Residential area near complete, needs a few more blocks to fill out space, as well as creation of a "high end" district.

### Changed
- Misc:
  - Fixed project version to display ``v2`` instead of ``v0``.

- Docs:
  - Restructuring of the documentation.
  - Creation of user guides.
  - Began writing of the project overview.

### Removed
- Web:
  - Documentation and homepage from main web repository, moved to seperate repo.
  - Configured Dynmap.
    - To use external webserver instead of the default internal webserver.
    - Installed LiveAtlas UI.
    - New link: [https://airshipcraft.tk/map](https://airshipcraft.tk/map).

- Discord:
  - Moved ``core`` module of Arya to seperate repository.