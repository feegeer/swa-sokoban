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

Provides the underlying infrastructure for multiplayer gameplay, synchronization, and authoritative server validation.

.. rubric:: Stimulus / Response Sequence
  :heading-level: 5

- The authenticated user interacts with the client application during gameplay, e.g. by pressing keys.
- At fixed, predefined time intervals, the client transmits the user's input actions to the server together with the user's session token.
- Upon receiving the client input, the server validates the session token to ensure the action originates from an authenticated and active user.
- The server validates each received input action against the authoritative game state, which ensures that the user action is allowed in the current game state and does not violate any logical limits (like over-long powerup durations)
- The server updates the game state based on the validated input actions.
- At fixed, predefined time intervals, the server distributes the updated game state to all connected clients.
- Each client updates its local presentation of the game world based on the received game state.
- If the server detects invalid, unauthorized, or inconsistent input actions, the server rejects the action and may trigger corrective or security measures.

.. rubric:: Associated Functional Requirements
  :heading-level: 5

- **FR1**: The server shall require a valid session token for all gameplay requests comming from clients.
- **FR2**: The server shall validate the session token of each received gameplay input message to make sure that the user is authenticated and the session is active.
- **FR3**: The server shall reject gameplay input messages originating from invalid, expired, or unauthenticated sessions.
- **FR4**: The client shall transmit user input actions to the server at fixed, predefined time intervals during active gameplay.
- **FR5**: Each gameplay request comming from a client shall include the user's current input actions and the associated session token.
- **FR6**: The server shall validate all received user input actions against the current, validated game state.
- **FR7**: The server shall verify that each input action is permitted for the user's current game state and role.
- **FR8**: The server shall enforce game rule and logical constraints regarding the game when validating the input actions.
- **FR9**: The server shall maintain the authoritative game state for all connected clients.
- **FR10**: The server shall update the authoritative game state exclusively based on validated user input actions.
- **FR11**: The server shall ignore or reject any input actions that fail validation and shall not apply them to the authoritative game state.
- **FR12**: The server shall distribute the updated authoritative game state to all connected clients at fixed, predefined time intervals.
- **FR13**: Each client shall update its local game representation exclusively based on the authoritative game state received from the server.
- **FR14**: If invalid, unauthorized, or inconsistent input actions are detected, the server shall respond according to predefined rules.
- **FR15**: The system shall ensure that invalid input actions do not compromise the consistency or integrity of the authoritative game state.

.. rubric:: Sokoban Game Engine (Core Logic)
  :heading-level: 4

.. rubric:: Introduction / Purpose
  :heading-level: 5

.. rubric:: Stimulus / Response Sequence
  :heading-level: 5

.. rubric:: Associated Functional Requirements
  :heading-level: 5

.. rubric:: Power-Ups and Power-Downs
  :heading-level: 4

.. rubric:: Introduction / Purpose
  :heading-level: 5

.. rubric:: Stimulus / Response Sequence
  :heading-level: 5

.. rubric:: Associated Functional Requirements
  :heading-level: 5

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
