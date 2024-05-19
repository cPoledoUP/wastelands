# Business Continuity
- process that prepares for, responds to, and recovers from system outage that can adversely affect business operations
## Information Availability
- ability of an IT infrastructure to function according to business expectations, during its specified time of operation
- can be defined with the help of:
	1. **Accessibility** - information should be accessible to the right user when required
	2. **Reliability** - information should be reliable and correct in all aspects
	3. **Timeliness** - defines the time window during which information must be accessible
## Causes of Information Unavailability
1. **Disaster (<1%)**
	- Natural or man made
		- Flood
		- Fire
		- Earthquake
2. **Unplanned Outages (20%)**
	- Failure
		- Database corruption
		- Component (physical/virtual) failure
		- Human error
3. **Planned Outages (80%)**
	- Competing workloads
		- Backup, reporting
		- Data warehouse extracts
		- Application and data restore
## Impact of Downtime
1. **Lost productivity**
	- Number of employees impacted x hours out x hourly rate
2. **Lost revenue**
	- Direct loss
	- Compensatory payments
	- Lost future revenue
	- Billing losses
	- Investment losses
3. **Damaged reputation**
	- customers
	- suppliers
	- financial markets
	- banks
	- business partners
4. **Financial performance**
	- revenue recognition
	- cash flow
	- lost discounts (A/P)
	- payment guarantees
	- credit rating
	- stock price
5. **Other expenses**
	- temporary employees
	- equipment rental
	- overtime costs
	- extra shipping costs
	- travel expenses
## Measuring Information Availability
- **MTBF (Mean Time Between Failures)**
	- average time available for a system or component to perform its normal operations between failures
	- MTBF = total uptime / number of failures
- **MTTR (Mean Time To Repair)**
	- average time required to repair a failed component
	- includes total time to do:
		- detect the fault
		- mobilize the maintenance team
		- diagnose the fault
		- obtain the spare parts
		- repair, test, and restore the data
	- MTTR = total downtime / number of failures
- **IA (Information Availability)**
	- IA = MTBF / (MTBF + MTTR)
	- IA = uptime / (uptime + downtime)
## BC Terminologies
1. **Disaster recovery**
	- coordinated process of restoring systems, data, and the infrastructure required to support ongoing business operations after a disaster occurs
	- after all recovery efforts are completed, data is validated to ensure that it is correct
2. **Disaster restart**
	- process of restarting business operations with mirrored consistent copies of data and applications
3. **Recovery-Point Objective (RPO)**
	- point-in-time to which systems and data must be recovered after an outage
	- amount of data loss that a business can endure
4. **Recovery-Time Objective (RTO)**
	- time within which systems and applications must be recovered after an outage
	- amount of downtime that a business can endure and survive
## BC Technology Solutions
1. **(Resolving) Single Points of Failure**
	- failure of a component that can terminate the availability of the entire system
	- **Resolving: through redundancy**
2. **Multipathing Software**
	- recognizes and utilizes alternate I/O path to data
	- provides **load balancing** by distributing I/Os to all available active paths
	- intelligently manages the paths to a device by sending I/O down to the optimal path
3. **Backup and Replication**
	- both local and remote
# Backup and Archive
## Backup
- additional copy of production data that is created for the sole purpose of **recovering lost or corrupted data**
- serve three purposes:
	1. **Disaster recovery**
	2. **Operational recovery**
	3. **Archive**
## Backup Granularity
1. **Full backup**
	- backup of the complete data on the production volumes
2. **Incremental backup**
	- copies the data that has changed since last full or incremental backup, whichever occurred more recently
	- less number of files to backup leads to less time to backup
	- last full and subsequent backups need to be applied leads to longer restore
3. **Cumulative backup**
	- copies the data that has changed since the last full backup
	- longer than incremental (because more files to backup) but faster to restore
## Backup Architecture
1. **Backup client**
	- gathers data that is to be backup up and send it to storage node
2. **Backup server**
	- manages backup operations and maintains backup catalog
3. **Storage node**
	- writes data to backup device
## Backup Operation
1. Backup server Initiates scheduled backup process
2. Backup server retrieves backup-related information from the backup catalog
3. a. Backup server instructs storage node to load backup media in backup device
3. b. Backup server instructs backup clients to send data to be backup up to storage node
4. Backup clients send data to storage node and update the backup catalog on the backup server
5. Storage node sends data to backup device
6. Storage node sends metadata and media information to backup server
7. Backup server updates the backup catalog
## Recovery Operation
1. Backup client requests backup server for data restore
2. Backup server scans backup catalog to identify data to be restored and the client that will receive data
3. Backup server instructs storage node to load backup media in backup device
4. Data is then read and send to backup client
5. Storage node sends restore metadata to backup server
6. Backup server updates the backup catalog
## Data Deduplication
- process of identifying and eliminating redundant data
## Deduplication Methods
1. **File level**
	- detects and removes redundant copies of identical files
	- the subsequent copies are replaced with a pointer that points to the original file
2. **Subfile level**
	- breaks the file into smaller chunks and then uses specialized algorithm to detect redundant data within and across the file
## Deduplication Implementations
1. **Source-based**
	- data is deduplicated at the source
	- increased overhead on the backup client
	- reduced storage capacity and network bandwidth requirements
2. **Target-based**
	- data is deduplicated at the target
	- offloads the backup client from deduplication process
	- all backup data traverse the network
## Backup in Virtualized Environment
1. **Traditional**
	- Backup agent on a VM
		- requires installing backup agent on each running VM
		- can only backup virtual disk data
	- Backup agent on a hypervisor
		- requires installing backup agent only on hypervisor
		- backs up all the VM files
2. **Image-based**
	- takes a snapshot of the VM
	- creates a copy of the guest OS and all the data associated with it
	- the backup is saved as a single file called an **image** and this image is mounted on a proxy server
# Local Replication
## Replication
- create an exact copy (**replica**) of data
- two classifications:
	1. **local replication** - replicating data within the same data center/array
	2. **remote replication** - replicating data at a remote site
## Uses of Local Replication
1. **Alternate source for backup**
2. **Fast recovery**
3. **Decision support activities**
4. **Testing platform**
5. **Data migration**
## Replica Characteristics
1. **Recoverability/Restartability**
	- replica should be able to restore data on the source device
	- restart business operation from replica
2. **Consistency**
	- replica must be consistent with the source
3. **Choice of replica tie back into Recovery Point Objective (RPO)**
	- **Point-in-Time (PIT)**
		- non-zero RPO
		- time failure - PIT
	- **Continuous replica**
		- zero RPO
## Local Replication Technologies
1. **Full-volume mirroring**
	- provides full copy of source data on target
	- target device is accessible after the target is detached from the source
	- PIT of a replica is determined by the time when the target is detached from the source
2. **Full-volume replication**
	- also provides full copy of source data on target
	- target device is immediately accessible by the BC host after the replication session is activated
	- PIT is determined by time of session activation
	- target needs to be at least the same size from source
3. **Pointer-based virtual replication**
	- target do not hold data, holds pointers to the data instead
	- target devices are accessible immediately when the session is started
	- recommended if the changes from the source are typically **less than 30%**
	- target can be a fraction of size from source
4. **Network-based local replication**
	- replication occurs at the network layer between hosts and storage array
	- ideal for **highly heterogenous environment**
	- typically provides the ability to restore data to any previous point in time
	- data changes are continuously captured and stored in a separate location from the production data
## Local Replication in Virtualized Environment
1. **VM Snapshot**
	- captures the state and data of a running VM at a specified PIT
	- uses a separate delta file to record all the changes to the virtual disk since the snapshot session is activated
	- restores all settings configured in a guest OS to the PIT
	- includes:
		- **VM files**
			- BIOS
		- **network configuration**
		- **power state**
			- powered-on
			- powered-off
			- suspended
2. **VM Clone**
	- identical copy of an existing VM
	- clone becomes a separate VM from its parent
	- close has own MAC address
	- useful when there is a need to deploy many identical VMs
# Remote Replication
- process of creating replicas at remote sites
- addresses risk associated with **regionally driven outages**
## Modes
1. **Synchronous**
	- a write is committed to both source and remote replica before it is acknowledged to the host
	- ensures source and replica have identical data at all times
	- provides **near-zero RPO**
	- response time depends on the bandwidth and distance
	- requires bandwidth more than the **maximum** write workload
	- typically deployed for distance **<200 km**
2. **Asynchronous**
	- write is committed to the source and immediately acknowledged to the host
	- data is buffered at the source and transmitted to the remote site later
	- data at the remote site will be behind the source by at least the size of the buffer
		- **non-zero RPO**
	- requires bandwidth equal to or greater than **average** write workload
	- sufficient buffer capacity should be provisioned
	- RPO depends on size of buffer and available network bandwidth
	- can be deployed over long distances
		- high response time because the writes are acknowledged immediately to the source host
## Technologies
1. **Host-based**
	- performed by host-based software
	- 2 approaches
		- **Local Volume Manager (LVM)-based replication**
			- writes to the source volumes are transmitted to the remote host by the LVM
		- **Log shipping**
			- commonly used in database environment
			- transactions to the source database are captured in logs, which are periodically transmitted by the source host to the remote host
2. **Storage Array-based**
	- performed by array-operating environment
	- 3 methods
		- **synchronous**
			- write is committed to both source and replica before it is acknowledged to host
		- **asynchronous**
			- writes are committed to source and immediately acknowledged to host
		- **disk buffered**
			- combination of local and remote replication technologies
			- a consisted PIT local replica of the source device is first created, then replicated to a remote replica on the target array
3. **Network-based (Continuous Data Protection (CDP))**
	- occurs at the network layer between the host and storage array
	- provides any-point-in-time recovery capability
	- CDP appliance must be present in both source and remote sites
## Migration in Virtualized Environment
- remote mirroring of virtual volume
	- virtual volumes assigned to hosts are mirrored to two different sites
- **VM migration**
	- moving VMs from one location to another without powering VMs off
	- 2 techniques
		1. **Hypervisor-to-hypervisor**
			- copies contents of virtual machine memory from the source hypervisor to the target
			- requires source and target hypervisor access to the same storage
		2. **Array-to-Array**
			- VM file are moved from source array to remote array
			- can move VMs across dissimilar storage arrays
# Cloud Computing Technologies, Characteristics, Benefits, Models, and Challenges
## Cloud Computing
- model for enabling ubiquitous, convenient, on-demand network access to a shared pool of configurable computing resources that can be rapidly provisioned and released with minimal management effort or service provider interaction
## Drivers for Cloud Computing
- business requirements
	- transformation of IT processes to achieve more with less
	- better agility and higher availability at reduced expenditure
	- reduced time-to-market
	- refreshing technology quickly
	- faster provisioning of IT resources - all at reduced cost
## Cloud Computing Essential Characteristics
According to National Institute of Standards and Technology (NITS)
1. **On-Demand Self-Service**
	- enables consumers to unilaterally provision computing capabilities as needed automatically
	- consumers view service catalogue via web-based ui and use it to request for a service
2. **Broad Network Access**
	- Computing capability are available over the network
	- accessed using broad range of clients
3. **Resource Pooling**
	- provider's computing resources are pooled to serve multiple consumers
	- consumers have no knowledge over the exact location of the provided resources
4. **Rapid Elasticity**
	- computing capabilities can be scaled rapidly, commensurate with consumer's demand (sense of unlimited scalability)
5. **Measured Service**
	- provides metering system that continuously monitors resource consumption
	- helps generate billing and chargeback reports
## Benefits of Cloud Computing
1. **Reduced IT cost**
	- reduces the up-front capital expenditure
2. **Business agility**
	- provides the ability to deploy new resources quickly
	- enables businesses to reduce time-to-market
3. **Flexible scaling**
	- enables consumers to scale up, scale down, scale out, or scale in the demand for computing resources easily
4. **High availability**
	- ensures resource availability at varying levels, depending on consumer's policy and priority
## Cloud Service Model
1. **Infrastructure-as-a-Service (IaaS)**
	- consumers deploy their software, including **OS and application** in IaaS provider
	- computing resources such as processing power, memory, storage, and networking components are offered as a service
2. **Platform-as-a-Service (PaaS)**
	- consumers deploy consumer-created or acquired **applications** in PaaS provider
	- consumer has control over deployed applications
3. **Software-as-a-Service (SaaS)**
	- consumers use provider's applications running on the cloud infrastructure
	- service providers exclusively manage computing infrastructure and software to support services
## Cloud Deployment Model
1. **Public**
	- cloud infrastructure is provisioned for open use by the general public
	- consumers use the cloud services offered by the providers via the internet and pay metered usage charges or subscription fees
	- e.g., amazon, google
2. **Private**
	- provisioned for exclusive use by a single organization comprising multiple consumers
3. **Community**
	- provisioned for exclusive use by a specific community of consumers from organizations that have shared concerns
4. **Hybrid**
	- composition of two or more distinct cloud infrastructures that remain unique entities, but are bound together by standardized or proprietary technology
## Cloud Challenges
- Consumer's Perspective
	1. **Security and Regulation**
		- if data moves to a cloud model other than an on-premise private cloud, consumers could lose absolute control of their sensitive data
	2. **Network latency**
		- realtime apps may suffer due to network latency and limited bandwidth
	3. **Supportability**
		- service provider might not support proprietary environment
	4. **Vendor lock-in**
		- restrict consumers from changing their cloud service provider
- Provider's perspective
	1. **Service warranty and service cost**
		- resources must be kept ready to meet unpredictable demands
		- hefty penalty if requirements are not fulfilled
	2. **Complexity in deploying vendor software in the cloud**
		- many vendors do not provide cloud-ready software licenses
		- higher cost of cloud-ready software licenses
	3. **No standard cloud access interface**
		- cloud consumers want open APIs
## Best Deployment Model
1. **Individual - Public cloud**
	- convenience outweighs risk
	- low cost or free
2. **Startup - Public cloud**
	- convenience outweighs risk
3. **SMB - Hybrid cloud**
	- Tier 1 apps: private cloud
	- Tier 2-4 apps (backup, archive, testing): public cloud
4. **Enterprise - Private cloud**
	- Tier 2-4: private cloud
	- Tier 1: may continue to run in a traditional data center environment
## Financial Advantage
- Cost of Owning Infrastructure
	- CAPEX (capital expenditure)
		- servers
		- storage
		- OS
		- etc.
	- OPEX (operating expenditure)
		- power and cooling
		- personnel
		- bandwidth
		- etc.
- Cloud Adoption Cost
	- OPEX
		- migration
		- compliance and security
		- subscription fee