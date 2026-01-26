=========================
Goals
=========================

The overall objective of the system is to provide a **distributed, multiplayer-capable Sokoban platform**
that delivers engaging gameplay, supports community interaction, and ensures fairness, performance,
and long-term sustainability.

------------------------------------------------------------
System Goals — Sokoban System
------------------------------------------------------------

* **SG1**: Enable execution of Sokoban gameplay while enforcing rules server-side

  - **SG1.1** Execute Sokoban gameplay  
  - **SG1.2** Apply core movement and box mechanics  
  - **SG1.3** Maintain authoritative game state  
  - **SG1.4** Apply power-ups and power-downs  
  - **SG1.5** Synchronize multiplayer state  


* **SG2**: Support multiple gameplay modes

  - **SG2.1** Provide single-player mode  
  - **SG2.2** Provide cooperative multiplayer mode  
  - **SG2.3** Provide competitive multiplayer mode  
  - **SG2.4** Provide tutorial mode  
  - **SG2.5** Provide spectator mode  


* **SG3**: Provide AI-assisted gameplay guidance

  - **SG3.1** Provide contextual hints  
  - **SG3.2** Generate next optimal move suggestions  
  - **SG3.3** Evaluate session-level state for hints  
  - **SG3.4** Evaluate level difficulty  


* **SG4**: Support community-driven content creation

  - **SG4.1** Provide level editor  
  - **SG4.2** Support level design and testing  
  - **SG4.3** Validate level solvability  
  - **SG4.4** Allow saving and sharing of custom levels  
  - **SG4.5** Support creativity in level creation  


* **SG5**: Support user identity and account management

  - **SG5.1** Manage user accounts  
  - **SG5.2** Manage profiles and friend lists  
  - **SG5.3** Provide settings configuration  
  - **SG5.4** Store account and statistics data  


* **SG6**: Ensure fair and secure gameplay

  - **SG6.1** Use server authority for game logic  
  - **SG6.2** Validate player actions  
  - **SG6.3** Use anti-cheat mechanisms  
  - **SG6.4** Provide cheating report mechanisms  
  - **SG6.5** Enforce fairness in matchmaking  


* **SG7**: Provide scalable infrastructure for multiplayer sessions

  - **SG7.1** Support session organization and coordination  
  - **SG7.2** Support ranking-based matchmaking  
  - **SG7.3** Initialize and manage game sessions  
  - **SG7.4** Maintain consistent session state  
  - **SG7.5** Handle concurrent player interactions  


* **SG8**: Ensure high usability and accessibility

  - **SG8.1** Provide ease of navigation  
  - **SG8.2** Provide accessibility settings  
  - **SG8.3** Support personalization options  
  - **SG8.4** Provide intuitive game mechanics  


* **SG9**: Deliver high performance and responsiveness

  - **SG9.1** Ensure low latency interaction  
  - **SG9.2** Ensure responsive system behavior  
  - **SG9.3** Provide efficient synchronization  
  - **SG9.4** Support large numbers of concurrent users  


* **SG10**: Ensure reliable operation under failures

  - **SG10.1** Provide reliable session handling  
  - **SG10.2** Support recovery from disconnects  
  - **SG10.3** Maintain consistent state management  


* **SG11**: Support long-term product success and player retention

  - **SG11.1** Keep players engaged  
  - **SG11.2** Encourage positive community  
  - **SG11.3** Prevent player churn  
  - **SG11.4** Support monetization potential  
  - **SG11.5** Help the game spread  


------------------------------------------------------------
Stakeholder Goals
------------------------------------------------------------

User Goals (Generic System User)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* **UG1**: Access the system securely and reliably  
* **UG2**: Navigate the system intuitively  
* **UG3**: Manage personal account information and preferences  
* **UG4**: Configure gameplay, audiovisual, and accessibility settings  
* **UG5**: Experience a usable, accessible, and personalized interface  


Player Goals (is-a User)
~~~~~~~~~~~~~~~~~~~~~~~~

* **PG1**: Play Sokoban in different gameplay modes  
* **PG2**: Learn the game through tutorials, observation, and assistance  
* **PG3**: Experience smooth, responsive, and balanced gameplay  
* **PG4**: Be appropriately challenged without frustration  
* **PG5**: Play fairly in competitive environments  
* **PG6**: Engage socially through cooperation, competition, and spectating  
* **PG7**: Have fun and remain engaged over repeated sessions  


Level Designer Goals (is-a User)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* **LDG1**: Create custom Sokoban levels  
* **LDG2**: Edit and refine created levels  
* **LDG3**: Ensure levels are solvable and balanced  
* **LDG4**: Test levels before publication  
* **LDG5**: Share levels with other players  
* **LDG6**: Express creativity with minimal technical barriers  


Customer Goals — Hawki
~~~~~~~~~~~~~~~~~~~~~~

* **CG1**: Deliver a successful Sokoban system  
* **CG2**: Retain players and encourage repeated engagement  
* **CG3**: Prevent early player drop-off  
* **CG4**: Foster a positive and fair community  
* **CG5**: Enable sustainable monetization  
* **CG6**: Support organic growth through community and sharing  


------------------------------------------------------------
Goal Model
------------------------------------------------------------

The following diagram provides an overview of the complete goal structure and
illustrates refinement relationships between system and stakeholder goals.

.. figure:: images/goal_model.svg
   :width: 100%
   :align: center

   Global Goal Model of the Distributed Sokoban System
