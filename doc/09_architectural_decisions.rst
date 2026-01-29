Architectural Decisions
=======================

The system architecture was reviewed using the **Module**, **Component-and-Connector (C&C)**, and **Allocation** viewtypes as defined by Clements et al. The applied architectural styles constrain architectural decisions and support key non-functional requirements such as scalability, maintainability, performance, and security.

+---------+--------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------+
| ID      | Decision (Style & Viewtype)                      | Rationale (Quality Attributes Supported)                                                                                         |
+=========+==================================================+==================================================================================================================================+
| AD-1    | **Decomposition Style (Module Viewtype)**        | System decomposed into GameClient, GameServer, AIService and Database modules. Supports **modifiability**, **understandability**,|
|         |                                                  | and **parallel development** by isolating responsibilities.                                                                      |
+---------+--------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------+
| AD-2    | **Layered Style (Module Viewtype)**              | Clear layers: Presentation (Client) → Application Logic (Server) → Data (DB). Enforces dependency direction and improves         |
|         |                                                  | **maintainability**, **information hiding**, and **change isolation**.                                                           |
+---------+--------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------+
| AD-3    | **Use (Usage) Style (Module Viewtype)**          | GameClient *uses* GameServer APIs; GameServer *uses* Database and AIService. Enables **impact analysis** and incremental         |
|         |                                                  | development.                                                                                                                     |
+---------+--------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------+
| AD-4    | **Generalisation Style (Module Viewtype)**       | Game modes (Single, Coop, Competitive) specialize a common game session abstraction. Supports **extensibility** and evolution    |
|         |                                                  | of new modes (NFR-M1).                                                                                                           |
+---------+--------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------+
| AD-5    | **Client–Server Style (C&C Viewtype)**           | Clients request services from centralized GameServer. Supports **security**, **scalability**, and central authority over game    |
|         |                                                  | logic (AC1, AC2).                                                                                                                |
+---------+--------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------+
| AD-6    | **Shared-Data Style (C&C Viewtype)**             | Database acts as shared repository accessed by GameServer. Ensures **consistency**, **persistence**, and supports transactional  |
|         |                                                  | integrity (LDR-G3, NFR-P12).                                                                                                     |
+---------+--------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------+
| AD-7    | **Communicating-Processes Style (C&C Viewtype)** | GameServer handles multiple concurrent sessions and processes client requests concurrently. Supports **concurrency**,            |
|         |                                                  | **responsiveness**, and **performance**.                                                                                         |
+---------+--------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------+
| AD-8    | **Publish-Subscribe Style (C&C Viewtype)**       | Server broadcasts updated authoritative game state to all clients. Clients subscribe to state updates. Supports **scalability**, |
|         |                                                  | **loose coupling**, and efficient state synchronization.                                                                         |
+---------+--------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------+
| AD-9    | **Deployment Style (Allocation Viewtype)**       | Allocation of GameClient to user devices, GameServer to server nodes, AIService to separate compute node, and DB to storage node.|
|         |                                                  | Enables **performance analysis**, **availability**, and **fault isolation**.                                                     |
+---------+--------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------+
| AD-10   | **Implementation Style (Allocation Viewtype)**   | Modules organized into separate implementation units (client, server, AI, DB schema). Supports **build management**,             |
|         |                                                  | **maintainability**, and configuration control.                                                                                  |
+---------+--------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------+
| AD-11   | **Work-Assignment Style (Allocation Viewtype)**  | Different teams can work on Client, Server, AI, and DB independently. Supports **project scalability** and efficient resource    |
|         |                                                  | allocation.                                                                                                                      |
+---------+--------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------+
| AD-12   | **MVC Pattern (Architectural Pattern)**          | Client uses MVC for UI separation. Improves **usability**, **maintainability**, and supports cross-platform UI evolution.        |
+---------+--------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------+
| AD-13   | **State–Logic–Display (3-Tier) Pattern**         | Clear separation of UI (Client), logic (Server), and data (DB). Supports **scalability**, **maintainability**, and **security**. |
+---------+--------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------+

All architectural decisions listed in this section are **accepted** and define the current baseline architecture of the system.  
They reflect the conceptual design stage and may be refined during implementation if new constraints or performance data require adjustments.

Technologies and Tools
----------------------

The selected technologies are consistent with the documented architecture and support the required quality attributes (Section 4.2).

Technology Decisions per Architectural Element
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

+------------------------+-----------------------------------------+---------------------------------------------------------------+------------------------------------------------------------+
| Architectural Element  | Technology                              | Architectural Fit                                             | Quality Attributes Supported                               |
+========================+=========================================+===============================================================+============================================================+
| GameClient             | TypeScript + React                      | Fits **Layered** and **MVC** structure of the client          | Maintainability, Modifiability, Portability, Usability     |
+------------------------+-----------------------------------------+---------------------------------------------------------------+------------------------------------------------------------+
| GameServer             | .NET 8 + ASP.NET Core + SignalR         | Implements **Client–Server**, **Communicating-Processes**,    | Performance, Scalability, Security, Reliability            |
|                        |                                         | and authoritative game logic                                  |                                                            |
+------------------------+-----------------------------------------+---------------------------------------------------------------+------------------------------------------------------------+
| Database               | PostgreSQL + EF Core                    | Realizes **Shared-Data Style** with transactional consistency | Data Consistency, Reliability, Persistence, Maintainability|
+------------------------+-----------------------------------------+---------------------------------------------------------------+------------------------------------------------------------+
| AIService              | Python + FastAPI                        | Independent service in **Client–Server** structure            | Modifiability, Scalability, Fault Isolation                |
+------------------------+-----------------------------------------+---------------------------------------------------------------+------------------------------------------------------------+

Tooling Decisions
~~~~~~~~~~~~~~~~~

The development and operational toolchain supports collaboration, automation, and deployment consistency.

+----------------------+-----------------------------------------------+
| Tool Category        | Tools                                         |
+======================+===============================================+
| Version Control      | Git, GitHub / GitLab                          |
+----------------------+-----------------------------------------------+
| CI/CD                | GitHub Actions / GitLab CI                    |
+----------------------+-----------------------------------------------+
| Containerization     | Docker, Docker Compose                        |
+----------------------+-----------------------------------------------+
| Testing              | xUnit (.NET), Jest (React)                    |
+----------------------+-----------------------------------------------+
| Modeling & Docs      | Visual Paradigm / draw.io, Sphinx             |
+----------------------+-----------------------------------------------+
