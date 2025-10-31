
WDM: Wavelength Division Multiplexing
Transmission of multiple light signals (wavelengths) on the same strand of fiber
- DWDM : Dense WDM --> more sophisticated = more expensive
- CWDM : Coarse WDM --> lower number of wavelengths = cheaper

Initial WDM Application : increase transmission capacity of fiber
- Trunk bandwidth
- increase the utilization (ROI : Return of Investment) of an existing fiber
- point to point configurations

Wavelength switches : can switch optical signals (input and outputs)
![[Pasted image 20250105150152.png|300]]
Ultimate WDM Application : wavelength switched networks
- arbitrary mesh topologies of WDM links and wavelength switches (wavelentgh routers, lambda routers, lambda switches...) Mostly "only" optical cross connects
- Optical switching - wavelength switching
![[Pasted image 20250105150531.png]]
Deployed in the network core because of its coarse bandwidth allocation 
--> 1 optical channel: 2.4Gb/s or more

Optical switching : has the potential of being simple, so delivering a very low cost per switched bit

## Types of Optical Switches

- Optical vs Electronic core
- Cross Connect vs Switch
- Wavelength conversion

Different levels of complexity and flexibility
# Switching Core
## Optical Core
Deploy physical properties of materials to deflect light from incoming fiber to outgoing fiber

- Tilting mirrors
	- micro-electro-mechanical systems (MEMS)
	- voltage operated
- Holographic reflecting surfaces
	- voltage operated
- Materials changing properties with
	- Heat
	- Pressure
	- Voltage/Current
Properties:
- Potentially inexpensive (low CAPEX) : low cost material, low cost process
- Bit Rate and signal independent : unlimited scalability, multi standard
- Low power consumption : low operation costs (OPEX)

- High production costs : immature technology
- High attenuation (and no regeneration)
## Electrical Core
Convert optical signal into an electric one and use a circuit interconnection network
- Optical-electrical conversion
- Receive the bits and switch them
- It loses all the nice properties of an optical core
	- Bit rate independence
	- Low power consumption
	- Low cost

However: cheaper and less complex than packet switching
# Deployment

Expectation from optical network : 
- provisioning and protection of lightpaths end-to-end
- client equipment to control provisioning of optical layer lightpaths (signaling)
- cost-effective deployment of flexible networks
## Provisioning
![[Pasted image 20250105153405.png]]
![[Pasted image 20250105153441.png]]
![[Pasted image 20250105153508.png]]
## Protection / Restoration
- Protection : pre-determined action
	- non-optimal resource utilization
- Restoration : dynamically determined action
	- optimization of resource utilization
![[Pasted image 20250105153623.png]]
# Control Plane

Optical Switches need:
- Resource discovery
	- Topology 
	- Access points and node identification
	- Resource usage
- Connection management/signaling
	- Lightpath setup
	- Lightpath take down
	- Lightpath modification
- Distributed routing
- Mesh/ring network protection and recovery
- Establishment of protection service classes

Optical Network Users need : 
- Resource discovery
	- Address of users reachable through the optical network
- Manage lightpaths
	- Lightpath setup
	- Lightpath take down
	- Lightpath modification
- Negotiate protection service classes
	- Protected, unprotected, best effort lightpaths

Routing : in the optical internet network users and routers :
- Overlay Model : 
	- optical network provides connectivity between routers
	- Routers see the optical network as a black box
	- Routers might be provided with reachability information
- Peer Model : 
	- Routers and switches participate to the same routing protocols
	- Routers know the topology of the optical network
	- Routers can choose the preferred path for lightpaths between them to reach specific destinations

MPLS is the only sensible choice for Optical Network Control Plane
--> MP$\lambda$S : Multi-Protocol Lambda Switching
OSPF, IS-IS, BGP for resource discovery
RSVP/LDP for signaling

