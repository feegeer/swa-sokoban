Risks and Technical Debt
========================

Architecture Evaluation (ATAM)
------------------------------

The architecture was evaluated using the **Architecture Tradeoff Analysis Method (ATAM)** to assess how architectural decisions satisfy quality attributes and to identify sensitivity points, tradeoffs, and risks.


ATAM Use-Case Scenarios
~~~~~~~~~~~~~~~~~~~~~~~

**Scenario U1 – Multiplayer State Synchronization**

**Related Requirements:** FR88–FR95, FR25, FR41, NFR-P2, NFR-P9, AC2, AC15  

+-------------+--------------------------------------------------------------------+
| Scenario    | Multiplayer State Synchronization                                  |
+-------------+--------------------------------------------------------------------+
| Quality     | Performance, Consistency                                           |
+-------------+--------------------------------------------------------------------+
| Environment | Normal network conditions                                          |
+-------------+--------------------------------------------------------------------+
| Stimulus    | Four players send movement inputs simultaneously                   |
+-------------+--------------------------------------------------------------------+
| Response    | Server validates inputs and broadcasts updated game state          |
+-------------+--------------------------------------------------------------------+
| Steps       | Client input → Server validation → Game engine update → Broadcast  |
+-------------+--------------------------------------------------------------------+

+-------------------------------+------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------+
| Architectural Decisions       | Risks                                                                                                                        | Sensitivity Points                                                                                                               | Tradeoffs                                                                                                                    |
+===============================+==============================================================================================================================+==================================================================================================================================+==============================================================================================================================+
| AD-5 Client–Server Authority  | Centralized validation and state updates can overload server CPU and network under peak load, causing delays (NFR-P2, P9).   | Sensitive to **server CPU capacity** and **network latency**; small increases directly raise synchronization delay.              | Improves **consistency and security**, but limits **performance and scalability** due to centralization.                     |
+-------------------------------+------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------+
| AD-8 Publish–Subscribe        | Frequent broadcasts may exceed bandwidth or subscriber capacity, leading to backlog and delayed delivery.                    | Sensitive to **network bandwidth** and **message rate**; higher update frequency quickly increases end-to-end delay.             | Enhances **scalability and modularity**, but reduces **latency predictability** under network load.                          |
+-------------------------------+------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------+
| AD-7 Communicating Processes  | Concurrent input handling can cause race conditions and inconsistent state without proper synchronization.                   | Sensitive to **thread pool size**, scheduling, and contention; small changes affect latency and correctness.                     | Increases **throughput and parallelism**, but adds **architectural and debugging complexity**.                               |
+-------------------------------+------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------+

**Reasoning:** Strong server authority (AC2, AC15) guarantees consistency.

**Scenario U2 – Cheating Attempt**

**Related Requirements:** FR24, FR30, FR32–FR37, NFR-S4, AC2, AC15

+-------------+---------------------------------------------------------------+
| Scenario    | Cheating Attempt                                              |
+-------------+---------------------------------------------------------------+
| Quality     | Security, Integrity                                           |
+-------------+---------------------------------------------------------------+
| Environment | Multiplayer session                                           |
+-------------+---------------------------------------------------------------+
| Stimulus    | Client sends invalid movement                                 |
+-------------+---------------------------------------------------------------+
| Response    | Server rejects action and state remains unchanged             |
+-------------+---------------------------------------------------------------+
| Steps       | Client input → Server validation → Reject                     |
+-------------+---------------------------------------------------------------+

+------------------------------+--------------------------------------------------------------------------------------------------+--------------------------------------------------------------+------------------------------------------------------------------------+
| Architectural Decisions      | Risks                                                                                            | Sensitivity Points                                           | Tradeoffs                                                              |
+==============================+==================================================================================================+==============================================================+========================================================================+
| AD-5 Client–Server Authority | Incomplete validation rules could allow illegal state transitions.                               | Sensitive to **validation rule completeness** and CPU load.  | Improves **security and integrity**, but adds **processing overhead**. |
+------------------------------+--------------------------------------------------------------------------------------------------+--------------------------------------------------------------+------------------------------------------------------------------------+ 

**Reasoning:** Centralized validation ensures illegal actions cannot alter game state.



**Scenario U3 – AI Hint Response**

**Related Requirements:** FR117–FR123, FR80–FR83, NFR-P3, AC6

+-------------+---------------------------------------------------------------+
| Scenario    | AI Hint Response                                              |
+-------------+---------------------------------------------------------------+
| Quality     | Performance, Modifiability                                    |
+-------------+---------------------------------------------------------------+
| Environment | Single-player                                                 |
+-------------+---------------------------------------------------------------+
| Stimulus    | Player requests AI hint                                       |
+-------------+---------------------------------------------------------------+
| Response    | AI service returns next optimal move                          |
+-------------+---------------------------------------------------------------+
| Steps       | Client → Server → AI Service → Response                       |
+-------------+---------------------------------------------------------------+

+-------------------------------+--------------------------------------------------------------------------------------------------+------------------------------------------------------+--------------------------------------------------------------+
| Architectural Decisions       | Risks                                                                                            | Sensitivity Points                                   | Tradeoffs                                                    |
+===============================+==================================================================================================+======================================================+==============================================================+
| AD-1 AI Service Decomposition | Network delays and heavy AI computation can increase response time.                              | Sensitive to **AI processing time** and RPC latency. | Improves **modifiability**, but increases **latency**.       |
+-------------------------------+--------------------------------------------------------------------------------------------------+------------------------------------------------------+--------------------------------------------------------------+

**Reasoning:** AI isolation enables independent evolution but adds communication overhead.




**Scenario U4 – Player Temporary Disconnect**

**Related Requirements:** NFR-R2, LDR-S4, AC12

+-------------+---------------------------------------------------------------+
| Scenario    | Player Temporary Disconnect                                   |
+-------------+---------------------------------------------------------------+
| Quality     | Reliability, Availability                                     |
+-------------+---------------------------------------------------------------+
| Environment | Multiplayer match                                             |
+-------------+---------------------------------------------------------------+
| Stimulus    | Player disconnects for 30 seconds                             |
+-------------+---------------------------------------------------------------+
| Response    | Session continues, player can rejoin                          |
+-------------+---------------------------------------------------------------+
| Steps       | Disconnect → Server retains state → Reconnect                 |
+-------------+---------------------------------------------------------------+

+-----------------------------+--------------------------------------------------------------------------------------------------+----------------------------------------------+-------------------------------------------------------------+
| Architectural Decisions     | Risks                                                                                            | Sensitivity Points                           | Tradeoffs                                                   |
+=============================+==================================================================================================+==============================================+=============================================================+
| AD-6 Shared Data (DB)       | Database outages or slow persistence can prevent session recovery.                               | Sensitive to **persistence mechanisms**.     | Improves **reliability**, but adds **DB dependency**.       |
+-----------------------------+--------------------------------------------------------------------------------------------------+----------------------------------------------+-------------------------------------------------------------+

**Reasoning:** Persistent state storage allows session recovery after disconnections.



**Scenario U5 – Map Solvability Validation**

**Related Requirements:** FR124–FR131, NFR-P4, AC14

+-------------+---------------------------------------------------------------+
| Scenario    | Map Solvability Validation                                    |
+-------------+---------------------------------------------------------------+
| Quality     | Performance, Correctness                                      |
+-------------+---------------------------------------------------------------+
| Environment | Map Editor                                                    |
+-------------+---------------------------------------------------------------+
| Stimulus    | User submits custom map                                       |
+-------------+---------------------------------------------------------------+
| Response    | Server validates solvability                                  |
+-------------+---------------------------------------------------------------+
| Steps       | Submit → Solver → Approve or reject                           |
+-------------+---------------------------------------------------------------+

+-------------------------------+--------------------------------------------------------------------------------------------------+--------------------------------------------------+-------------------------------------------------------------------------------+
| Architectural Decisions       | Risks                                                                                            | Sensitivity Points                               | Tradeoffs                                                                     |
+===============================+==================================================================================================+==================================================+===============================================================================+
| AD-5 Client–Server Authority  | Computationally heavy validation may delay other server operations under concurrent load.        | Sensitive to **server CPU capacity**.            | Ensures **correctness and security**, but reduces **performance** under load. |
+-------------------------------+--------------------------------------------------------------------------------------------------+--------------------------------------------------+-------------------------------------------------------------------------------+
| AD-7 Communicating Processes  | Concurrent sessions and solver execution may compete for processing resources.                   | Sensitive to **task scheduling and contention**. | Improves **throughput**, but increases **latency variability**.               | 
+-------------------------------+--------------------------------------------------------------------------------------------------+--------------------------------------------------+-------------------------------------------------------------------------------+

**Reasoning:** Validation is centralized in the GameServer (AC14), ensuring correctness, but performance depends on server resource availability and concurrency.



ATAM Growth Scenarios
~~~~~~~~~~~~~~~~~~~~~

**Scenario G1 – Increased User Load**

**Related Requirements:** NFR-P5, AC11

+-------------+---------------------------------------------------------------+
| Scenario    | Increased User Load                                           |
+-------------+---------------------------------------------------------------+
| Quality     | Scalability, Performance                                      |
+-------------+---------------------------------------------------------------+
| Environment | Peak operational conditions                                   |
+-------------+---------------------------------------------------------------+
| Stimulus    | System load doubles to 10,000 concurrent users                |
+-------------+---------------------------------------------------------------+
| Response    | Horizontal scaling of server instances                        |
+-------------+---------------------------------------------------------------+
| Steps       | Load increase → Additional nodes → Load balancing             |
+-------------+---------------------------------------------------------------+

+-------------------------------+--------------------------------------------------------------------------------------------------+------------------------------------------------+---------------------------------------------------------------------------------------+
| Architectural Decisions       | Risks                                                                                            | Sensitivity Points                             | Tradeoffs                                                                             |
+===============================+==================================================================================================+================================================+=======================================================================================+
| AD-5 Client–Server Authority  | Centralized coordination can create processing bottlenecks under high load.                      | Sensitive to **server CPU capacity**.          | Ensures **control and consistency**, but limits **scalability**.                      |
+-------------------------------+--------------------------------------------------------------------------------------------------+------------------------------------------------+---------------------------------------------------------------------------------------+
| AD-9 Deployment Style         | Inter-node synchronization overhead may reduce performance gains.                                | Sensitive to **network latency between nodes**.| Improves **availability and scalability**, but increases **coordination complexity**. |
+-------------------------------+--------------------------------------------------------------------------------------------------+------------------------------------------------+---------------------------------------------------------------------------------------+
| AD-6 Shared Data (DB)         | High concurrency may cause database contention and slower transactions.                          | Sensitive to **database throughput**.          | Maintains **data consistency**, but reduces **performance under load**.               |
+-------------------------------+--------------------------------------------------------------------------------------------------+------------------------------------------------+---------------------------------------------------------------------------------------+

**Reasoning:** Horizontal scaling improves capacity, but performance depends on DB throughput and coordination efficiency.

**Scenario G2 – New Game Mode Integration**

**Related Requirements:** NFR-M1, AC16

+-------------+---------------------------------------------------------------+
| Scenario    | New Game Mode Integration                                     |
+-------------+---------------------------------------------------------------+
| Quality     | Modifiability, Maintainability                                |
+-------------+---------------------------------------------------------------+
| Environment | System evolution                                              |
+-------------+---------------------------------------------------------------+
| Stimulus    | A new multiplayer mode is added                               |
+-------------+---------------------------------------------------------------+
| Response    | Integrated without modifying core engine                      |
+-------------+---------------------------------------------------------------+
| Steps       | New mode implementation → Integration via abstraction layer   |
+-------------+---------------------------------------------------------------+

+-------------------------------+--------------------------------------------------------------------------------------------------+-----------------------------------------------+---------------------------------------------------------------------+
| Architectural Decisions       | Risks                                                                                            | Sensitivity Points                            | Tradeoffs                                                           |
+===============================+==================================================================================================+===============================================+=====================================================================+
| AD-1 Decomposition Style      | Poorly defined boundaries may increase integration complexity.                                   | Sensitive to **module coupling**.             | Improves **extensibility**, but adds **integration overhead**.      |
+-------------------------------+--------------------------------------------------------------------------------------------------+-----------------------------------------------+---------------------------------------------------------------------+
| AD-4 Generalisation Style     | Changes in base abstractions may affect all derived modes.                                       | Sensitive to **abstraction stability**.       | Promotes **reuse**, but increases **ripple-effect risk**.           |
+-------------------------------+--------------------------------------------------------------------------------------------------+-----------------------------------------------+---------------------------------------------------------------------+
| AD-2 Layered Style            | Layer boundary violations may reduce modularity over time.                                       | Sensitive to **dependency discipline**.       | Enhances **maintainability**, but may add **performance overhead**. |
+-------------------------------+--------------------------------------------------------------------------------------------------+-----------------------------------------------+---------------------------------------------------------------------+

**Reasoning:** Modular design allows extension, but stability depends on abstraction integrity.



**Scenario G3 – AI Algorithm Upgrade**

**Related Requirements:** AC6, NFR-M3

+-------------+---------------------------------------------------------------+
| Scenario    | AI Algorithm Upgrade                                          |
+-------------+---------------------------------------------------------------+
| Quality     | Modifiability                                                 |
+-------------+---------------------------------------------------------------+
| Environment | System evolution                                              |
+-------------+---------------------------------------------------------------+
| Stimulus    | AI algorithm replaced                                         |
+-------------+---------------------------------------------------------------+
| Response    | AIService updated independently                               |
+-------------+---------------------------------------------------------------+
| Steps       | AI module update → Service redeploy → Interface validation    |
+-------------+---------------------------------------------------------------+

+-------------------------------+--------------------------------------------------------------------------------------------------+-----------------------------------------------+-------------------------------------------------------------------------+
| Architectural Decisions       | Risks                                                                                            | Sensitivity Points                            | Tradeoffs                                                               |
+===============================+==================================================================================================+===============================================+=========================================================================+
| AD-1 AI Service Decomposition | Interface changes may break compatibility with the game server.                                  | Sensitive to **API contract stability**.      | Enables **independent evolution**, but increases **integration risk**.  |
+-------------------------------+--------------------------------------------------------------------------------------------------+-----------------------------------------------+-------------------------------------------------------------------------+
| AD-5 Client–Server Authority  | Variations in AI processing time may affect response consistency.                                | Sensitive to **AI processing latency**.       | Maintains **central control**, but reduces **response predictability**. |
+-------------------------------+--------------------------------------------------------------------------------------------------+-----------------------------------------------+-------------------------------------------------------------------------+

**Reasoning:** AI isolation allows upgrades, but system stability depends on interface consistency.


Technical Debt
--------------

Although no intentional shortcuts were made, the current architecture introduces **structural and technological debt risk** inherent to the selected architectural styles and technologies. These do not represent defects, but **long-term cost drivers** that will require future mitigation.

Architectural Debt
~~~~~~~~~~~~~~~~~~

**Centralized GameServer (AD-5 Client–Server)**

All game logic, validation, synchronization, and solver execution reside in a single authoritative server.  
This creates future debt in the form of:

- limited horizontal scalability
- complex state management
- potential monolithic growth of server responsibilities

**Shared Database Access (AD-6 Shared-Data)**

Tight coupling between GameServer logic and database schema increases the cost of schema evolution and risks ripple effects across modules.

**Communicating-Processes Concurrency (AD-7)**

Concurrency management introduces hidden complexity in:

- synchronization correctness
- nondeterministic bugs
- testing difficulty

This represents **complexity debt**, which grows with system scale.


Technology Debt
~~~~~~~~~~~~~~~

**SignalR Dependency**

Tight integration with SignalR for real-time messaging may limit flexibility if performance requirements exceed framework capabilities.

**EF Core ORM Usage**

ORM abstraction improves productivity but can cause:

- inefficient queries
- hidden performance costs
- migration complexity at scale

**Cross-Language AI Service (Python ↔ .NET)**

Multi-language stack increases:

- integration testing cost
- deployment complexity
- operational troubleshooting overhead


Operational Debt
~~~~~~~~~~~~~~~~

Multi-service deployment (GameServer, AIService, DB) increases:

- monitoring requirements
- orchestration complexity
- fault diagnosis difficulty

These debt items are **inherent consequences of architectural decisions**.  
They will require **future architectural evolution**, especially if system scale, performance demands, or operational complexity increase.
