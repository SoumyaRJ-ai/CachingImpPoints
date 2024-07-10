# Caching overview

- Cache is a smaller and faster memory which stores copies of the frequently used data. Content like HTML pages, images, files, web objects, etc are stored in cache to improve the efficiency and overall performance of the application.
- A cache is typically stored in memory or on disk. A memory cache is normally faster to read from than a disk cache. But, does not survive system restarts.
- Caches are widely used in most of the high volume applications to:
  - Reduce latency
  - Increase capability
  - Improve App Availability
- Types of caching
  - Web caching: Web Caching helps reduce overall network traffic and latency.
    - Browser caching
      - Helps individual users to quickly navigate pages they have recently visited.
      - This process requires Cache-Control and ETag headers to be present to instruct the user’s browser to cache certain files, for a certain period of time.
    - Proxy and Gateway caching
      - Data that does not change frequently and can be cached for longer periods of time is cached on Proxy or Gateway servers.
        E.g.: DNS data that resolve domain names to the IP address
  - Database caching
    - Data Caching is very important for DB driven applications.
    - It stores the Data in local memory on the server and helps avoid extra trips to the DB for retrieving Data that has not changed.
    - It is standard practice to clear any cache data after it has been altered.
    - Overuse of Data Caching can cause memory issues, if data is constantly added and removed to and from cache.
  - Application caching:
    - Utilizes server level caching techniques that cache raw HTML. It can be a page or parts of a page (headers/footers), but it is usually HTML markup.
    - This type of caching can drastically reduce website load time and reduce server overhead.
  - Distributed caching:
    - High volume applications such as Google, YouTube and Amazon use Clustered Distributed Caching
    - Distributed cache is typically made up of a cluster of cheaper machines only serving up memory.
    -  Once the cluster is setup:
      - New machine of memory can be added at any time without disrupting users
      - Allows the web servers to pull and store from distributed server’s memory.
      - Allows the web server to simply serve pages and not have to worry about running out of memory.
    - Few popular systems in use – GemFire, Memcached, Redis, Ehcache, etc

## Distributed cache types:
### Cache-aside:
- Application is responsible for reading and writing from the DB and the cache doesn't interact with the DB at all.
- The cache is *kept aside* as a faster and more scalable in-memory data store.
- The application checks the cache before reading anything from the database and updates the cache after making any updates to the database. Thus the application ensures that the cache is kept synchronized with the database.
- Powerful technique and allows app to issue complex database queries involving joins, nested queries, and manipulate data as required
### Read-through/ Write-through:
- Application treats cache as the main data store and reads data from it and writes data to it
- The cache is responsible for reading and writing this data to the database, thereby relieving the application of this responsibility.
- Read-through/Write-through is ideal for reference data that is meant to be kept in the cache for frequent reads even though this data changes periodically.
- Advantages over cache-aside:
  - Simplification of application code
  - Better read scalability with Read-through
  - Better write performance and DB scalability with Write-behind
  - Auto refresh cache on expiration and data updates
 
## Cache performance:
- Cache hit = If the required data is found in the cache.Super fast.
- Cache miss = If the required data is not found in the cache, this is the way using which the data will be coming inside the cache.
- A good tool of measuring cache performance is : AMAT (Average Memory Access TIme)
- This is how AMAT is calculated
  
    **AMAT= HIT TIME = MISS RATE X MISS PENALTY** (WE NEED TO KEEP THIS TO VERY LOW IN ORDER TO FLAG THE CACHE AS A GOOD CACHE)

    *HIT TIME= * Should be small and fast
    *MISS RATE= * The cache should be large enough to keep more and more data or the cache should be smart enough to remove old data and keep new data inside the cache
    *MISS PENALTY= * This is simply the time required to access the data from the main memory
    
