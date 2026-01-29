Runtime View
============
+++
NOTES:
Activity diagrams (supporting): game session, level creation (if they add clarity beyond sequences)
+++

Sequence diagrams
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

.. rubric:: Postconditions (success)
  :heading-level: 4

- A sessionId exists on the server and the initial state is stored.
- The client has received {sessionId, initialState, levelData} and renders the level.

.. rubric:: Alternatives / errors
  :heading-level: 4

- Invalid levelId: Server rejects request; client shows error.
- Level file missing/unavailable: Server rejects request; client shows error.

Play turn
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

.. rubric:: Postconditions (success)
  :heading-level: 4

- Server has persisted the updated session state.
- Client receives (newState, moveResult=OK, winStatus) and updates the UI (and shows completion if winStatus=true).
- (If multiplayer) state update is broadcast to other session participants.

.. rubric:: Alternatives / errors
  :heading-level: 4

- Version conflict (client out of date): server returns Conflict with latest serverState, client refreshes and asks player to retry.
- Illegal move (blocked by rules): server returns moveResult=ILLEGAL with stateUnchanged=true, client shows blocked feedback and keeps the board unchanged.

Request hint
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

.. rubric:: Postconditions (success)
  :heading-level: 4

- The client receives a hint (single move) and displays it.

.. rubric:: Alternatives / errors
  :heading-level: 4

- AI service unavailable / timeout: server returns Service Unavailable, client shows "try again later."
- No hint found: server returns "no hint available" for the given state, client displays message.
- State conflict: server returns Conflict with the latest server state, client refreshes and prompts the player to request the hint again.

Level designing
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

.. rubric:: Postconditions (success)
  :heading-level: 4

- A design-mode sessionId exists.
- The new level is validated as solvable and stored with a new levelId.
- The client shows "level saved" feedback and can reference the saved levelId.

.. rubric:: Alternatives / errors
  :heading-level: 4

- Invalid/unsolvable level: server returns levelValid=false with reasons. The client shows "level invalid" and displays the reasons, no level is stored.
