Requirements and Features
=========================

In this chapter, main features and the functional and nonfunctional requirements of the Sokoban project are presented. They are organized according to Annex A.5 **SRS Section 3 organized by feature** from SRS IEEE Std. 830.

Abbreviations
-------------

- **FR** - Functional requirement
- **NFR** - Non-Functional requirement
- **Client** - A software application connecting to the server
- **Server** - Central process that manages registration, sessions, game state, matchmaking, persistence, and communication.
- **AI** - Artificial Intelligence
- **UI** - User Interface
- **AC** - Architectural Constraints

Specific Requirements
---------------------

External Interfaces
^^^^^^^^^^^^^^^^^^^

.. rubric:: User Interfaces
  :heading-level: 4

Depending on the hardware the interface must support:

- Viewing the game
- Moving the Sokoban
- Hearing

.. rubric:: Hardware Interfaces
  :heading-level: 4

No specific hardware interfaces are required beyond standard desktop input/output devices (keyboard, display, audio).

.. rubric:: Software Interfaces
  :heading-level: 4

tbd

.. rubric:: Communications Interfaces
  :heading-level: 4

Client-Server Architecture

System Features
^^^^^^^^^^^^^^^

.. rubric:: User Login and Authentification
  :heading-level: 4

.. rubric:: Introduction / Purpose
  :heading-level: 5

This feature allows users to authenticate themselves and establish a secure session with the system before accessing any gameplay or account-related functionality.

.. rubric:: Stimulus / Response Sequence
  :heading-level: 5

For login and authentification, a user

- launches the client and provides the login credentials, i.e. their username and password, via a login form.
- After the user presses the OK button of the form, the server authenticates the user.
- If the authentication is successful, a user session is created and the user account context is loaded for the respective user.
- If the authentification fails, because the provided username or the password was not found in the user database, an error message is displayed.

.. rubric:: Associated Functional Requirements
  :heading-level: 5

- **FR1**: There is a login form on the client that allows users to enter and submit their username and password
- **FR2**: Upon a login request from a client, the server authenticates the user by validating username and password provided with the request
- **FR3**: For a successful authentification, the server creates a session token and loads the account context for the authenticated user
- **FR4**: For a failed authentification, the server responds with an error message to the client request

.. rubric:: User Account and Profile Management
  :heading-level: 4

.. rubric:: Introduction / Purpose
  :heading-level: 5

This feature manages user-specific data such as settings, controls, profiles, and preferences and ensures persistence across sessions and devices.

.. rubric:: Stimulus / Response Sequence
  :heading-level: 5

- On the client, the authenticated user opens the settings menu.
- The user then modifies settings regarding the account itself, game controls or the profile attributes visible for other authenticated users
- The user saves or discards made changes.
- The server validates and persists the changes in the settings by storing the update user information in a database.
- After a successful server-sided update of the user settings, the server sends a confirmatory response to the client, that will be displayed to the user.
- If the server-sided update of the user settings fails, the server sends a failure response to the client that will be displayed to the user.
- On the next user login, the saved user settings are applied automatically.

.. rubric:: Associated Functional Requirements
  :heading-level: 5

- **FR3**: The system shall allow an authenticated user to open the settings menu from the client application.
- **FR4**: The server shall provide the current persisted user settings to the client when the settings menu is opened.
- **FR5**: The system shall allow an authenticated user to modify settings related to: account preferences, game controls, profile attributes visible to other authenticated users, without immediately persisting those changes.
- **FR6**: The system shall allow the user to explicitly choose to save or discard modified settings.
- **FR7**: The system shall prompt the user for confirmation before persisting modified settings.
- **FR8**: If the user discards changes or cancels the confirmation, all unsaved modifications shall be discarded and no server-side update shall occur.
- **FR9**: Upon a confirmed save request, the server shall validate the modified user settings.
- **FR10**: If validation succeeds, the server shall persist the updated user settings in a server-side database.
- **FR11**: If validation or persistence fails, the server shall reject the update and return a failure response to the client.
- **FR12**: Upon successful persistence of user settings, the server shall return a success response that the client displays to the user.
- **FR13**: Upon failure to persist user settings, the server shall return a failure response that the client displays to the user.
- **FR14**: Persisted user settings shall be automatically loaded and applied by the system upon the user's next successful login.

.. rubric:: Networked Client-Server Game Infrastructure
  :heading-level: 4

.. rubric:: Introduction / Purpose
  :heading-level: 5

The Networked client-server game infrastructure deals with the communication between the server and different clients, dealing with topics such as game state synchronization, information exchange, player authentication and routing of messages from the clients to the server-sided game engine and vice versa.

.. rubric:: Stimulus / Response Sequence
  :heading-level: 5

- The authenticated user interacts with the client application during gameplay, e.g. by pressing keys.
- At fixed, predefined time intervals, the client transmits the user's input actions to the server together with the user's session token.
- Upon receiving the client input, the server validates the session token to ensure the action originates from an authenticated and active user.
- The server-sided game engine validates each received input action against the authoritative game state, which ensures that the user action is allowed in the current game state and does not violate any logical limits (like over-long powerup durations)
- At fixed, predefined time intervals, the server distributes the updated game state to all connected clients.
- Each client updates its local presentation of the game world based on the received game state.
- If the game engine detects invalid, unauthorized, or inconsistent input actions, the server rejects the action and may trigger corrective or security measures.

.. rubric:: Associated Functional Requirements
  :heading-level: 5

- **FR1**: The server shall require a valid session token for all gameplay requests comming from clients.
- **FR2**: The server shall validate the session token of each received gameplay input message to make sure that the user is authenticated and the session is active.
- **FR3**: The server shall reject gameplay input messages originating from invalid, expired, or unauthenticated sessions.
- **FR4**: The client shall transmit user input actions to the server at fixed, predefined time intervals during active gameplay.
- **FR5**: Each gameplay request comming from a client shall include the user's current input actions and the associated session token.
- **FR9**: The server shall maintain the authoritative game state for all connected clients.
- **FR10**: The game engine shall update the authoritative game state exclusively based on user input actions validated by the server in terms of authorization.
- **FR11**: The server shall ignore or reject any input actions that fail validation and shall not hand them to the server-sided game engine.
- **FR12**: The server shall distribute the updated authoritative game state to all connected clients at fixed, predefined time intervals.
- **FR13**: Each client shall update its local game representation exclusively based on the authoritative game state received from the server.
- **FR14**: If unauthorized actions are detected, the server shall respond according to predefined rules.

.. rubric:: Sokoban Game Engine (Core Logic)
  :heading-level: 4

.. rubric:: Introduction / Purpose
  :heading-level: 5

The Sokoban Game Engine implements the core Sokoban rules, board logic, movement validation, and victory detection.

.. rubric:: Stimulus / Response Sequence
  :heading-level: 5

- Upon the start of a new Sokoban match, the server-side Sokoban game engine initializes an authoritative game state by loading the configured game board.
- The initialized game board consists of a grid of tiles, including walls and traversable ground tiles, player-controlled pushers, movable boxes of which some contain a hidden powerup or -down, box storage locations ("plates")
- The server transmits the initial authoritative game state to all clients participating in the match.
- During gameplay, each client transmits the user's directional movement inputs to the server.
- For each received input, the server validates the input against the authoritative game state.
- If the input represents a valid movement action, the server updates the authoritative game state by: moving the pusher accordingly, pushing exactly one box if applicable and permitted, applying powerup or powerdown effects if a gift box is moved onto a plate.
- If the input represents an invalid movement action, the game engine rejects the input and leaves the authoritative game state unchanged.
- After processing validated inputs, the game engine updates the authoritative game state.
- The game engine evaluates the authoritative game state after each update to determine whether the match completion condition is met.
- The match is considered completed when all boxes are positioned on plates.
- Upon match completion, the game engine calculates the final score according to defined scoring rules.
- The server marks the match as completed and sends a match result notification, including the outcome and score, to all participating clients.
- Upon receiving the match result notification, each client displays the match outcome and achieved score to the user.

.. rubric:: Associated Functional Requirements
  :heading-level: 5

**FR24**: The game engine shall initialize an authoritative game state at the start of each Sokoban match.
**FR25**: The game engine shall load the game board, including tiles, pushers, boxes, plates, and configured powerups and powerdowns.
**FR26**: The server-sided game engine shall be the sole authority for modifying the game state.
**FR27**: Clients shall transmit movement inputs to the server during active gameplay.
**FR29**: The game engine shall validate each received movement input against the authoritative game state.
**FR30**: The game engine shall allow pusher movement only in the four cardinal directions: up, down, left, and right.
**FR31**: The game engine shall prevent pushers from moving into wall tiles.
**FR32**: The game engine shall prevent pushers from moving into occupied tiles unless pushing a box is permitted.
**FR33**: The game engine shall allow a pusher to push exactly one box at a time.
**FR34**: The game engine shall prevent pushers from pulling boxes.
**FR36**: When a box, that was moved to a plate contains a powerup or powerdown, the game engine shall apply the corresponding effect to the pusher.
**FR37**: The game engine shall apply powerup and powerdown effects only as a result of validated actions.
**FR38**: The game engine shall update the authoritative game state only after successful validation and application of movement inputs.
**FR39**: The server shall distribute the updated authoritative game state to all participating clients
**FR40**: The game engine shall evaluate the authoritative game state after each update to determine whether the match completion condition is met.
**FR41**: The game engine shall consider the match completed when all boxes are positioned on plates.
**FR42**: Upon match completion, the game engine shall prevent further movement updates.
**FR43**: Upon match completion, the game engine shall calculate a final score.
**FR44**: The server shall send a match result notification, including the outcome and final score, to all participating clients.
**FR45**: Upon receiving a match result notification, the client shall display the match outcome and achieved score to the user.

.. rubric:: Power-Ups and Power-Downs
  :heading-level: 4

.. rubric:: Introduction / Purpose
  :heading-level: 5

Powerups-and downs are contained in a random number of boxes and buff or nerf (i.e., apply positive of negative effects on) the player's pusher's abilities. As their distribution in boxes during matches is nondeterministic, they make the game more unpredictable and and allow further strategic variation.

.. rubric:: Stimulus / Response Sequence
  :heading-level: 5

- During initialization of a Sokoban match, the server-side game engine designates a subset of boxes on the game board as gift boxes.
- For each gift box, the game engine assigns either a powerup or a -down according to a predefined probability distribution.
- The content of a gift box remains hidden during gameplay until the gift box is pushed onto a plate.
- When a gift box is pushed onto a plate, the game engine reveals its content and determines the affected player.
- The game engine applies the effect of the revealed powerup or -down to the affected player's pusher.
- Applied powerup and -down effects modify the pusher's abilities for a predefined duration.
- After the expiration of the effect duration, the game engine removes the applied effect from the pusher.
- All powerup and powerdown assignment, activation, and expiration are enforced exclusively by the server-side game engine as part of the authoritative game state.

.. rubric:: Associated Functional Requirements
  :heading-level: 5

**FR33**: At match initialization, the game engine shall designate a subset of boxes as gift boxes.
**FR34**: For each gift box, the game engine shall assign exactly one effect, either a powerup or a powerdown.
**FR36**: The game engine shall enforce a probability of 2/3 for assigning a powerup and 1/3 for assigning a powerdown.
**FR37**: The content of a gift box shall remain hidden from players until the gift box is pushed onto a plate.
**FR38**: When a gift box is pushed onto a plate, the game engine shall outpu its assigned effect, such that the server can send an according message to the affected clients.
**FR39**: Upon revealing a gift box, the game engine shall apply the associated powerup or powerdown effect to the affected player’s pusher.
**FR40**: Powerup and powerdown effects shall be applied only as a result of validated game actions.
**FR41**: Each powerup and powerdown effect shall have a predefined duration.
**FR42**: The game engine shall automatically remove the applied effect from the pusher after the duration expires.
**FR43**: The expiration of powerup and powerdown effects shall be enforced exclusively by the game engine.
**FR44**: Players shall not be able to enable, disable, or alter powerup or powerdown effects through client-side actions.
**FR45**: The game engine shall ensure that powerup and powerdown effects are consistently reflected in the authoritative game state.

.. rubric::  Game Mode Selection (General)
  :heading-level: 4

.. rubric:: Single-Player Mode
  :heading-level: 4

.. rubric:: Cooperative-Multiplayer Mode
  :heading-level: 4

.. rubric:: Competitive-Multiplayer Mode
  :heading-level: 4

.. rubric:: Multiplayer Lobby Management (Shared Feature)
  :heading-level: 4

.. rubric:: AI Assistance
  :heading-level: 4

.. rubric:: Map Editor (Level Designer)
  :heading-level: 4

.. rubric:: Community & Social Interaction
  :heading-level: 4

Performance Requirements
^^^^^^^^^^^^^^^^^^^^^^^^^

Logical Database Requirements
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. rubric:: User Account Data
  :heading-level: 4

.. rubric:: Player Profile and Settings Data
  :heading-level: 4

.. rubric:: Gameplay Statistics Data
  :heading-level: 4

.. rubric:: Game Session Data
  :heading-level: 4

.. rubric:: Custom Map Data
  :heading-level: 4

.. rubric:: Moderation and Report Data
  :heading-level: 4

Design Constraints
^^^^^^^^^^^^^^^^^^

Software System Attributes
^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. rubric:: Usability
  :heading-level: 4

.. rubric:: Accessibility
  :heading-level: 4

.. rubric:: Portability and Compatibility
  :heading-level: 4

.. rubric:: User Interface and Interaction
  :heading-level: 4

.. rubric:: Accessibility and Internationalization
  :heading-level: 4

.. rubric:: Data Persistence and Cross-Device Support
  :heading-level: 4

.. rubric:: Security and Privacy
  :heading-level: 4

Other Requirements
^^^^^^^^^^^^^^^^^^^

tbd

.. rubric:: test
  :heading-level: 4
