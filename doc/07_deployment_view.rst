Deployment View
===============

This chapter covers the deployment view of the Sokoban system.

.. figure:: _static/deployment_diagram.svg
   :width: 100%
   :align: center

As shown in the deployment diagram, the `GameClients` connect to a `GameServer` (selected by the `LoadBalancer`) via the public internet. `GameServers` can be hosted in the cloud, and the architecture allows for an arbitrary amount of servers. This allows for low latencies, minimal downtime and load balancing.

Each `GameServer` communicates with an external `AIHintService`. The architecture allows for multiple `AI Services`, but doesn't require them. At least one `AI Service` is required.

The database consists of two deployments: a `Level Files Bucket` and an `SQL Database`. The level files are stored separately in a bucket because for level files, a relational database like the `SQL Database` is not efficient as there are no meaningful relations on game maps. The `SQL Database` stores the remaining data.
