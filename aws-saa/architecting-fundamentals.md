## Data centers 
- Uses proprietary AWS network equipment 
- Organized into Availability Zones 

## Availability Zones 
- Data centers in a region 
- Interconnected by high speed private links
- Deploy across AZs to achieve **fault tolerance**
- Organized into Regions 

## Region
- Consists of multiple isolated and physically separated AZs
- Independent of other regions so resources are not automatically replicated (unlike with AZs)

## Factors for Region Selection 
1. Governance and legal requirements: Data sovereignty (e.g., China)
2. Latency 
3. Service availability: Not all services are available in all regions 
4. Cost: Different regions have different costs 

## AWS Local Zones
1. Extends an AWS Region to provide single-digit milisecond latency to end users
1. Use cases: Video streaming, real-time multiplayer streaming 

## Edge Locations (Points of Presence)
1. Nearest point to a requester of an AWS Service 
2. Receive requests and cache copies of the content for faster delivery 

**Note: AWS Local Zones vs Edge Locations = Deployment of services vs. Data**

## AWS Well-Architected Framework Pillars 
1. Security: Protect data and assets; Allow audit and traceability; Monitor and alert
2. Cost optimization: Analyze and attribute expenses; Achieve cost efficiency despite flunctuating resource needs
3. Reliability: Disaster recovery; Scalability; Fault tolerance 
4. Performance efficiency: Deliver performance efficiently given a set amount of resources
5. Operational excellence: Monitor systems and improve supporting processes and procedures
6. Sustainability: Minimize and understand the environmental impact that cloud workload imposes

AWS Well Architected Tool is based on the above framework and aims to help organizations review their AWS workloads without the need for an AWS SA
