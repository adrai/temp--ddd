# All together

                                   (Replay)
                  +-------------- Repository <-------------+
                  |                                        |
                  v                                        |
      +-----> API (Write) ----> Domain-Events ------> Event-Store
     /          (DDD)                |
 Command                  +----------+
   /                      |          v
UI  <---------------------+      Projection ---------------+
   ^                                                       |
  Query                                                    |
     \                                                     v
      +------ API (Read) <--------------------------- View-Store


GraphQL
- Mutations (=> Commands)
- Subscriptions (=> Domain-Events)
- Queries
