# SAN
## Definition
- Storage Area Network
- high-speed, dedicated network of servers and shared storage devices
- enables organizations to connect geographically dispersed servers and storage
# FC SAN
## Definition
- uses FC protocol for communication
## Components
1. Node (server and storage) ports
	- a **node** is a source or destination of information
	- provide a physical interface for communicating with other nodes
	- has transmit (Tx) and receive (Rx) link
2. Cables
	- **Copper cable** for short distances (<30m)
	- **Optical fiber cable** for long distances
		- **Single mode**: single beam of light, up to 10km
		- **Multimode**: multiple beams, shorter distance
3. Connectors
	- attached at the end of a cable for easy (dis)connection of cables to/from ports
	- **Standard Connector**, **Lucent Connector**: duplex
	- **Straight Tip Connector**: simplex
4. Interconnecting Devices
	- **Hubs**: data travels through all connection points
	- **Switches**: route data from one port to another
	- **Directors**: high-end switches with higher port count and fault tolerance
5. SAN Management Software
	- tools to manage interfaces between SAN hosts and storage arrays (GUI or CLI)
## Connectivity
1. Point-to-Point
	- two devices connected directly
	- used in DAS
2. Fibre Chanel Arbitrated Loop
	- shared loop  to attached nodes
	- implemented using **ring or star topology**
	- one device can perform I/O operation at a time
	- max 126 nodes
3. Fibre Channel Switched Fabric (FC-SW)
	- FC-SW network provides dedicated data path and scalability
	- **fabric**: logical space in which all nodes communicate with one another in a network
	- **Interswitch Links (ISL)** enables switches to be connected together
## Switched Fabric Ports
1. N_Port (Node Port)
	- end port in the fabric (usually HBA or storage array port)
2. E_Port (Expansion Port)
	- connect two FC switches through ISLs
3. F_Port (Fabric Port)
	- port on a switch that connects an N_Port
4. G_Port (Generic Port)
	- can operate as E_Port or F_Port on  switch
## Fibre Channel Architecture
### Fibre Channel Protocol Stack
1. FC-4 Layer
	- **mapping interface**
	- defines the application interfaces and the way Upper Layer Protocols (ULPs) are mapped to the lower FC layers
	- **SCSI, HIPPI, ESCON, ATM, and IP** can operate on this layer
2. FC-2 Layer
	- **routing, flow control**
	- provides Fibre Channel addressing, structure, and organization of data
3. FC-1 Layer
	- defines how data is **encoded/decoded**
4. FC-0 Layer
	- defines the **physical** interface, media, and transmission of bits
	- specifications include cables, connectors, etc.
### Fibre Channel Addressing
- format: Domain ID (23-16) | Area ID (15-08) | Port ID (07-00)
- Domain ID is the unique number provided to each switch in the fabric (239 avail for 8 bit)
- assigned to nodes
- dynamically assigned
### World Wide Names
- 64-bit unique identifier assigned to each device
- statically assigned
- format:
	- Array: Format Type | Company ID (24bit) | Port | Model Seed (32bit)
	- HBA: Format Type | Reserved (12bit) | Company ID (24bit) | Company Specific (24bit)
### Structure and Organization of FC Data
1. Exchange
	- enables two N_Ports to identify and manage a set of information units
2. Sequence
	- contiguous set of frames that correspond to an information unit
3. Frame
	- fundamental unit of data transfer
	- 5 parts
		- SOF and EOF: delimiters (4 bytes)
		- Frame header: contains addressing information (24 bytes)
		- Data field: contains data payload (up to 2112 bytes)
		- Cyclic Redundancy Check (CRC): checksum for error detection (4 bytes)
## Fabric Services
1. Fabric Login Server
	- used during the initial part of the node's fabric login process
2. Name Server (Distributed Name Server)
	- responsible for name registration and management of node ports
3. Fabric Controller
	- provides services to both node ports and other switches
	- manage Registered State Change Notifications (RSCN)
	- SCNs keep the name server up-to-date on all switches
4. Management Server
	- enables the FC SAN management software to retrieve information and administer the fabric
## Switched Fabric Login Types
1. Fabric Login (FLOGI)
	- performed between N_Port and F_Port
	- after N_Port has logged in, it can query the name server database for information about all other logged in ports
1. Port Login (PLOGI)
	- occurs between two N_Ports to echange parameters relevant to the session
2. Process Login (PRLI)
	- occurs between two N_Ports to exchange ULP related parameters
## Zoning
- enables node ports to be logically segmented into groups
- takes place in fabric level compared to LUN masking which is at the array level
- RSCN is sent to all nodes impacted when change happen in name server database
- all nodes will be impacted if zoning is not configured
1. Port Zoning
	- uses the physical address of switch ports
	- if a node is moved, zoning must be modified
2. WWN Zoning
	- uses World Wide Names to define zones
	- allows nodes to be moved to another switch port in the fabric and maintain connectivity
3. Mixed Zoning
	- combines Port an WWN Zoning
## FC SAN Topologies
### Mesh Topology
1. Full Mesh
	- each switch is connected to all switches
2. Partial Mesh
	- not all switch is connected to other switches
	- more scalability
### Core-Edge Fabric
- consist of Edge and Core Switch Tiers
- Network traffic traverses or terminates at Core Tier
- Storage is connected at Core Tier
- High availability, Medium scalability, Medium to maximum connectivity
## Virtualization in SAN
1. Block-level Storage Virtualization
	- abstracts block storage devices and creates a storage pool by aggregating LUNs
	- virtualization layer maps virtual volumes to LUNs
1. Virtual SAN (VSAN)
	- logical fabric on an FC SAN
	- enables communication among a group of nodes regardless of their physical location in the fabric
# IP SAN
## Definition
- technology of transporting block I/Os over an IP (Internet Protocol)
- more economical as there is no need to by FC components and organizations usually have an existing IP-based infrastructure
- two primary protocols are Internet SCSI (iSCSI) and Fiber Channel over IP (FCIP)
# iSCSI
## Definition
- IP based protocol
- encapsulates SCSI commands and data into an IP packet and transports them using TCP/IP
## Components
1. Initiator (host)
2. Target (storage or iSCSI gateway)
	- if an iSCSI-capable storage array is deployed, then a host with iSCSI initiator can directly communicate with the storage array over an IP network
	- in an implementation that uses FC array for iSCSI communication, iSCSI gateway is used
3. IP-based network
## iSCSI Host Connectivity
1. Standard NIC with software iSCSI initiator
	- NIC provides network interface
	- software initiator provides iSCSI functionality
	- requires host CPU cycles for iSCSI and TCP/IP processing
2. TCP Offload Engine (TOE) NIC with software iSCSI initiator
	- moves TCP processing load off the host CPU onto the NIC card
	- software initiator provides iSCSI functionality
	- requires host CPU cycles for iSCSI processing
3. iSCSI HBA
	- offloads both iSCSI and TCP/IP processing from host CPU
	- simplest option for boot from SAN
## iSCSI Topologies
1. Native iSCSI Connectivity
	- iSCSI initiators are directly attached to storage array or connected through IP network
	- storage array has iSCSI port
	- each iSCSI port is configured with an IP address
2. Bridged iSCSI Connectivity
	- iSCSI gateway is used to enable communication between iSCSI host and FC storage
	- converts IP packets to FC frames and vice versa
	- iSCSI initiator is configured with gateway's IP address as its target
	- iSCSI gateway is configured as FC initiator to storage array
3. Combining FC and Native iSCSI Connectivity
	- most common topology
	- enable iSCSI and FC connectivity in the same environment
	- no bridge device needed
## iSCSI Protocol Stack
1. Layer 7 Application: SCSI - commands and data
2. Layer 5 Session: iSCSI - login and discovery
3. Layer 4 Transport: TCP - windows and segments
4. Layer 3 Network: IP - packets
5. Layer 2 Data Link: Ethernet - frames
## iSCSI Discovery
- initiator must discover location and name of target on a network
- two ways:
	- SendTargets discovery
		- initiator is manualy configured with the target's network portal
	- Internet Storage Name Service (iSNS)
		- initiators and targets register themselves with iSNS server
		- initiator can query iSNS for targets
## iSCSI Names
- unique identifier to identify initiators and targets
- two types
	- iqn (iSCSI Qualified Name)
		- needs a registered domain name
		- date is included in the name
	- eui (Extended Unique Identifier)
		- globally unique identifier
		- conposed of eui prefix followed by a 16-character hexadecimal name
# FCIP
## Definition
- FC SAN is for localized data movement
- FCIP is for long distance at multiple geographic locations
- IP-based protocol used to connect distributed FC SAN islands
- creates virtual FC links over existing IP network
## FCIP Protocol Stack
1. Application
2. iSCSI Commands, Data, and Status
3. FCP (SCSI over FC)
4. FCIP
5. TCP
6. IP
7. Physical Media
- Layers 7-5 is FC Frame
- Layers 4-2 is FC to IP Encapsulation
## FCIP Topology
- FCIP gateway is connected to each fabric via a standard FC connection
- an IP address is assigned to the port on the gateway, which is connected to an IP network
# FCoE
## Definition
- Fibre Channel over Ethernet
- provides consolidation of LAN and SAN traffic over a single physical interface infrastructure
- protocol that transports FC data over Ethernet network
- before FCoE, environment is complex as storage resources are accessed through HBAs and IP network resources are accessed using NICs
- with FCoE, Converged Network Adapters (CNA) replaces both HBAs and NICs
## Components
1. Converged Network Adapter (CNA)
	- provides functionality of both standard NIC and FC HBA
	- reduces the required number of server slots and switch ports
2. Cables
	- Copper based Twinax
		- shorter distance (10 meters)
		- less power and less expensive
		- use Small Form Factor Pluggable Plus (SFP+) connector
	- Standard fiber optical cables
		- longer distance
		- relatively more expensive
		- use SFP+ connector
3. FCoE switches
	- provides both ethernet and FC switch functionality
	- consists of Fibre Channel Forwarder (FCF), Ethernet Bridge, and a set of ethernet ports
	- forwards frames based on Ethertype
## Frame Structure - FCoE Frame Mapping
### FC Protocol Stack
1. FC - 4 Protocol map
2. FC - 3 Services
3. FC - 2 Framing
4. FC - 1 Data enc/dec
5. FC - 0 Physical
### OSI Stack
1. 7 - Application
2. 6 - Presentation
3. 5 - Session
4. 4 - Transport
5. 3 - Network
6. 2 - Data Link
7. 1 - Physical
### FCoE Stack
1. FC - 4
2. FC - 3
3. FC - 2
4. FCoE Mapping
5. 2 - MAC
6. 1 - Physical
- it basically replaced FC-0 and FC-1 layers of the FC stack with Ethernet
## FCoE Enabling Technologies
### Conventional Ethernet
- lossy in nature
- frames might be dropped or lost during transmission
### Converged Enhanced Ethernet (CEE)
- provides lossless Ethernet
- has the following functionalities:
	- Priority-based flow control (PFC)
		- creates eight virtual links on a single physical link
		- uses PAUSE capability of ethernet for each virtual link
			- mechanism is based on user priorities or classes of service
	- Enhanced transmission selection (ETS)
		- allocates bandwidth to different traffic classes (LAN, SAN, Inter Process Communication (IPC))
		- provides available bandwidth to other classes of traffic when a particular class of traffic does not use its allocated bandwidth
	- Congestion notification (CN)
		- provides mechanism for detecting congestion and notifying the source
		- enables a switch to send a signal to other ports that need to stop or slow down their transmissions
	- Data center bridging exchange protocol (DCBX)
		- enables CEE device to convey and configure their features with other CEE devices in the network
		- ensures consistent configuration across network
# File Sharing Environment
## Definition
- enables users to share files with other users
- user who creates the file (owner) determines the type of access (R, W, execute, append, delete) to be given to other users
- Examples:
	- File Transfer Protocol (FTP)
	- Distributed File System (DFS)
	- Network File System (NFS)
	- Peer-to-peer (P2P)
# NAS
## Definition
- Network Attached Storage (NAS)
- dedicated, high-performance file sharing and storage device
- enables both UNIX and Microsoft Windows users to share the same data
- a NAS device uses its own operating system
## General-Purpose Servers vs NAS Device
### General Purpose Servers (Windows or UNIX)
1. Applications
2. Print Drivers
3. File System
4. Operating System
5. Network Interface
### Single Purpose NAS Device
1. File System
2. Operating System
3. Network Interface
## Benefits of NAS
1. Improved efficiency
	- NAS performs better because it has specialized OS
2. Improved flexibility
	- compatible with both UNIX and Windows platforms
3. Centralized storage
	- centralized data minimize data duplication
4. Simplified management
	- provides a centralized console that makes it possible to manage file systems efficiently
5. Scalability
	- scales well with different utilization profiles because of the high-performance and low-latency design
6. Security
	- ensures security, user auth, and file locking with industry-standard schemas
7. Low cost
	- uses commonly available and inexpensive Ethernet components
8. Ease of deployment
	- configuration at the client is minimal
	- built-in software
## Components of NAS
1. NAS head
	- network interface card (NIC)
	- protocols for file sharing
	- optimized OS
	- CPU and Memory
	- Storage interface and ports
2. Storage
## NAS Implementation
1. Unified NAS
	- consolidates NAS-based (file-level) and SAN-based (block-level) data access
	- supports both CIFS and NFS protocols for file access and iSCSI and FC protocols for block level access
	- reduce and organization's infrastructure and management costs
2. Gateway NAS
	- consists of 1+ NAS heads and uses external and independently managed storage
	- management functions are more complex (separate admin tasks for NAS head and storage)
	- more scalable (NAS heads and storage arrays can be independently scaled up)
3. Scale-out NAS
	- pools multiple nodes together in a cluster
		- node may consist of either NAS head or storage or both
	- cluster performs the NAS operation as a single entity
	- better aggregate performance and availability, ease of use, low cost, and theoretically unlimited scalability
## NAS File-Sharing Protocols
1. Common Internet File System (CIFS)
	- client-server application protocol
	- enables client programs to make requests for files and services on remote computers over TCP/IP
	- stateful protocol
		- maintains connection information regarding every connected client
		- can automatically restore connections and reopen files that were open prior to interruption
2. Network File System (NFS)
	- client-server application protocol
	- based on User Datagram Protocol (UDP)
	- 3 versions
		- NFSv2
			- uses UDP to proved stateless network connection
			- features such as locking are handled outside the protocol
		- NFSv3
			- commonly used
			- uses UDP or TCP
			- features include 64-bit file size, asynchronous writes, and additional file attributes to reduce refetching
		- NFSv4
			- uses TCP
			- offers enhanced security
## File-Level Virtualization
- uses virtualization appliance
- break dependencies between client access and file location
- optimize storage utilization
- migrations does to disrupt or cause downtime
# Object-Based Storage
## Description
- more than 90% of data is unstructured
- traditional NAS for storing unstructured data became inefficient
- NAS manages large amounts of metadata which is stored as part of the file, which adds to the complexity and latency in searching and retrieving files
- Object-based storage is a way to store data in the form of objects based on its content and other attributes rather than the metadata
## Hierarchical File Systems vs Flat Address Space
- Hierarchical file system organizes data in files and directories
- Object-based Storage Device (OSD) store data in the form of objects
	- uses flat address space that enables storage of large number of objects
	- object contains data, metadata, and other attributes
	- each object has a unique objectID, generated using specialized algorithms
## Components of OSD
1. Nodes
	- server that runs the OSD operating environment
	- provides 2 services to store, retrieve, and manage data
		- metadata service: generate and map objectID
		- storage service: manage set of disks where user data is stored
2. Internal network
	- provides node-to-node connectivity and node-to-storage connectivity
3. Storage
	- low-cost and high-density disk drives
## Process of Storing Objects in OSD
1. Application server sends a file to OSD
2. OSD node divides the file into two parts, user data and metadata
3. OSD node generates object ID from the user data
4. OSD stores the metadata and object ID using the metadata service
5. OSD stores user data (object) using the storage service
6. Acknowledgement sent to the application server
## Process of Retrieving Objects in OSD
1. Application server request file from OSD
2. Metadata service locates the object ID for the requested file
3. Metadata service sends the object ID to the application server
4. Application service sends the object ID to the OSD storage service for object retrieval
5. OSD storage service retrieves the object from the storage device
6. Storage service sends the file to the application server
## Key Benefits of OSD
1. Security and Reliability
	- unique object ID ensures data integrity and content authenticity
	- request authentication is performed at storage device
2. Platform Independence
	- enables sharing of objects across heterogeneous platforms
	- suitable for cloud computing environment (either through HTTP/S (REST, SOAP), NFS, or CIFS)
3. Scalability
	- both OSD nodes and storage can be independently scaled
4. Manageability
	- have inherent intelligence to manage objects
	- have self-healing capability
	- policy based management capability enables OSD to handle routing jobs automatically
# Unified Storage
## Description
- deploying disparate storage solutions (SAN, NAS, OSD) adds management cost, complexity, and environmental overhead
- unified storage consolidates block, file, and object-based access with one unified platform
	- supports multiple protocols for data access
	- can be managed through single management interface
## Components of Unified Storage
1. Storage Controller
	- manages back-end storage pool
	- configures LUNs and presents them to application servers, NAS heads, and OSD nodes
2. NAS head
	- dedicated file server that provides file access to NAS clients
3. OSD node
	- accesses the storage through the storage controller
	- LUNs assigned to the OSD node appear as physical disks
4. Storage