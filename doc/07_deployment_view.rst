Deployment View
===============

This chapter covers the deployment view of the Sokoban system.

.. figure:: _static/deployment_diagram.svg
   :width: 100%
   :align: center

As shown in the deployment diagram, the `GameClient` and `LevelDesigner` connect to the `GameServer` via the public internet. The `GameClient` uses HTTPS/WSS to enable real-time gameplay with websockets and `LevelDesigner` communicates via HTTPS to publish, delete, and edit maps.

The `GameServer` communicates to the `AIHintService` (`AI/LLM API`) via HTTPS for hint generation.

The database consists of two deployments: a `Level Files Bucket` and an `SQL Database`. The `Level Files Bucket` is accessed via HTTPS. The level files are stored separately in a bucket because for level files, a relational database like the `SQL Database` is not efficient as there are no meaningful relations on game maps. The `SQL Database` stores the remaining data and is accessed through a TCP connection.