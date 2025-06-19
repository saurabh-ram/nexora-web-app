# nexora

Nexora is an online review collaboration platform that contains reviews for movies, web series , miniseries, animes, etc.

</br>

# Architecture Overview

For my project, I use a hybrid data storage architecture combining Postgres as the _primary database_ and Redis as an _in-memory cache layer_.

1. Supabase Postgres manages persistent, structured data — like user profiles, posts, and relational data.

2. Upstash Redis caches frequently accessed data such as homepage posts, sessions, or API responses to reduce database load and improve latency.

</br>

# Why this combination?

1. **Performance:** Redis dramatically improves read speeds for hot data, reducing response times.

2. **Cost-efficiency:** Using Redis cache lowers the number of queries hitting Postgres, optimizing resource usage and staying within free-tier limits.

3. **Scalability:** Both services scale independently, supporting growth without a major redesign.

4. **Reliability:** Supabase Postgres ensures durable storage with ACID compliance and easy SQL querying.

5. **Developer Experience:** Both platforms offer simple SDKs, quick setup, and generous free tiers, enabling fast iteration and deployment.

</br>

# Cache Pattern & Data Flow

The data retrieval follows a cache-aside strategy:

1. Try to fetch data from Redis cache first.

2. On a cache miss or cache failure, retrieve data from Postgres.

3. Optionally, update Redis cache with fresh data and set appropriate expiration.

4. Implement error handling to fallback to Postgres if Redis is unavailable or bandwidth-limited (important with free tiers).

</br>

# Handling Free Tier Limits

1. Be mindful of bandwidth and command quotas on Redis.

2. Implement graceful degradation by falling back to Postgres when Redis is throttled.

3. Use TTL (time-to-live) to automatically expire cache entries and manage storage efficiently.

4. Monitor usage and optimize queries to stay within free-tier limits.

</br>

# Security & Access Control

1. Use environment variables and secure API keys for database and cache access.

2. Apply proper authentication and authorization (e.g., JWT tokens with Supabase).

3. Validate and sanitize inputs to prevent injection attacks.

</br>

# Future Scaling & Improvements

1. Add rate limiting to protect services.

2. Use background jobs or serverless functions to refresh cache asynchronously.

3. Consider auto-scaling or upgrading plans if user base grows beyond free tiers.

