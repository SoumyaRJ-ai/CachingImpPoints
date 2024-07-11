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
  
    **AMAT= HIT TIME + MISS RATE X MISS PENALTY** (WE NEED TO KEEP THIS TO VERY LOW IN ORDER TO FLAG THE CACHE AS A GOOD CACHE)

    *HIT TIME= * Should be small and fast
    *MISS RATE= * The cache should be large enough to keep more and more data or the cache should be smart enough to remove old data and keep new data inside the cache
    *MISS PENALTY= * This is simply the time required to access the data from the main memory
    
- HIT TIME = MISS TIME + MISS PENALTY
- For designing an ideal cache, the HIT time should be much lesser than the MISS time and the MISS time should be greater than or equal to the MISS penalty
- HIT RATE = 1 -  MISS RATE
- For a well-designed cache, we need to have:
  - HIT RATE > MISS RATE
  - HIT RATE IS ALMOST 1
- In Netflix, caches play a major role throughout the customer experience, right from playing a movie, providing a high-volume, low-latency, globally available data layer (EVCache) that backs Netflix’s stateless services.

## Cache locality
- Defines that when ever the cache is hit at some address, the function is going to grab the near by location of cache hit as well because it can happen that the next ask comes to these neighbouring addresses. 
- There are 2 types of cache locality:
  - **temporal:** If we access one address it's likely that we are going to access that address again. (Very often access leads to Temporal locality)
  - **spatial:** If we access one address, it's likely that we are going to access the nearby addresses as well. (Address of the next available items likely to be accessed)
- Example of locality:
  - Let's say we are taking an example of Library which is large and but slow to access.
    - TEMPORAL locality : Look for the definition of same LOCALITY more often
    - SPATIAL locality : Look for a computer architecture definition in the book and you are likely to see other computer architecture  definition as well.

## Cache Organization
**The modern processors usually have several caches not just one. So now we are going to talk about the L1 caches.**
- **L1 cache**  
  - Directly service the read and write request from the processor.
  - If this is a hit then its okay, else if it is a miss then things get complicated because before we go to the main memory, we go to other caches.
  - In recent processors the typical size of L1 caches have been 16 KBs - 64 KBs
  - Large enough in size to get about 90% hit rate but still small enough to HIT in 1-3 processor cycle. So, we spend very few processor cycles waiting for the data to comeback from this cache if it's a hit.

**There are basically 2 things we need to answer while managing the cache**
- *How to determine HIT or MISS?*
  - We can represent our cache as a table which we can index with some bits of address of the data and it tells us what we have in the cache and if we do have what we are looking for, then it gives us the data.
  - Some part of the cache entry should tell us about the cache hit and how much data we have in the entry is called the Block size or Line size. And they tell us the size of the each entry.
  - The size of the block is determined by keeping in mind that we need to have the Spatial locality in the block as well. So, the block size should be atleast as large as the largest single access that we can do in the cache and hopefully a bit larger than that because when we do a load work or store work, we should get our data at the same location. Again we need to have spatial locality beacuse we need to know the next locations as well from where we can access the next data as well.
  - So typically the block size should be around 32 to 128 bytes work well both from the prospective of larger than a typical access and also they capture most of the spatial locality that exists in the program.
  - Including SPATIAL locality helps in bringing in the near by addresses of each variable so that we won't be provisioning an entire block to a single variable. If there is no SPATIAL locality and only TEMPORAL locality, in order to get the high rate of HIT we need to keep the entire block of the variable inside the cache which significantly decreases the amount of blocks you can have inside your cache.  
*- How to decide which one to kick out of the cache*


  
