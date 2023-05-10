# DevOps Monitoring Deep Dive

## Chapter 2 - Creating an Environment
---
### Alertmanager Setup
- Monitoring isn't just gathering metrics
  - What do we do with these metrics
  - Use data to determine when system needs to alert due to error
- Prometheus's Alertmanager sends alerts to endpoints
- Also able to:
  - Group: Notice when alerts are similar and ensure there are not repeated alerts
  - Inhibit: Suppress alerts if related alerts are already firing
  - Silence: Mute alerts on demand
- Default port `9093`

### Grafana Setup
- Visualization and querying front end
- Make data-driven decisions based on metrics from multiple sources
- Default port `3000`

##### Data Source
- An application sending metric data to Grafana
- For us, Prometheus

##### Dashboard
- A view of our metric visualizations
- Contains Tables, groups, information
- Plugins available


## Chapter 3 - Monitoring Basics
---
### Push or Pull

#### Pull
- Monitoring solution does the work
- Scrapes endpoints every x amount of seconds to add data
- Less "live"
- Can run from (almost) anywhere
- Access metrics from web directory from endpoints
- Limited if using complicated networking extensive firewalls

#### Push
- Clients do the work
- Event-based monitoring friendly
- More "live"
- Modular
- Less concern about firewalls


### Patterns and Anti-Patterns


#### Anti-Patterns: Don't Do This

##### Avoid Becomming Tool Obsessed

- Don't try to fit your needs to a tool
- Don't jump from tool to tool
- Don't build what you can buy (or download for free)

##### Don't Follow the Herd

- Just because Google does it doesn't mean we should
- Don't monitor certain metrics because "you're supposed to"

 ##### Don't Ignore Automation
 
 - Don't manually add targets to your monitor system
 - Don't repeat the same troubleshooting tasks again and again
 
 ##### Don't Leave One Person in Charge
 
 - One person shouldn't add monitoring support/access to everything
 - Don't have monitoring as a job

#### Patterns: Do This

#### Find the Right Tool

- Think about your needs and find the right tool
- Add tools when needed (e.g, adding log aggregation to a system with none)
- Write extensions to the extending tools you use when something is lacking

#### Think about your needs

- Ask yourself why you're doing what you're doing
- Monitor for the issue, not the symptom

#### Automate

- Use some form of service discovery
- Review runbooks to see where repeated tasks can be automated

#### Make Monitoring a Team Effort

- Everyone should be monitoring-minded
- Think about how to monitor from the start of a project


## Chapter 4 - Infrastructure Monitoring
---

### Using Node Exporter

##### The Basics

- Currently, the monitoring system only monitors itself
- Use exporters to gather metrics for other servers/services
  - Node Exporter: System metrics for Prometheus (Linux)
    - collectd popular among push solutions
  - WMI Exporter: Sysmte metrics for Windows servers
- An exporter gathers metrics and creates the enpoing that Prometheus can scrape

##### What Gets Monitored

- CPU
- Memory
- Disk I/O
- Disk space and filesystem metrics
- Most metrics enabled by default
  - Select metrics can be added via `--collector.Name` flag on startup

### CPU Metric

- `node_cpu` prefix
- Modes
  - `idle:` Time idle
  - `iowait:` Time waiting for I/O
  - `irq:` Time fixing interrupts
  - `nice:` time spent on process with positive nice value
  - `softirq:` Time fixing interrupts
  - `steals:` if VM, time stolen from CPUs
  - `guests:` if host, time spent for VM CPU cycles
  - `system:` Time in the kernel
  - `user:` time in userland

##### node_cpu_seconds_total
- Counter: Adds time in each mode; always increases
- Use PromQL quesries to extract usefull data
- `rate:` Used with counters to calculate the per-second average change in the given time series in the range.
- `irate:` Same as rate, but for fast-moving counters; differences will be more obvious the larger the time series


### Memory Metric

```bash
cat /proc/meminfo
```

- `node_memory` prefix
- Caches store recently accesssed data
- Buffers store metadata about recently accessed data
  - Buffers and caches can be reclaimed
- Data taken from /proc/meminfo
- Gauges: Snapshots of our system at the moment the metric is taken

##### node_memory_MemTotal_bytes
- All memory on the system
- If you allocate 64GB of memory, this would be around 64,000,000,000

##### node_memory_MemFree_bytes
- Total available memory
- If you have 5GB used, 10GB free, and 10GB in buffers/caches, this would be 10GB

##### node_memory_MemAvailable_bytes
- Total available memory, including buffers and caches
- If you have 5GB used, 10GB Free and 10GB in buffers/caches, this would be 20GB

##### node_memory_Buffers_bytes
- Space being used by buffers

##### node_memory_Cached_bytes
- Space being used by caches


### Disk Metric

iostat

- `node_disk` prefix
- Data taken from /proc/diskstats

##### node_disk_io_now
- Gauge of how many I/O operations are running at a particular point in time

##### node_disk_io_time_seconds_total
- Amount of time spent on all I/O operations

##### node_disk_written_bytes_total
- Amount of bytes written to the disk

##### node_disk_read_time_seconds_total
- Amount of time spent on write operations

##### node_disk_write_time_seconds_total
- Amount of time spent on write operations

##### node_disk_reads_completed_total
- Amount of reads completed

##### node_disk_writes_completed_total
- AMount of writes completed

##### node_disk_reads_merged_total
- AMount of reads merged

##### node_disk_writes_merged_total
- Amount of writes merged


### File System Metric

- `node_filesystem` prefix
- Mounted file system data only

##### node_filesystem_avail_bytes
- How many available bytes in a file system, not including free space reserved for root

##### node_filesystem_device_error
- Simple gauge determining if there's an error
- 0 if no error
- 1 if error

##### node_filesystem_files
- Amount of inodes used

##### node_filesystem_files_free
- Amount of inodes free

##### node_filesystem_free_bytes
- HOw many available bytes in a file system, including free space reserved for root

##### node_filesystem_readonly
- Simple gauge determining if a file system is read-only
- 0 if it is not
- 1 if it is

##### node_filesystem_size_bytes
- The size of the disk

### Networking Metrics

- `node_network` prefix
- Network device data
- For detailed networking data that would benefit networking engineers: Use the SNMP exporter

##### node_network_</sys/class/net/INTERFACE/FILE>
- Gauges for each setting in the /sys/class/net/INTERFACE directory
##### node_network_up
- Gauges if the network is working
##### node_network_transmit*
- Outbound network information, including metrics for bytes, packets FIFO, and more
##### node_network_receive*
- Inbound network information, including metrics for bytes, packets, FIFO and more
##### node_network_transmit_bytes_total
- Outbound network data transmit in bytes
- Use with rate to calculate bandwidth
##### node_network_received_bytes_total
- Inbound network data transmit in bytes
- Use with rate to calculate bandwidth

### Using cAdvisor to Monitor Containers
- Add as a container in a single command
- No additional setup
- MOst metrics have an equivalent node_* metric

##### container_cpu_*
- CPU Metrics
- Based on system versus user
##### container_fs_*
- File system metrics
- node_filesystem_files_free = container_fs_inodes_free
- I/O data also stored here
##### container_memory_*
- Memory metrics
- Usage, failures, caches
##### container_network_*
- Basic networking metrics
- Inbound/outbound
##### container_spec_*
- Additional CPU and memory metrics


## Chapter 5 - Application Monitoring
---

### Counters
- Most common metric type
- Tracks total

##### Importing a Metric
```bash
const METRICNAME = new prom.METRICTYPE({
  name: 'NAME_OF_METRIC',
  help: 'HELP INFORMATION'
  });
```

##### Adding a Counter
- Define the metric name/help info
- Call the metric using the defined variable name
- Increase with inc()
  - Can only increase
  - Increase by n: inc(n)

### Gauges
- Can track anything measurable
- The state of the metric on our system at that point in time

##### Adding a Counter
- Define the metric name/help info
- Call the metric using the defined varaible name
- Increase with inc()
  - Increase by n: inc(n)
- Decrease with dec()
  - Decrease by n: dec(n)

##### Additional Functions
- setToCurrentTime()
  - Use a gauge to track the time
- startTimer()
  - Use gauge timer to track request length


### Summaries and Histrograms
- Uses two time series:
  - `_sum:` Sum of all values passed to Prometheus
  - `_count` Amount of times metric has been taken
- Summaries save data in percentiles
- Histograms save data in defined bucket
- Define the metric name/help info
- Call the metric using the defined variable name
- Metric cannot be called alone
  - Takes a value, not increase/decreae

