Introduction and Stakeholders
=============================

Sokoban is a puzzle video game where a player controls a character called the Sokoban on a 2D map viewed from above. The map consists of wall and floor squares as well as boxes and marked storage locations. The goal in each level is to push all boxes on the map to marked storage locations. The Sokoban can move across floor tiles horizontally and vertically and push, but not pull boxes by "walking" against them. This only works, if the square behind the box is an empty floor tile, i.e. not a wall nor another box.

Our customer Hawki employed us to create a distributed Sokoban game, that can be played in both single and multiplayer modes.

.. rubric:: Main features of the distributed Sokoban game
  :heading-level: 3

- **Client-Server architecture**: clients can connect to the game server, that manages all connected clients.
- **Sokoban Game Engine**: is server-side and implements the core game logic and thus features like player movements, game board related operations and updates of the authoritative game state.
- **Different game modes**: our Sokoban features a single player mode and two different multiplayer modes (cooperative and competitive)
- **Power-ups and -downs**: the Sokoban game boards feature different power-ups and downs like increased/decreased movement speed or the ability to pass through walls, to make the game entertaining and versatile
- **AI Assistance**: players can toggle AI assistance during game sessions, if they are stuck on the current puzzle
- **User and Account Management**: players can register and authenicate for the distributed Sokoban game. They are also able to manage their profiles and view player statistics and leaderboards for the ranked multiplayer mode
- **Map Editor**: players can use the map editor to create and share custom Sokoban maps
- **Community and Social Interaction**: players have the option to spectate ongoing multiplayer sessions and exchange in-game chat messages with each other.
- **Progress and Statistics**: players of our game can track their gameplay staticstics and share their scores and achievements

Stakeholders
------------

+-----------------------------+---------------------------------------------+-------------+--------------------------+
| Role / Name                 | Concerns / Expectations                     | Importance  | Influence on the project |
+=============================+=============================================+=============+==========================+
| Customer (HAWKI)            | compliance to Sokoban rules                 | high        | high                     |
|                             | (non-) functional requirements are met      |             |                          |
|                             | extensibility of the system                 |             |                          |
+-----------------------------+---------------------------------------------+-------------+--------------------------+
| Developer                   | knowing the features to develop             | rather high | rather low               |
|                             | project scope                               |             |                          |
|                             | feature priorities                          |             |                          |
|                             | tech stack                                  |             |                          |
+-----------------------------+---------------------------------------------+-------------+--------------------------+
| Sokoban Player              | play Sokoban online (also with others)      | mid         | low                      |
|                             | overall game experience                     |             |                          |
|                             | be challenged (AI or real opponent)         |             |                          |
|                             | create maps                                 |             |                          |
|                             | be entertained                              |             |                          |
+-----------------------------+---------------------------------------------+-------------+--------------------------+
| Project Manager             | timeline adherence                          | high        | high                     |
|                             | project budget is not exceeded              |             |                          |
|                             | resource allocation                         |             |                          |
|                             | risk management                             |             |                          |
+-----------------------------+---------------------------------------------+-------------+--------------------------+
| Software Architect          | system architecture                         | high        | high                     |
|                             | be involved in decisions                    |             |                          |
|                             | scalability                                 |             |                          |
|                             | maintainability                             |             |                          |
+-----------------------------+---------------------------------------------+-------------+--------------------------+
| Game Designers              | game mechanics                              | mid         | mid                      |
|                             | level design                                |             |                          |
|                             | player engagement                           |             |                          |
+-----------------------------+---------------------------------------------+-------------+--------------------------+
| Cloud / Server Provider     | hosting infrastructure                      | mid         | mid                      |
|                             | scalability                                 |             |                          |
|                             | overload and downtime prevention            |             |                          |
|                             | security                                    |             |                          |
+-----------------------------+---------------------------------------------+-------------+--------------------------+
| System Admin / DevOps       | deployment pipelines                        | high        | mid                      |
|                             | system uptime                               |             |                          |
|                             | performance efficiency                      |             |                          |
|                             | security                                    |             |                          |
+-----------------------------+---------------------------------------------+-------------+--------------------------+
| QA & Testers                | functional correctness                      | mid         | mid                      |
|                             | regression prevention                       |             |                          |
|                             | test coverage                               |             |                          |
+-----------------------------+---------------------------------------------+-------------+--------------------------+
| Community / Moderation Team | community guidelines enforcement            | low         | low                      |
|                             | user behavior moderation                    |             |                          |
+-----------------------------+---------------------------------------------+-------------+--------------------------+
| Law & Compliance            | legal rule compliance                       | mid         | mid                      |
|                             | licensing                                   |             |                          |
+-----------------------------+---------------------------------------------+-------------+--------------------------+
| IT Security                 | threat modeling                             | high        | mid                      |
|                             | vulnerability management                    |             |                          |
+-----------------------------+---------------------------------------------+-------------+--------------------------+
| Other externals             | supplying project components                | low         | low                      |
+-----------------------------+---------------------------------------------+-------------+--------------------------+
