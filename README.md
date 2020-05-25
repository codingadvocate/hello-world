hello-world
===========

IT Service Management (ITSM) has been well established with proprietary tooling for nearly 20 years. If you’re not familiar with the space, vendor names have included HP, IBM, BMC, CA, Service Now, Micro Focus, Cherwell, Ivanti, and others. So why create an open-source offering for ITSM discovery and integration activities?

Several reasons, including:

 * Add new methodologies into ITSM discovery, more than just the 1-to-1 model of job-to-technology
 * Discovery jobs should be vendor agnostic; develop once and send to any ITSM product
 * Offer horizontal, on-demand scalability with the discovery framework, instead of installing/configuring new collectors
 * Greatly increase the developer resource pool for new content
 * Crowdsourcing discovery/integration jobs

As a special note: the included "dynamic discovery" is fundamentally different from the method used by proprietary products. It captures new products and dependencies without requiring 1-to-1 configurations or 1-to-1 discovery before mapping a new product. This is increasingly important with how fast products go from idea to release today, by using turn-key DevOps, readily available developer libraries, and the open-source consortium.

There are many paid ITSM services available after setting up tooling, as delivered by partner ecosystems with the proprietary products. The same is available here, but without the initial price tag. The preferred provider of paid professional services, support, training, and documentation is currently CMS Construct (https://cmsconstruct.com), the creators of this platform.


Installation:
-------------
OCP has stack dependencies on Python 3.x and corresponding modules, Kafka, and Postgres. 

Install the stack: 
 * Kafka
 * Postgres
 * Python 3.x

Note: To simplify the setup for a Proof of Concept (POC), you can put everything on a single server, using localhost, without redundancy or TLS certs.

Install this project (examples below illustrated from PowerShell on a Windows server):
 * Download the code into a local directory (e.g. D:\Software\openContentPlatform)
 * Configure this to run on your stack::

   cd "D:\Software\openContentPlatform\framework\lib"
   python .\platformConfig.py

Clone the directory for a few clients::

   md "D:\Software\ocp_client_1"
   md "D:\Software\ocp_client_2"
   md "D:\Software\ocp_client_3"
   Copy-Item -Path "D:\Software\openContentPlatform\*" -Destination "D:\Software\ocp_client_1" -recurse
   Copy-Item -Path "D:\Software\openContentPlatform\*" -Destination "D:\Software\ocp_client_2" -recurse
   Copy-Item -Path "D:\Software\openContentPlatform\*" -Destination "D:\Software\ocp_client_3" -recurse

Start the OCP server from the command line (you can also use a service/daemon)::

   python <installPath>\framework\openContentPlatform.py 
  
Start some clients::

   python F:\Work\openContentPlatform\ocp_client_1\framework\openContentClient.py resultProcessing
   python F:\Work\openContentPlatform\ocp_client_2\framework\openContentClient.py contentGathering
   python F:\Work\openContentPlatform\ocp_client_3\framework\openContentClient.py universalJob


Since this is entended as an enterprise product, production deployments would have a larger infrastructure footprint as you plan for resilience. 

To avoid a single point of failure, you want multiple Kafka instances. Depending on the level of desired resilience, the same goes for ZooKeeper - used by Kafka. For the database, you can accomplish this multiple ways (3rd-party or database-specific clustering, active syncs, virtualization-level mechanisms, storage-level mechanisms).

The clients are built to horizontally scale on demand. If you need more performance/capacity/throughput on a certain client type, spin another one up. It will subscribe to the appropriate service and start the work. You can have tooling automate the spin up and down of new clients as well. To enable that, you would copy/clone the directory structure for an "image" to deploy/execute on demand.

Multiple clients (of the same type or different), may be spun up in the same execution environment - as long as they are deployed in their own sub-directory. This avoids file conﬂicts, mainly from logging. 


License:
--------
`GNU Library or "Lesser" General Public License (LGPL) v3 <https://opensource.org/licenses/LGPL-3.0>`_

