# Load balancing in Layer 4 vs Layer 7

#### Layer 4

##### works via TCP

> Pros
- Simple Load Balancing: Only looks for the ip and port and does not need to look the data
- Efficient: No data lookup
- More secured than Layer 7
- One TCP connection
- Uses NAT

> Cons
- No smart load balancing: 
- N/A Microservices
- Sticky per segment
- No caching: since it cannot look at the data there is no way to cache data inside


#### Layer 7

#####  works via HTTP

> Pros
- Smart load balancing: because it can read data and decide what to do with it
- Caching
- Great for microservices
- Can change stickyness

> Cons
- Expensive: looks at data
- Less secure compared to Layer 4
- Decrypts data which could potentially a source of leak when hacked
- Two TCP connections: slowwwww
- Must share TLC certificate

#### Kinds of Proxy Server

- Nginx 
- HA Proxy