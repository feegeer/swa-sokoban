Goals
=====

The main goal is to have a distributed Sokoban game. Furthermore, we have

.. rubric:: G1: Have a server
  :heading-level: 4

- **G1.1**: The server sorts out the client connection and communication

  - **G1.1.1**: The server takes care of the client pairing multiplayer games
  - **G1.1.2**: The server handles client registration. Desktop should be the first supported platform, others like Steam, mobile or web might follow later.

- **G1.2**: the server sorts out the session management

  - **G1.2.1**: the game board will be generated and initialized
  - **G1.2.2**: random gift boxes will be placed on the map
  - **G1.2.3**: during session management, the server will calculate the players' scores


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
