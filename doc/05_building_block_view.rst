Building Block View
===================

In this chapter, the building block view of the Sokoban system is shown. First, the high-level architecture is shown. Then, the sub-components are covered with special focus on the `GameServer`.

High-level architecture
-----------------------

The high-level architecture of the Sokoban system contains 5 main components: the game server, an external AI hint service, the database, game clients, and level designer clients.

.. figure:: _static/component_diagram.svg
   :width: 100%
   :align: center

`GameServer`
^^^^^^^^^^^^

The `GameServer` component represents the backend. It handles the interaction with the player clients (`GameClient`) and level designer clients (`LevelDesigner`), as well as the communication with the AI hint service (`AIHintService`) and the `Database`. The architecture allows for multiple game servers to enable load balancing, downtime minimizing and quick response times in multiple regions.

.. figure:: _static/component_diagram_game_server.svg
   :width: 100%
   :align: center

`GameServer` contains multiple sub-components:

`ClientInteraction`
"""""""""""""""""""

This component handles client requests through the `IGameServerAPI`. These include authorization requests, which are handled by the `AuthService`, as well as login with authorization, lobby and mode selection and gameplay inputs, handled by the `GameSessionManager` through the `IGameSession`.
User settings are being stored by the `Persistence` module, which is accessed through the `IAccountSettingsStore` interface.
The `ClientInteraction` component can also request hints from the `HintOrchestrator` through the `IHintService` interface and pass those back to the player.


`GameSessionManager`
""""""""""""""""""""

The `GameSessionManager` hosts active game sessions and maps players to their active sessions. It passes the player inputs into the authoritative game engine (`SokobanGameEngine`) and stores game results and stats using the `Persistence` component, accessed through the `IGameSessionStore` interface. Through the same interface, it loads session and player settings when needed.

`HintOrchestrator`
""""""""""""""""""

The `HintOrchestrator` communicates with the external `AIHintService` through the `IHintServiceAPI`, as well as the `GameSessionmanager` through the `IPlayerStateQuery` interface to generate hints for the user when requested through the `IHintService` interface.
The hints are generated according to the hint policy, which is stored in the `Persistence` component and accessed through the `IHintPolicyStore` interface.

`AuthService`
"""""""""""""

This component authorizes users every time they try to interact with the game server. This ensures that only the players themselves can log in to their account and make moves on their behalf.

`SokobanGameEngine`
"""""""""""""""""""

This is the sole authority for state mutation. It applies moves, updates the game board, handles powerups and powerdowns, detects wins and computes stores. To validate the requested moves, it accesses the `MoveValidator` component through the `IMoveValidation` interface.

`MoveValidator`
"""""""""""""""

This component executes rule-level validation of requested moves against the authoritative game board state. Since it is purely rule-based, it does not depend on any other component. This ensures that every move is legal, and the game can't run into an impossible state.

`MapManagement`
"""""""""""""""

The `MapManagement` component is responsible for interacting with the level designer clients (`LevelDesigner`) through the `ILevelAPI`. This includes requests for uploading, downloading and listing maps. The component runs a server-side solvability validation with timeout and ensures that invalid maps are not saved. Valid maps get stored by the `Persistence` component, accessed through the `IMapStore` interface.

`Persistence`
"""""""""""""

This component is the only one accessing the external database through the `IDatabaseAPI`. It stores user data, game session data, hint policies, maps, and statistics. To provide access to the database for the other components, it exposes `IMapStore`, `IAccountSettingsStore`, `IGameSessionStore`, and `IHintPolicyStore` interfaces.

`AIHintService`
^^^^^^^^^^^^^^^

This is an external AI service that can produce AI hints based on the current state of the game, including the player's state (location, powerups, powerdowns, etc.). The service is external so it can be easily replaced when necessary.

Database
^^^^^^^^

The database is an external service so that multiple GameServers can access the same database to allow for global statistics and consistent data.

`GameClient`
^^^^^^^^^^^^

The `GameClient` represents the frontend for the players of the game. It communicates with the game server through the `IGameServerAPI` and is itself stateless to make for an authoritative game logic.

`LevelDesigner`
^^^^^^^^^^^^^^^

This component represents the frontend for the level designers to enable users to create and publish their own maps. It communicates to the game server through the `ILevelAPI`. The `LevelDesigner` could be implemented along with the `GameClient` as a single front-end application, although not necessarily, because a user can create, edit, and publish maps without playing the game himself. The same applies to the `GameClient`.
