## I/O Hardware

Variety of I/O devices: storage, transmission, human-interface
common conceps: signals from I/O devices interface with computer
- Port : connection point for device
- Bus : daisy chain or shared direct access
	- PCI bus common in PCs and servers, PCI Express (PCIe)
	- expansion bus connects relatively slow devices
	- Serial-Attached SCSI (SAS) common disk interface
- Controller (host adapter) : electronics that operate port, bus, device
	- sometimes integrated
	- sometimes separate circuit board (host adapter)
	- contains processor, microcode, private memory, bus controller etc... 
![[Screenshot 2025-06-25 at 4.16.52 PM.png|500]]

Fibre channel (FC) is complex controller, usually separate circuit board (host-bus adapter HBA) plugging into bus
I/O instructions control devices
Devices usually have registers where device driver places commands, addresses, and data to write or read data from registers after command execution
Devices have addresses used by I/O instructions and memory-mapped I/O


## Application I/O Interface




## Kernel I/O Subsystem





## Transforming I/O Requests to Hardware Operations






## STREAMS





## Performance






