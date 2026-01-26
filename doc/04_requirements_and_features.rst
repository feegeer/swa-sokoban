Requirements and Features
=========================

In this chapter, main features and the functional and nonfunctional requirements of the Sokoban project are presented. They are organized according to Annex A.5 **SRS Section 3 organized by feature** from SRS IEEE Std. 830.

Abbreviations
-------------

- **FR** - Functional Requirement
- **NFR** - Non-Functional Requirement
- **LDR** - Logical Database Requirement
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

**NFR-P1** – the system shall process player movement inputs and update the game state within **100 ms** in single-player mode.

**NFR-P2** – in multiplayer modes, authoritative server validation and game state synchronization shall complete within **200 ms** under normal network conditions.

**NFR-P3** – AI-based hint calculation shall return a result within **2 seconds** for standard levels.

**NFR-P4** – map solvability validation shall respect the configured timeout (default **10 seconds**) and terminate gracefully, returning an error if exceeded.

**NFR-P5** – the system shall support at least 5000 **concurrent active users** without degradation of core gameplay functionality.

**NFR-P6** – the system shall support at least 200 **simultaneous multiplayer sessions per server instance**.

**NFR-P7** – matchmaking and lobby creation shall complete within **2 seconds** under normal and peak load conditions.

**NFR-P8** – the system shall handle game state synchronization for multiplayer sessions with up to **4 players** without exceeding defined latency thresholds.

**NFR-P9** – user profile and settings data shall be loaded within **1 second** during login under normal load.

**NFR-P10** – the system shall store and retrieve custom maps of up to **512 KB** without impacting gameplay performance.

**NFR-P11** – the system shall process concurrent game state updates deterministically and without data corruption.

Logical Database Requirements
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

User Account Data
""""""""""""""""""

**LDR-U1** – Each user account shall have a **unique identifier (UUID)** and **unique username/email** to enforce uniqueness constraints.

**LDR-U2** – Authentication credentials shall be stored **hashed and salted** (per NFR-S2).

**LDR-U3** – The database shall maintain a **last login timestamp** and **session token reference** to support session expiration (NFR-S7).

**LDR-U4** – Rate-limiting data (e.g., failed login attempts, input bursts) shall be stored per user to enforce security constraints (NFR-S8).

Player Profile and Settings Data
""""""""""""""""""""""""""""""""

**LDR-P1** – Player profile information shall include **avatar selection, display name, and optional bio**.

**LDR-P2** – Player settings (controls, audio, video, accessibility) shall be stored in a **separate table linked to the user account** to allow easy updates without affecting core account data.

**LDR-P3** – Default settings shall be referenced in a **template table**, allowing resets (FR9, FR14).

**LDR-P4** – All profile and settings data shall be **associated with exactly one user account** (foreign key constraint).

**LDR-P5** – The database shall support **versioning of settings** for backward compatibility if new features are added.

Gameplay Statistics Data
""""""""""""""""""""""""
**LDR-G1** – Each statistic (boxes moved, win/loss ratio, best time, best score, achievements) shall be **stored per user** and **historically recorded** for auditing and leaderboard purposes.

**LDR-G2** – Aggregated data (leaderboards, matchmaking metrics) shall be **derived from raw statistics tables**, not manually stored, to ensure consistency.

**LDR-G3** – Statistics updates shall be **transactional**, ensuring data consistency even in multiplayer conflicts (FR24–FR32, FR46–FR56).

**LDR-G4** – The system shall track **timestamps for each statistic update** to support time-based queries (e.g., seasonal leaderboards).

Game Session Data
""""""""""""""""""""

**LDR-S1** – Active game sessions shall have **unique session identifiers** and store **participating user IDs**, **game mode**, **session start and end times**.

**LDR-S2** – Game session data shall be **retained temporarily** and **discarded after session completion**, except for results required for statistics and achievements (FR83).

**LDR-S3** – Session-related moves and game state may be **stored in memory or temporary tables** for performance efficiency (NFR-P1–P4, NFR-R3).

**LDR-S4** – Session tables shall support **rollback mechanisms** in case of server failure to prevent corrupted states (NFR-R3).

Custom Map Data
""""""""""""""""""""

**LDR-M1** – Each map shall have a **unique map identifier**, **creator ID**, **validation status**, **timestamp of creation**, and **last modification timestamp**.

**LDR-M2** – Only maps **validated as solvable** shall have their status set to “approved” and be available to other players (FR84–FR86).

**LDR-M3** – Map data shall include **map layout (tiles, walls, boxes, plates)** in a **normalized schema** to optimize queries for multiplayer sessions.

**LDR-M4** – Versioning shall be supported to allow updates to maps without overwriting user-submitted versions.

Moderation and Report Data
"""""""""""""""""""""""""""""

**LDR-MOD1** – Reports shall include **report ID, reporter ID, reported player ID, reason code, timestamp, and current status**.

**LDR-MOD2** – Moderation actions shall be **linked to reports** and include **action type, moderator ID, timestamp, and outcome**.

**LDR-MOD3** – Historical moderation data shall be read-only for regular users and auditable for compliance (NFR-S5).

**LDR-MOD4** – Reports shall support status tracking visible to the reporting player (FR88).

Design Constraints
^^^^^^^^^^^^^^^^^^

**AC1** – The solution must follow a **Client–Server architecture**.

**AC2** – **Game session management, board generation, object placement, move validation, scoring, and cheating detection** must be performed on the server.

**AC3** – The architecture shall be **extensible to support additional client platforms** without redesigning server interfaces.

**AC4** – **Power-ups and power-downs** must always be enabled and cannot be disabled by players.

**AC5** – **Random gift box distribution** must enforce a bias where power-downs appear with a probability of one third.

**AC6** – The **AI engine** shall be independent of the core game engine.

**AC7** – **AI-based hints** shall be controllable at both server level and pregame lobby level.

**AC8** – **Spectating** shall be limited to Cooperative-Multiplayer and Competitive-Multiplayer modes.

**AC9** – **User settings** must be persisted server-side and linked to a player account.

**AC10** – **Server communication** shall be versioned and protocol-compatible with multiple client versions (Portability / Maintainability).

**AC11** – The architecture shall support **horizontal scaling**, allowing multiple game server instances to run concurrently for multiplayer sessions.

**AC12** – **Session state and active game data** shall be maintained in a way that allows **rollback or recovery** in case of server failures (Reliability / Availability).

**AC13** – All **authentication and security checks** (login, moderation, matchmaking) shall occur **server-side**.

**AC14** – **Custom map validation** shall be executed **on the server**, enforcing solvability and correctness before maps are made available.

**AC15** – Game clients shall remain **stateless regarding critical game logic**, with all authoritative decisions made server-side (Consistency / Anti-cheat).

**AC16** – The architecture shall allow **modular updates** to AI, matchmaking, and scoring algorithms **without downtime** or client changes (Maintainability).

Software System Attributes
^^^^^^^^^^^^^^^^^^^^^^^^^^^

Security
""""""""

**NFR-S1** – All client-server communication shall use **encrypted protocols (TLS 1.3 or higher)**.

**NFR-S2** – User credentials and profile data shall be stored securely using **industry-standard hashing and encryption**.

**NFR-S3** – Only authenticated users shall access personal profiles, saved settings, or custom maps.

**NFR-S4** – Multiplayer actions shall be **validated server-side** to prevent cheating or unauthorized game state modification.

**NFR-S5** – All moderation actions, reports, and administrative changes shall be **logged and auditable for at least 12 months**.

**NFR-S6** – AI hints shall only return information about the **current player’s own puzzle state** and shall **never reveal positions, moves, or scores of other players**.

**NFR-S7** – User sessions shall automatically **expire after a configurable period of inactivity** (default: 15 minutes), requiring re-authentication.

**NFR-S8** – The system shall implement **rate-limiting on rapid input bursts** to mitigate bots, while allowing legitimate fast player actions without noticeable delay.


Reliability
""""""""""""

**NFR-R1** – The system shall maintain active single-player sessions without data loss for at least **30 minutes** in the event of a client crash.

**NFR-R2** – Multiplayer sessions shall continue correctly when a client temporarily disconnects for up to **60 seconds**.

**NFR-R3** – Game state shall remain consistent across all clients in multiplayer modes, even in case of partial network failures.

**NFR-R4** – Solvability verification in the Map Editor shall always produce correct results; invalid boards shall never be saved.

**NFR-R5** – AI-based hints shall be generated deterministically to ensure reproducible guidance.

**NFR-R6** – In case of unrecoverable errors, the system shall notify affected users and terminate the session cleanly.


Availability
"""""""""""""

**NFR-A1** – The system shall achieve an uptime of **99.5% per month** for multiplayer servers.

**NFR-A2** – Login and authentication services shall respond within **2 seconds** 95% of the time under normal load.

**NFR-A3** – Multiplayer matchmaking and lobby services shall be available **24/7** except for scheduled maintenance.

**NFR-A4** – During maintenance, the system shall notify users at least **1 hour in advance**.

**NFR-A5** – If user actions are temporarily throttled or blocked (e.g., due to rate-limiting), the system shall **explicitly notify the user** of the remaining lockout duration instead of silently dropping requests.

Maintainability
""""""""""""""""

**NFR-M1** – New game modes shall be integrated without modifying the core Sokoban game engine.

**NFR-M2** – Power-ups, power-downs, and AI behaviors shall be configurable through external XML configuration files.

**NFR-M3** – The system shall allow updates to matchmaking rules and scoring algorithms without recompiling the client.

**NFR-M4** – The codebase shall achieve a modular structure enabling maintenance by developers with **less than 3 months of prior experience**.

**NFR-M5** – Map validation logic shall be maintainable such that rules or timeouts can be updated without downtime.

**NFR-M6** – An initial ASCII-based prototype UI shall allow later transition to a graphical UI without rewriting core logic.

**NFR-M7** – Community-related metadata (e.g., map ratings, approvals) shall be extensible without requiring schema redesign.


Portability
""""""""""""

**NFR-P1** – The client shall run on **Windows 11 and macOS 13 or higher** with identical functionality.

**NFR-P2** – The client shall support both **keyboard and standard gamepad input**.

**NFR-P3** – The system shall allow server deployment on at least **two major cloud providers** without modification of application logic.

**NFR-P4** – Saved user profiles and custom maps shall be **transferable between client platforms** without data loss.

**NFR-P5** – Multiplayer communication protocol shall remain consistent regardless of client operating system.

**NFR-P6** – User profile visibility settings (e.g., avatar, bio) shall be enforced consistently across all platforms and clients.


Other Requirements
^^^^^^^^^^^^^^^^^^^

Usablity
"""""""""""

**NFR-U1** – The user interface shall provide immediate visual feedback for user actions. During longer-taking processes, the client UI shall show a spinner/progress indicator.

**NFR-U2** – Key rebinding and settings changes shall be understandable without external documentation.

**NFR-U3** – Confirmation dialogs shall clearly indicate the consequences of the user’s action.

**NFR-U4** – AI hints shall be displayed in a non-intrusive manner and shall not block gameplay

**NFR-U5** – The user interface and main user flows shall allow players to complete common tasks with a minimal number of steps.

**NFR17** – The UI shall be visually simple and optimized for fast loading across supported client types.

Accessibility
""""""""""""""

**NFR-ACC1** – The system shall support **adjustable visual settings**, including font size, buttons, menus, contrast, and color-blind-friendly color schemes.

**NFR-ACC2** – All gameplay-relevant information shall be **conveyed through more than one sensory channel** (e.g., visual and audio) where applicable.

**NFR-ACC3** – Accessibility settings shall be **persisted per user account** and applied automatically on login.
