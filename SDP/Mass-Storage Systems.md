# Overview of Mass Storage Structure

Bulk of secondary storage for modern computers is hard disk drives (HDDs) and nonvolatile memory (NVM) devices 
HDDs spin platters of magnetically-coated material under moving readwrite heads 

- Drives rotate at 60 to 250 times per second 
- Transfer rate is rate at which data flow between drive and computer
- Positioning time (random-access time) is time to move disk arm to desired cylinder (seek time) and time for desired sector to rotate under the disk head (rotational latency) 
- Head crash results from disk head making contact with the disk surface -- That’s bad 

Disks can be removable

![[Screenshot 2025-04-02 at 5.38.20 PM.png|500]]

### Hard Disk Drives (HDD)

platters range from .85" to 14", commonly now 3.5", 2.5" and 1.8"
Range from 30GB to 3TB per drive
Performance:
- Transfer Rate : theoretical 6GB/sec
- Effective Transfer Rate : 1GB/sec
- Seek time from 3ms to 12ms -- 9ms is common for the desktop drives
- Average seek time measured or calculated based on 1/3 of tracks
- Latency based on spindle speed
$$
1 / (RPM / 60) = 60 / RPM
$$
Average latency = 1/2 latency

Access Latency = Average Access Time = Average Seek Time + Average Latency
- faster disks : 3ms + 2ms = 5ms
- slow disks : 9ms + 5.56ms = 14.56ms

Average I/O time = average access time + (amount to transfer / transfer rate) + controller overhead

### Nonvolatile Memory Devices

Solid-State Disks (SSD)
other forms include: USB Drives, DRAM Disk replacements, surface-mounted on motherboards and main storage in devices like smartphones

- more reliable than HDDs
- More expensive per MB
- Shorter lifespan
- less capacity
- much faster
- busses can be too slow --> connect directly to PCI 
- no moving parts so no seek time or rotational latency


11/ch3