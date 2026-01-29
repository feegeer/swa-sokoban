Risks and Technical Debt
========================

Architecture Evaluation (ATAM)
------------------------------

The architecture was evaluated using the **Architecture Tradeoff Analysis Method (ATAM)** to assess how architectural decisions satisfy quality attributes and to identify sensitivity points, tradeoffs, and risks.


ATAM Use-Case Scenarios
~~~~~~~~~~~~~~~~~~~~~~~

**Scenario U1 – Multiplayer State Synchronization**

**Related Requirements:** FR88–FR95, FR25, FR41, NFR-P2, NFR-P9, AC2, AC15  

+-------------+------------------------------------------------------------+
| Stimulus    | Four players send movement inputs simultaneously           |
+-------------+------------------------------------------------------------+
| Environment | Normal network conditions                                  |
+-------------+------------------------------------------------------------+
| Response    | Server validates inputs and broadcasts updated game state  |
+-------------+------------------------------------------------------------+
| Measure     | State synchronization ≤ 200 ms (NFR-P2)                    |
+-------------+------------------------------------------------------------+

**Sensitivity Point:** Network latency and server processing time directly affect response time.  

**Tradeoff Point:** Strong server-side validation is required to ensure consistency and security but increases response time under high concurrency.


**Scenario U2 – Cheating Attempt**

**Related Requirements:** FR24, FR30, FR32–FR37, NFR-S4, AC2, AC15  

+-------------+------------------------------------------------------------+
| Stimulus    | Client sends invalid movement                              |
+-------------+------------------------------------------------------------+
| Environment | Multiplayer session                                        |
+-------------+------------------------------------------------------------+
| Response    | Server rejects action and state remains unchanged          |
+-------------+------------------------------------------------------------+
| Measure     | 100% of invalid actions rejected                           |
+-------------+------------------------------------------------------------+

**Sensitivity Point:** The completeness and correctness of server-side validation rules directly determine whether invalid or malicious client actions can alter game state.
 
**Non-Risk:** Centralized server authority ensures integrity by design.


**Scenario U3 – AI Hint Response**

**Related Requirements:** FR117–FR123, FR80–FR83, NFR-P3, AC6  

+-------------+------------------------------------------------------------+
| Stimulus    | Player requests AI hint                                    |
+-------------+------------------------------------------------------------+
| Environment | Single-player                                              |
+-------------+------------------------------------------------------------+
| Response    | AI service returns next optimal move                       |
+-------------+------------------------------------------------------------+
| Measure     | Response ≤ 2 sec (NFR-P3)                                  |
+-------------+------------------------------------------------------------+

**Sensitivity Point:** AI processing time directly affects system response time.

**Tradeoff Point:** Increasing AI solution accuracy requires more computational effort, which increases response time.



**Scenario U4 – Player Temporary Disconnect**

**Related Requirements:** NFR-R2, LDR-S4, AC12  

+-------------+------------------------------------------------------------+
| Stimulus    | Player disconnects for 30 seconds                          |
+-------------+------------------------------------------------------------+
| Environment | Multiplayer match                                          |
+-------------+------------------------------------------------------------+
| Response    | Session continues, player can rejoin                       |
+-------------+------------------------------------------------------------+
| Measure     | No game state loss (NFR-R2)                                |
+-------------+------------------------------------------------------------+

**Sensitivity Point:** Session persistence and state storage mechanisms affect recovery success.  

**Non-Risk:** Centralized state management supports recovery.


**Scenario U5 – Map Solvability Validation**

**Related Requirements:** FR124–FR131, NFR-P4, AC14  

+-------------+------------------------------------------------------------+
| Stimulus    | User submits custom map                                    |
+-------------+------------------------------------------------------------+
| Environment | Map Editor                                                 |
+-------------+------------------------------------------------------------+
| Response    | Server validates solvability                               |
+-------------+------------------------------------------------------------+
| Measure     | Validation ≤ 10 sec (NFR-P4)                               |
+-------------+------------------------------------------------------------+

**Sensitivity Point:** Algorithmic complexity of map validation strongly affects performance.  

**Risk:** Worst-case validation may exceed timeout.


ATAM Growth Scenarios
~~~~~~~~~~~~~~~~~~~~~

**Scenario G1 – Increased User Load**

**Related Requirements:** NFR-P5, AC11  

+-------------+------------------------------------------------------------+
| Stimulus    | System load doubles to 10,000 users                        |
+-------------+------------------------------------------------------------+
| Response    | Horizontal scaling of server instances                     |
+-------------+------------------------------------------------------------+
| Measure     | Core latency remains within limits                         |
+-------------+------------------------------------------------------------+

**Sensitivity Point:** Server CPU capacity and database throughput directly affect scalability.  

**Tradeoff Point:** Horizontal scaling improves scalability and availability but increases synchronization overhead.


**Scenario G2 – New Game Mode**

**Related Requirements:** NFR-M1, AC16  

+-------------+------------------------------------------------------------+
| Stimulus    | New multiplayer mode added                                 |
+-------------+------------------------------------------------------------+
| Response    | Integrated without modifying core engine                   |
+-------------+------------------------------------------------------------+
| Measure     | No regression in existing modes                            |
+-------------+------------------------------------------------------------+

**Sensitivity Point:** Module coupling affects extensibility.  

**Non-Risk:** Layered and modular design supports extension.


**Scenario G3 – AI Algorithm Upgrade**

**Related Requirements:** AC6, NFR-M3  

+-------------+------------------------------------------------------------+
| Stimulus    | AI algorithm replaced                                      |
+-------------+------------------------------------------------------------+
| Response    | AIService updated independently                            |
+-------------+------------------------------------------------------------+
| Measure     | No impact on game server                                   |
+-------------+------------------------------------------------------------+

**Sensitivity Point:** Interface contracts between AI service and game server affect independence.  

**Non-Risk:** Service isolation minimizes impact.


ATAM Findings Summary
~~~~~~~~~~~~~~~~~~~~~

+--------------------+--------------------------------------------------------------------------+
| Category           | Findings                                                                 |
+--------------------+--------------------------------------------------------------------------+
| Sensitivity Points | • Network latency and server processing time affect response time        |
|                    | • Completeness of validation rules affects system integrity              |
|                    | • AI processing time affects response time                               |
|                    | • Session persistence affects recovery reliability                       |
|                    | • Map validation algorithm complexity affects performance                |
|                    | • Server CPU capacity and DB throughput affect scalability               |
|                    | • Module coupling affects extensibility                                  |
|                    | • Interface contracts affect service independence                        |
+--------------------+--------------------------------------------------------------------------+
| Tradeoff Points    | • Strong validation improves consistency/security but increases response |
|                    |   time under concurrency                                                 |
|                    | • Higher AI accuracy increases computation effort and response time      |
|                    | • Horizontal scaling improves scalability but increases synchronization  |
|                    |   overhead and coordination complexity                                   |
+--------------------+--------------------------------------------------------------------------+
| Risks              | • Worst-case map validation may exceed timeout limits                    |
|                    | • Peak multiplayer load may saturate server or database resources        |
+--------------------+--------------------------------------------------------------------------+
| Non-Risks          | • Server authority protects game state integrity                         |
|                    | • Centralized state management enables session recovery                  |
|                    | • Modular and layered design supports extension of new game modes        |
|                    | • AI service isolation enables independent evolution                     |
+--------------------+--------------------------------------------------------------------------+

Technical Debt
--------------

At the current stage, no deliberate architectural compromises have been made, and therefore no technical debt has been incurred.

However, several architectural and technology decisions define potential future debt hotspots. These include:

• Centralized GameServer (Client–Server style) → risk of monolithic growth  
• Shared database access → tighter coupling between server and persistence  
• Concurrency handling in Communicating-Processes style → complexity and debugging overhead  
• Use of SignalR and EF Core → potential framework lock-in and performance constraints  
• Cross-language AI service → long-term maintenance complexity  

These are not technical debt yet, but architectural risk areas where future implementation shortcuts could accumulate debt. The project will reassess once concrete implementation tradeoffs are made.
