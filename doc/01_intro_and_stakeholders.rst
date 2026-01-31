Introduction and Stakeholders
=============================

Sokoban is a puzzle video game where a player controls a character called the Sokoban on a 2D map viewed from above. The map consists of wall and floor squares as well as boxes and marked storage locations. The goal in each level is to push all boxes on the map to marked storage locations. The Sokoban can move across floor tiles horizontally and vertically and push, but not pull boxes by "walking" against them. This only works, if the square behind the box is an empty floor tile, i.e. not a wall nor another box.

Our customer Hawki employed us to create a distributed Sokoban game, that can be played in both single and multiplayer modes.

.. rubric:: Main features of the distributed Sokoban game
  :heading-level: 3

- **Client-Server architecture**: clients can connect to the game server, that manages all connected clients and synchronizes game states across them. The server also validates the player actions.
- **Sokoban Game Engine**: is server-sided and implements the core game logic and thus features like player movements, game board related operations and updates of the authoritative game state.
- **Different game modes**: our Sokoban features a single player mode and two different multiplayer modes (cooperative and competitive)
- **Power-ups and -downs**: the Sokoban game boards feature different power-ups and downs like increased/decreased movement speed or the ability to pass through walls, to make the game entertaining and versatile
- **AI Assistance**: players can toggle AI assistance during game sessions, if they are stuck on the current puzzle
- **User and Account Management**: players can register and authenicate for the distributed Sokoban game. They are also able to manage their profiles and view player statistics and leaderboards for the ranked multiplayer mode
- **Map Editor**: players can use the map editor to create and share custom Sokoban maps
- **Community and Social Interaction**: players have the option to spectate ongoing multiplayer sessions and exchange in-game chat messages with each other.
- **Progress and Statistics**: players of our game can track their gameplay staticstics and share their scores and achievements

Stakeholders
------------

Overview
^^^^^^^^
+-----------------------------------+---------------------------------------------+-------------------------------+-------------------------------------+
| Stakeholder                       | Concerns / Expectations                     | Importance for System Success | Influence on Requirements / Project |
+===================================+=============================================+===============================+=====================================+
| Customer (HAWKI)                  | Defines vision and goals,                   | Very high                     | Very high                           |
|                                   | approves all decisions                      |                               |                                     |
+-----------------------------------+---------------------------------------------+-------------------------------+-------------------------------------+
| Players / End-Users               | Game must satisfy them                      | High                          | Medium                              |
|                                   | feedback affects features                   |                               |                                     |
+-----------------------------------+---------------------------------------------+-------------------------------+-------------------------------------+
| Developers                        | Implement the system                        | Low                           | Medium                              |
+-----------------------------------+---------------------------------------------+-------------------------------+-------------------------------------+
| Software Architects               | System design, distributed architecture     | Medium                        | High                                |
+-----------------------------------+---------------------------------------------+-------------------------------+-------------------------------------+
| System Admins / DevOps            | Uptime, hosting, scaling                    | Medium                        | Medium                              |
+-----------------------------------+---------------------------------------------+-------------------------------+-------------------------------------+
| Cloud / Server Provider           | Required for an always-online game          | High                          | Medium                              |
+-----------------------------------+---------------------------------------------+-------------------------------+-------------------------------------+
| Game Designers                    | Gameplay quality, level design              | Medium–High                   | Low–Medium                          |
+-----------------------------------+---------------------------------------------+-------------------------------+-------------------------------------+
| QA / Testers                      | Ensure quality, avoid regressions           | Medium                        | Medium                              |
+-----------------------------------+---------------------------------------------+-------------------------------+-------------------------------------+
| IT Security                       | Account protection, cheating prevention     | Medium                        | Medium                              |
+-----------------------------------+---------------------------------------------+-------------------------------+-------------------------------------+
| Support & Helpdesk                | User satisfaction & support                 | Medium                        | Low                                 |
+-----------------------------------+---------------------------------------------+-------------------------------+-------------------------------------+
| Community & Moderation Team       | Leaderboards, map moderation                | Medium                        | Low                                 |
+-----------------------------------+---------------------------------------------+-------------------------------+-------------------------------------+
| Project Managers                  | Planning & delivery                         | Medium                        | Medium                              |
+-----------------------------------+---------------------------------------------+-------------------------------+-------------------------------------+
| External Software / Asset         | Libraries, engines                          | Low–Medium                    | Low                                 |
| Suppliers                         |                                             |                               |                                     |
+-----------------------------------+---------------------------------------------+-------------------------------+-------------------------------------+
| Law & Compliance                  | Licensing, data protection                  | Low–Medium                    | Medium                              |
+-----------------------------------+---------------------------------------------+-------------------------------+-------------------------------------+


Importance vs. Influence diagram
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. figure:: images/impInfDiagram.svg
   :width: 100%
   :align: center


List of Concerns
^^^^^^^^^^^^^^^^

**Customer (HAWKI)**

- Distributed game with central client-server architecture
- Game should follow basic Sokoban rules
- Cross-platform client support
- AI only assisting, not competing
- Always online, stable system
- Good user experience and customization
- Project stays within a predefined budget
- agreed upon project timeline
- ROI expectations

**Players/End-Users**

- Playing a sokoban game
- Fun gameplay, intuitive controls
- Stable servers and low latency
- Fair matchmaking in ranked mode
- Possibility to create, save, and share maps
- Ability to spectate friends
- Smooth matchmaking and co-op experience
- Accessible UI/UX (key bindings, visual aids, color-blind mode)
- Community interaction

**Developers**

- Clear and feasable requirements
- Clear priorities
- Stable technologies (game engines/graphics libraries)
- Maintainable codebase
- Manageable workload

**Software Architects**

- Scalable client-server architecture
- Long term maintainability
- Clear session management
- Multiplayer synchronization
- Secure and fair gameplay

**Game Designers**

- Flexible map editor
- Validity checks for solvable maps
- Balanced power-ups and power-downs
- Clear rules for co-op and time-trial modes
- No cluttered game boards in multiplayer
- Power-ups and -downs working correctly
- Proper tooling support for map creation

**QA / Testers**

- Well-defined scenarios
- Testable system design
- Bug-free mechanics, especially multiplayer sync
- Thorough testing of gameplay features
- Monitoring of error rates
- Stable and reliable builds

**System Admins / DevOps**

- Server uptime and monitoring
- Deployments, updates, backups
- Cloud resource management
- Cost-efficient infrastructure usage
- Cost control mechanisms

**Cloud / Server Provider**

- Reliable hosting
- Scalable back-end capacity
- Defined service-level agreements

**Support & Helpdesk**

- Tools to assist players with login issues, bug reports
- Structured issue resolution workflows to solve most problems
- Clear error messages from the system

**Community & Moderation Team**

- Manage leaderboards
- Approve player-created maps
- Moderate chat/interaction channels
- Player forum, feedback and a report system
- Moderation tools

**IT Security**

- Prevent cheating/tampering
- Secure login/authentication
- Protect user data

**Law & Compliance**

- Licensing for assets
- IP rights handling
- Follow local law regarding a.g. censorship
- Data protection laws (GDPR, etc.)
- TOS and PP

**External Suppliers**

- Provide game assets
- Provide compatible libraries

**Project Managers**

- Scope clarity
- Timeline management
- Stakeholder alignment
- Risk management planning
