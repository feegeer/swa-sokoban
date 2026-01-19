Intro and Goals
===============

Sokoban is a puzzle video game where a player controls a character called the Sokoban on a 2D map viewed from above. The map consists of wall and floor squares as well as boxes and marked storage locations. The goal in each level is to push all boxes on the map to marked storage locations. The Sokoban can move across floor tiles horizontally and vertically and push, but not pull boxes by "walking" against them. This only works, if the square behind the box is an empty floor tile, i.e. not a wall nor another box.

Our customer Hawki employed us to create a distributed Sokoban game, that can be played in both single and multiplayer modes.

.. rubric:: Main features of the distributed Sokoban game
  :heading-level: 3

- **Client-Server architecture**: clients can connect to the game server, that manages all connected clients and synchronizes game states across them. The server also validates the player actions.
- **Sokoban Game Engine**: implements the core game logic and thus features like player movements, game board related operations and game state updates
- **Different game modes**: our Sokoban features a single player mode and two different multiplayer modes (cooperative and competitive)
- **Power-ups and -downs**: the Sokoban game boards feature different power-ups and downs like increased/decreased movement speed or the ability to pass through walls, to make the game entertaining and versatile
- **AI Assistance**: players can toggle AI assistance during game sessions, if they are stuck on the current puzzle
- **User and Account Management**: players can register and authenicate for the distributed Sokoban game. They are also able to manage their profiles and view player statistics and leaderboards for the ranked multiplayer mode
- **Map Editor**: players can use the map editor to create and share custom Sokoban maps
- **Community and Social Interaction**: players have the option to spectate ongoing multiplayer sessions and exchange in-game chat messages with each other.
- **Progress and Statistics**: players of our game can track their gameplay staticstics and share their scores and achievements

(Quality) Goals
---------------

The main goal is to have a distributed Sokoban game. Furthermore, we have

.. rubric:: G1: Have a game server
  :heading-level: 4

- **G1.1**: The game server sorts out the client connection and communication

  - **G1.1.1**: The game server takes care of the client pairing multiplayer games
  - **G1.1.2**: The game server handles client registration. Desktop should be the first supported platform, others like Steam, mobile or web might follow later

- **G1.2**: the server sorts out the session management

  - **G1.2.1**: the game board will be generated and initialized
  - **G1.2.2**: random gift boxes will be placed on the map
  - **G1.2.3**: during session management, the game server will calculate the players' scores


.. rubric:: G2: Gameplay
  :heading-level: 4

- **G2.1**: The Sokoban (the character on the field)

  - **G2.1.1**: The Sokoban can move up, down, left and right
  - **G2.1.2**: The Sokoban can push a box, if no other box or wall is on the tile behind the box
  - **G2.1.3**: The Sokoban can have a buff or nerf effect, if a Powerup/-down is active

- **G2.2**: There is a grid-like map
- **G2.3**: There are boxes, that can be moved to fields without a wall
- **G2.4**: There are plates to which the player can move the boxes
- **G2.5**: There are gift boxes containing powerups and -downs
- **G2.6**: There are walls enclosing the map and (potentially) on the map


.. rubric:: G3: Game Modes
  :heading-level: 4

- **G3.1**: There is a single player mode
  - **G3.1.2**: An AI, that can help the player, is integrated into the single player mode

- **G3.2**: There is a friends-only multiplayer mode and an open multiplayer mode (friends and random players can join)

  - **G3.2.1**: Players can start new sessions
  - **G3.2.2**: Players can join existing sessions
  - **G3.2.3**: the multiplayer mode is a time-trial game, that is either ranked or casual
  - **G3.2.4**: Two to six players can be part of a multiplayer game

- **G3.3**: There is a map editor mode

  - **G3.3.1**: Players can create their own maps
  - **G3.3.2**: The system automatically checks the validity of the player-made maps
  - **G3.3.3**: The player-made maps can be saved

- **G3.4**: There is a Spectator mode


.. rubric:: G4: User Experience
  :heading-level: 4

- **G4.1**: The system has a user-friendly interface that is easy to navigate
- **G4.2**: The system provides clear and concise feedback to the user about their progress and any errors that occur
- **G4.3**: The system allows users to customize their experience, such as choosing their own avatar or creating their own maps


.. rubric:: G5: AI Integration
  :heading-level: 4

- **G5.1**: AI assistance is triggered in Co-op and Singleplayer mode by hitting a help button.
- **G5.2**: When triggered, AI draws an arrow to indicate direction in which the next box should be moved. When box is moved in that direction the arrow disappears and AI assistance is turned off till the next help button hit comes.
- **G5.3**: In Co-op the host has the toggle AI assistance on/off button.
- **G5.4**: The AI is able to provide helpful hints and suggestions to the user


.. rubric:: G6: Power-ups and Power-downs
  :heading-level: 4

- **G6.1**: The system has a variety of power-ups and power-downs that can be used by the player
- **G6.2**: The system has functionality for distributing power-ups and power-downs randomly throughout the game
- **G6.3**: The system has functionality for tracking the player's use of power-ups and power-downs


.. rubric:: G7: Map Editor
  :heading-level: 4

- **G7.1**: The system has a user-friendly interface for creating and editing maps
- **G7.2**: The system has functionality for validating the player-made maps
- **G7.3**: The system has functionality for saving and loading player-made maps


.. rubric:: G8: Performance
  :heading-level: 4

- **G8.1**: The system is able to handle a large number of users simultaneously
- **G8.2**: The system is able to handle a large amount of data and game state
- **G8.3**: The system is able to respond quickly to user input


.. rubric:: G9: Security
  :heading-level: 4

- **G9.1**: The system has functionality for authenticating and authorizing users
- **G9.2**: The system has functionality for protecting user data and game state
- **G9.3**: The system has functionality for preventing cheating and hacking


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
| Sokoban Player              | play Sokoban online                         | mid         | low                      |
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
