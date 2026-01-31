Crosscutting Concepts
=====================


This section describes architectural concepts and design principles that apply consistently across multiple system components of the Sokoverse.


Server Authority and Validation
-------------------------------

| **Description**
| The system follows a server-authoritative architecture. All game-relevant logic is executed and validated on the Game Server.

| **Key Rules**
| Clients only submit intended actions (e.g., move direction, power-up activation)
| The Game Server validates all actions against the current game state
| The authoritative game state is maintained exclusively on the server
| Clients never modify the game state directly

| **Applies To**
| Game Client
| Game Server

| **Goals Addressed**
| SG6 (Fair and secure gameplay)
| SG7 (Consistent session state)
| SG10 (Reliable operation)

Error Handling and Feedback
---------------------------

| **Description**
| Errors are handled in a consistent and user-aware manner across all clients.

| **Key Rules**
| Services return standardized error responses
| Clients translate technical errors into user-friendly messages
| Errors do not expose internal system details
| Recoverable errors (e.g., temporary disconnects) trigger retry or recovery mechanisms

| **Applies To**
| Game Client
| Level Designer Client
| All service interfaces

| **Goals Addressed**
| SG8 (Usability and accessibility)
| SG10 (Reliable operation under failures)

AI Assistance Integration
-------------------------

| **Description**
| AI functionality is implemented as a separate, stateless service that provides gameplay assistance without influencing authoritative game logic.

| **Key Rules**
| AI services only analyze game state; they do not modify it
| AI hints are advisory and optional
| AI services access game state indirectly via the Game Server
| AI latency must not block gameplay progression
| AI services and requests are controlled by the game server and not by the clients

| **Applies To**
| AI Service
| Game Server
| Game Client (indirectly)

| **Goals Addressed**
| SG3 (AI-assisted gameplay guidance)
| SG1.3 (Authoritative game state)
| SG9 (Responsiveness)
