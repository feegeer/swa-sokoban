Runtime View
============
+++
NOTES:
Sequence diagrams (primary): x, play turn, request hint, level designing
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
