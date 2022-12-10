## AWS EC2 
- Service to create and run virtual machines 
- Considerations when launching EC2 instances
    - Name and tag: Tags can manage resource access control, track costs, and automate tasks (FYI, case sensitive)
    - Application and OS image 
    - Instance type and size 
    - Authentication and key pair 
    - Network and security 
    - Storage
    - Placement and tenancy: Where to run EC2 instances? 
    - Scripts and metadata: What to automate upon launch?

## Amazon Machine Image (AMI)
- Provides information required for launching an instance 
- Repeatable, reusable, recoverable 
- **AMIs are created from an existing EC2 instance**
- AMIs can also be bought and shared via the AWS Marketplace and User Community
- An AMI includes:
    - Template for instance volumns (e.g., OS, application server, applications etc.)
    - Launch permissions 
    - Block device mapping 

## EC2 Instance types
- c6g.xlarge:
    - `c` refers to the instance family 
    - `6` refers to the instance generation 
    - `g` refers to additional properties (e.g., g is for Graviton2, an ARM-based processor developed by AWS)
    - `xlarge` refers to the instance size (CPU, memory, storage and network performance)

## EC2 Instance families
- General Purpose: Balance compute, memory and networking = Good for a diverse range of workloads (e.g., web applications)
- Compute Optimized: Compute-bound applications (e.g., Scientific modeling, machine learning etc.)
    - Compute-bound means that the time it takes for a task to complete is determined principally on the central processor speed 
- Memory Optimized: Fast delivery of large datasets (e.g., cache, database server, data analytics workload)
- Accelerated Computing: GPU-bound applications 
    - CPU vs. GPU: GPU has an enhanced mathematical computation capability whereas CPU is a general processor that is designed to handle a wide variety of tasks 
- Storage Optimized: High sequential read/writes on large datasets (e.g., NoSQL, OpenSearch)

**Note: New generation generally increase compute capabilities and reduce processing costs associated with running the instance = Lower costs**

## AWS Compute Optimizer
-   Analyze using ML the current configuration and usage data from CloudWatch to recommend the most optimized compute configurations 
- 