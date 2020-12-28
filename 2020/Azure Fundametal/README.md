# Module 1

## Why cloud

- High availability
- Scalability
- Elasticity
  - Scale out and in.
  - Scale up and down.
  - Dynamic scalability.
- Agility
  - Do things first.
  - Running things in minutes.
- Fault tolerance
  - Pysical problem is cloud provider's responsibility.
  - Disaster recovery.
  - Share responsibity

## Features

1. 56 regions in the world.
2. Olympics online game uses Azure Data Center.
3. Predicative cose for budget.
4. Cloud and Security
   - How I put it and how the cloud provider save it?
   - DDoS protection is default for every service and free.
5. Economies of scale
   - Less expensive
   - More effoicient
   - Pass benefits on
6. CapEx vs OpEx
   - Nobody likes to run cable. Dont worry about the hardware.
7. Consumption-based model
   - No upfront cost.
8. Types of cloud
   - Public cloud
     - ExpressRoute
     - 
   - Private cloud
     - Owned by customer.

   - Hybrid cloud:
     - Combines public and private clouds.
     - On-prem AD sync to Azure AD for Office365 usage.
9. Types of Services
   All of the following servers can talk to each other.
   - SaaS
     - Prdicative cose.

   - PaaS
     - Build the solution or buy the solution.
     - 
   - IaaS
     - Flexible.

   When do you use SaaS, PaaS, IaaS? think "Pizza as a Service".



# Module 2 - Azure Core Services

## Regions

1. A regions is a collection of Data Centers.
2. It gives us flexibity and scalibility.
3. Some of regions of different resources.
4. Pick the regions closing to the end users.
5. Customer chooses the regions, not cloud provider.
6. Regions are paired, and can be failed-over. One pair must in the same geography.
7. Facilities have isolated cooling, power, firewall and networking.


## Zones and Sets

1. Data centers have updates, the update can be by Zones and Sets.
2. Isolation bundary.
3. 3 webservers on the same region, but Azure will put each of them in the different zone. MS doesn't update the 3 zones in the same time. (Fault and update tolerence).


## Resource Groups

1. Every resource lives in a Resource Group.
2. The resouces in a resource group lives with same life cycle and same region.
3. Hoerarchy: HereManagement groups -> Subscriptions -> Resource groups -> Resources.
4. Security by role-based authorization.
5. 

## Azure Portal

1. Use the search panel to search the available resources.
2. Preview product(s) doesn't have SLA.
3. Tutorials about creating VM:
   - Event Log monitor is optional, it will use some storage.
   - You can always change the Size of a VM.
   - Custom Script: run some scripts after the VM created.
   - The process of creation can be saved as a template(JSON file) for later creating the same-spec VM.
   - Bastion service: connect to a public-ip VM with first authentication.

## Container Services

### Azure Container Service
### AKS

## Azure Newwork Services

- Azure Virtual Network
- Azure Load Balance
- VPN Gateway
- Azure Application Gateway
- Content Delivery Network
- VNet Peering

## Connectivity

### ExpressRoute

Connect from on-prem to Azure.


## Azure Data Categories

1. Structure data
2. Semi-structure data
3. Unstructure data



## Create a storage account tutorial

1. Got to **Storage Account**
2. Storage type:
   - Hot:
   - Cold
3. Replication: notice the traffice cost copying the data between regions (eager traffice).
4. 


## Database server

### SQL Server

- Its a PaaS service.
- Azure has migration data.


## Azure Marketplace


## Azure IOT

- Monitor IOT devices
- Bidirection communication


## Big Data and Analytics

- Azure Synapse Analytics
- Azure HDInsight
- Azure Data Lake Analytics

## Azure AI

- Machine Learning Service
- Machine Learning Studio

## Severless Computing

- Azure Functions
- Azure Logic App
- Event Grid

## DevOps Server

- Azure DevOps services
- Azure DevTest lab

## App Service

- ...
- Serveless code


## Azure Management Tools

- Auzre Portal
- Azure Powershell
- Azure CLI
- Cloud shell




# Module 3

# Module 4

aka.ms/azfun


# Questions

1. First thing to create in Azure?
  - Resource group: A single management unit.

2. Cannot find my resource?
   - The resource might in the other region.

3. Service Message Block(SMB)



`folks`