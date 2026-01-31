Domain Model and Use Cases
==========================

THe following image shows the domain model for the Sokoban project. By clicking on it, you can view it in detail.

.. figure:: _static/domain_model.svg
   :width: 100%
   :align: center

The domain model captures the following core ideas:

- **session-based gameplay architecture**: A ``GameServerBackend`` manages multiple ``UserSessions``, which create or join ``GameSessions`` controlled and validated by a central ``GameEngine`` and configured via ``GameModeType``.
- **the system centers around the player**: Each ``Player`` has their own ``PlayerPreferences``, that consist of ``ControlSettings``, ``AudioVisualSettings`` and ``AccessibilitySettings``. Additionaly, a ``Player`` gets their own ``PlayerStatistics``, and participates in one or more ``GameSessions``, in which they control a ``SokobanCharacter``. If the user plays a ranked match, they will also be part of a ``Leaderboard``.
- **the Sokoverse game world**: Every ``GameSession`` contains a ``GameBoard`` composed of multiple ``Tiles`` and ``GameObjects`` (``Wall``, ``SokobanCharacter``, ``Box``), that each have a ``Position`` on the gameboard.
- **effects**: The ``Effects``, that are contained in some of the boxes, are either of type ``PowerUp`` or ``PowerDown``.
- **progress and match outcomes**: ``GameSessions`` produce a ``Score`` for each participating ``Player`` and a ``GameResult``, which in case of ranked mode feed into a ``Leaderboard`` with individual ``LeaderboardEntries``.
- **AI assistance**: An ``AiAssistant`` can support players in non-competitive matches by generating ``Hint``s within a ``GameSession``.
- **level creation**: A ``LevelDesigner`` builds and structures ``GameBoards`` and all the objects contained on the game boards ``Tiles``.
