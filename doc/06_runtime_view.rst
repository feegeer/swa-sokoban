Runtime View
============
+++
NOTES:
Activity diagrams (supporting): game session, level creation (if they add clarity beyond sequences)
+++

Sequence Diagrams
-----------------
Start Game Session
^^^^^^^^^^^^^^^^^^
The following diagram shows the sequence to start a game.

.. figure:: images/sequence_diagram_start_game_session.svg
   :width: 100%
   :align: center

.. rubric:: Goal
  :heading-level: 4

Create a new game session for the selected level and mode and deliver the initial game state to the client.

.. rubric:: Trigger
  :heading-level: 4

The Player selects levelId + gameMode and confirms “Start Game”.

.. rubric:: Preconditions
  :heading-level: 4

- The Player is connected to the server.
- The selected levelId is expected to exist on the server.

.. rubric:: Postconditions (Success)
  :heading-level: 4

- A sessionId exists on the server and the initial state is stored.
- The client has received {sessionId, initialState, levelData} and renders the level.

.. rubric:: Alternatives / Errors
  :heading-level: 4

- Invalid levelId: Server rejects request; client shows error.
- Level file missing/unavailable: Server rejects request; client shows error.

Play Turn
^^^^^^^^^
The following diagram shows the sequence to play a turn during a game.

.. figure:: images/sequence_diagram_play_turn.svg
   :width: 100%
   :align: center

.. rubric:: Goal
  :heading-level: 4

Apply a player's move in an existing session, enforce Sokoban rules on the server, update the authoritative state, and return the resulting state/version to the client.

.. rubric:: Trigger
  :heading-level: 4

Player presses a direction key (Up/Down/Left/Right) during an active session.

.. rubric:: Preconditions
  :heading-level: 4

- A valid sessionId exists, and the player is authorized to act in that session.
- Client has a current clientStateVersion for the session.

.. rubric:: Postconditions (Success)
  :heading-level: 4

- Server has persisted the updated session state.
- Client receives (newState, moveResult=OK, winStatus) and updates the UI (and shows completion if winStatus=true).
- (If multiplayer) state update is broadcast to other session participants.

.. rubric:: Alternatives / Errors
  :heading-level: 4

- Version conflict (client out of date): server returns Conflict with latest serverState, client refreshes and asks player to retry.
- Illegal move (blocked by rules): server returns moveResult=ILLEGAL with stateUnchanged=true, client shows blocked feedback and keeps the board unchanged.

Request Hint
^^^^^^^^^^^^
The following diagram shows the sequence to request a hint before a turn.

.. figure:: images/sequence_diagram_request_hint.svg
   :width: 100%
   :align: center

.. rubric:: Goal
  :heading-level: 4

Provide the player with a strategic hint for the current session state by querying the AI hint service and returning the recommendation to the client.

.. rubric:: Trigger
  :heading-level: 4

Player clicks the Hint button during an active session.

.. rubric:: Preconditions
  :heading-level: 4

- A valid sessionId exists.
- The server can load the current session state (and level information needed for solving).
- The client provides clientStateVersion for conflict detection.

.. rubric:: Postconditions (Success)
  :heading-level: 4

- The client receives a hint (single move) and displays it.

.. rubric:: Alternatives / Errors
  :heading-level: 4

- AI service unavailable / timeout: server returns Service Unavailable, client shows "try again later."
- No hint found: server returns "no hint available" for the given state, client displays message.
- State conflict: server returns Conflict with the latest server state, client refreshes and prompts the player to request the hint again.

Level Designing
^^^^^^^^^^^^^^^

The following diagram shows the sequence of designing a new level.

.. figure:: images/sequence_diagram_level_designing.svg
   :width: 100%
   :align: center

.. rubric:: Goal
  :heading-level: 4

Allow a player to create a custom level, validate that it is solvable under Sokoban rules, and persist the level, so it can be used in gameplay.

.. rubric:: Trigger
  :heading-level: 4

Player selects Design level and later clicks Save level in the editor.

.. rubric:: Preconditions
  :heading-level: 4

- The client can start a design-mode session and present the level editor UI.
- The server can validate levels (solvability check) and access persistence.

.. rubric:: Postconditions (Success)
  :heading-level: 4

- A design-mode sessionId exists.
- The new level is validated as solvable and stored with a new levelId.
- The client shows "level saved" feedback and can reference the saved levelId.

.. rubric:: Alternatives / Errors
  :heading-level: 4

- Invalid/unsolvable level: server returns levelValid=false with reasons. The client shows "level invalid" and displays the reasons, no level is stored.

Settings Management
^^^^^^^^^^^^^^^^^^^

The following diagram shows the sequence of updating game preferences. The player opens the settings menu, reviews their current configuration, modifies the desired preferences, and saves the changes. The GameClient communicates with the GameServer, which updates the player settings in the database.

.. figure:: images/sequence_diagram_change_settings.svg
   :width: 100%
   :align: center

.. rubric:: Goal
  :heading-level: 4

Allow a player to manage settings, controls, profiles, and preferences with persistence across sessions and devices.

.. rubric:: Trigger
  :heading-level: 4

The player opens the settings menu.

.. rubric:: Preconditions
  :heading-level: 4

- The player is authenticated in the game.
- The player has existing preferences stored.

.. rubric:: Postconditions (Success)
  :heading-level: 4

- The player’s new preferences are stored in the database.
- The client shows a “settings saved” message to the player.

.. rubric:: Alternatives / Errors
  :heading-level: 4

- Player cancels saving changes: The preferences are not stored and the settings remain unchanged.
- Error while storing settings in the database: The database returns an error, the server returns saved=false with an error reason. The client displays a “try again” message to the player and the settings remain unchanged.

Activity Diagrams
-----------------
Game Session
^^^^^^^^^^^^

The following diagram shows the sequence of designing a new level.

.. figure:: images/activity_diagram_game_session.svg
   :width: 100%
   :align: center

.. rubric:: Goal
  :heading-level: 4

Describe the end-to-end control flow of a typical game session from application start and login through level play until completion or leaving the session.

.. rubric:: Trigger
  :heading-level: 4

Player opens the client application and proceeds to login and start a game.

.. rubric:: Preconditions
  :heading-level: 4

- GameClient is installed and can connect to the GameServer.
- Player has valid credentials.
- At least one playable level is available on the server.

.. rubric:: Postconditions (Success)
  :heading-level: 4

- Level is completed, and the client shows the completion message (and the session can be finalized).

.. rubric:: Alternatives / Errors
  :heading-level: 4

- Application not loaded: user retries / client exits.
- Login failed: client shows login error and returns to login.
- Level not found: user returns to level selection.
- Invalid movement: client indicates invalid movement, gameplay continues.
- Player leaves session: client asks for confirmation. If confirmed, session ends without completion.

Level Designing
^^^^^^^^^^^^^^^

The following diagram shows the sequence of designing a new level.

.. figure:: images/activity_diagram_level_designing.svg
   :width: 100%
   :align: center

.. rubric:: Goal
  :heading-level: 4

Describe the control flow for creating/editing a custom level in the editor, validating solvability, and saving (or rejecting) the level.

.. rubric:: Trigger
  :heading-level: 4

Player selects Level designer mode and starts an editor session.

.. rubric:: Preconditions
  :heading-level: 4

- Player is authenticated/identified.
- GameClient can start the editor UI and communicate with the GameServer
- GameServer can run a solvability check and persist levels.

.. rubric:: Postconditions (Success)
  :heading-level: 4

- The edited map is verified as solvable and saved (server stores a levelId or updates an existing one).
- GameClient shows a success message and the editor state is consistent with the saved version.

.. rubric:: Alternatives / Errors
  :heading-level: 4

- Application not loaded: user retries / client exits.
- Login failed: client shows login error and returns to login.
- Map not selected / browse cancelled: user returns to selection.
- Invalid placement: client rejects placement and keeps editor state unchanged.
- Solvability check fails (unsolvable/timeout): client shows error, user continues editing without saving.
- Leave session with unsaved changes: client asks confirmation. If confirmed, changes are discarded and session ends. If not confirmed, return to editor.
