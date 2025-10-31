# Computer Evolution
first general purpose computers were created in the late 40s

Personal computer now ($500) is equivalent to what could be bought for $1M in 1985

evolution possible by:
- advances in semiconductor tech
- innovations in computer design

![](Pasted%20image%2020240924084839.png)

### Exponential growth of the size of Data to be stored
![](Pasted%20image%2020240924085122.png)
### Microprocessor Performance Growth
![](Pasted%20image%2020240924085427.png)
Performance referenced to VAX-11/780

- **70s-80s**: 25% per year -> tech advancements
- **80-2000s**: 50% per year -> tech was improving and RISC architectures introduced
- **starting 2002**: 23% per year -> power issues, lower instruction-level parallelism, unchanged memory latency. Major industries cancelled its race for high performance single processor projects running at higher clock frequencies, and embrace multicore CPUs
- **2010s**: 12% per year, or double every 8 years -> mainly due to limits on instruction parallelism

Growth: due to improvements in technology, microprocessor architecture and software dev.

Software is a big part of the growth: Compiler is crucial for RISC architectures, to perform tasks efficiently

From simple CPU architectures (like this 8086 Architecture, standard in the 80s)

![](Pasted%20image%2020240924090934.png)

To many more modules and cores
![](Pasted%20image%2020240924091000.png)
open for adding accelerators, for graphics, AI (so matrix multiplications)
## Cerebras
Devices for AI applications. Training of large language models, which requires huge computational power.
![[Pasted image 20240924091148.png|200]]


# Computer Market
## Personal Mobile Device (PMD)
smart phones tablets computers.
emphasis on energy and efficiency and real time applications
saturated market, not much growth
system price: $100-$1000
microprocessor price: $10-$100
## Desktop Computing
area covers from PCs to workstations
main target is to optimize the price-performance ration
system price
## Servers
larger-scale and more reliable computing services
main parameters are:
- availability
- scalability
- throughput
system price: $5,000 - $10,000,000
Microprocessor price: $200 - $2,000
## Warehouse-Scale Computers (WAS)
Software as a Service (SaaS)
Platform as a Service
Emphasis on availability, price-performance, and power consumption
supercomputers, emphasis: floating point performance and fast processing
## Embedded Computers
fastest growing portion of the computer market
covers all special-purpose computer-based applications
Adopted microprocessors vary from cheap low-end 8-bit processors to very efficient and expensive high-end processors
System price: $10 - $100,000
Microprocessor price: $0.01 - $100
Requirements:
- real-time performance
- memory minimization
- power consumption minimization
- reliability constraints
Embedded problems are often solved resorting to one of these solutions:
- Standard processor + custom logic + custom SW
- Standard processor + custom SW
- Standard DSP + custom SW
Programmable devices (FPGAs) are playing a growing role
# Parallelism
## Classes of Parallelism

- Data-level Parallelism (DLP) : data items that can be operate on at the same time
- Task-level parallelism (TaskLP) : different tasks (threads) of a work can operate independently
- Instruction-level parallelism (ILP) : different instructions can be executed simultaneously
## Parallel Architectures

Instruction Level Parallelism (ILP) modestly exploit DLP
Vector Architecture and GPUs exploits DLP
Thread-level parallelism (TLP) exploits DLP and TaskLP
Request-level Parallelism (RLP) ((ignored for now))

# Designing a Computer

determining which attributes are important for the new machine
design a machine which:
- maximizes performance
- matches cost and power constraints
## Computer Architecture

Computer design took advantage of:
- architectural innovations
- technology improvements

Three aspects of computer Design:
1. Instruction set architecture
2. organization
3. hardware

Must meet requirements on:
- functional requirements
- price
- power
- performance
- dependability
## Moore's Law

observation stating that for a number of decades: 
>The number of transistors can be integrated in a single chip doubles every 18/24 months

![](Pasted%20image%2020240924100434.png)

## IC Manufacturing Cost

Yield is the percentage of products that pass the test phase
Every product undergoes an evolution which normally leads to an improvement in yield (learning curve)
When yield increases, cost decreases

>More than 50% of manufacturing costs is due to validation and testing procedures

## Power Consumption
continuous increase in system complexity and device integration often leads to problems with power consumption
Power consumption may be critical under two aspects:
- power
- energy
### Power
until now it has been dominated by dynamic power (consumed by each transistor when switching between states)
- **dynamic power** for each transistor can be evaluated: $Power_{dynamic} = \frac{1}{2} \cdot C \cdot V^2 \cdot f$
- **static power** is becoming gradually more important: $Power_{static} = V \cdot I$ (25% of power consumption)
### Energy
Given by: $Energy_{dynamic} = C \cdot V^2$
Mainly of interest for mobile devices

## Dependability
dependability is the quality of the system to deliver a correct service
dependability of a computer system is traditionally very high, but can be lowered by:
- bugs in the design of hardware
- bugs in software
- defects in the hardware (introduced in manufacturing process)
- faults happening during the product operation

### Safety-critical Applications
in the past possible misbehaviors of computer systems were critical in some areas such as Space, Avionics, Nuclear plant controls
In more recent years computer-based systems expanded to other safety-critical areas, such as: 
- rail-road traffic control
- automotive
- biomedical
- telecommunications
#### Dependability Evaluation
- Mean Time To Failure (MTTF) or Failures In Time (FIT) which is its reciprocal.
	- 1 FIT = 1 failure in 1 billion hours
- Mean Time Between Failures (MTBF)
- Mean Time To Repair (MTTR)
$$MTBF = MTTF + MTTR$$
Availability is the probability that a system works correctly in a generic time instant
![[Pasted image 20240924111015.png]]
To fix the initial failure rate, manufacturers can age artificially the circuits (BURNING)

Newer technology change the shape of this curve:
- failure rate in the flat is higher
- dropout arises earlier
- more difficult to forecast when dropout will arise, since it's affected by
	- process variations
	- stress
	- environmental conditions

As a result, in-field test becomes crucial

## Computer Performance
**User POV**: response time (time between start and completion of an operation)
**System Manager POV**: throughput (total work done in the time unit)
### Time
- Elapsed time -> from start of request to answer
- CPU time -> time of CPU usage
	- user CPU time -> usage of CPU from user
	- system CPU time -> overhead, due to sharing the CPU 

Alle these measures can be of interest: UNIX provides all of them through the `time` command:

```
90.7s 12.9s 2:39 65%
```
User CPU time - System CPU time - Elapsed time - CPU time / Elapsed time
### Performance Evaluation
often performed by letting the computer execute applications and observing its behavior.
the choice of applications severely affects the performance.
Ideal case: one should use as workload the mix of applications the user will run
However, they are normally unknown and largely variable from one user to another.

some *benchmarks* are selected to mimic real cases
### Program Benchmarks
possible benchmarks:

- real programs -> compilers, text processors, special-purpose tools
- kernels -> Livermore Loops, Linpack
- toy benchmarks -> Quicksort, sieve of Eratostenes (algorithms)
- synthetic benchmarks -> Whetstone etc...
### Benchmark Suites
Contains a number of different programs so that the weakness of any component is lessened by the presence of others.
benchmarks sets are normally composed of: 
- kernels
- programs
- applications

SPEC evolution (Standard Performance Evaluation Corporation)
MiBench benchmarks
### Reproducibility
information about execution times on benchmarks should allow reproducibility
This means reporting details about:
- hardware
- software
- program input
### Performance Comparison
- Total Execution Time ->  $\sum_{i=1}^nTime_i$ 
- Arithmetic Mean ->  $\frac{1}{n} \sum_{i=1}^nTime_i$ -> can't tell if it's fast or has many programs
- Weighted Mean ->  $\sum_{i=1}^n Weight_i \times Time_i$ -> best option

To measure a real program weight it according to their frequency of execution

Example:
![[Pasted image 20241001201437.png]]

# Guidelines and Principles for Computer Design

## Amdahl's Law
$$
speedup = \frac{performance\ with\ enhancement}{performance\ without\ enhancement}
$$
The speedup resulting from an enhancement depends on two factors:
- $fraction_{enhanced}$ -> fraction of the computation time that takes advantage of the enhancement
- $speedup_{enhanced}$ ->size of the enhancement on the parts it affects

$$
execution\ time = \frac{1}{(1-fraction_{enhanced})+ \frac{fraction_{enhanced}}{speedup_enhanced}}
$$
Example:
![](Pasted%20image%2020241001203157.png)


approaches to measuring the time required to execute a program:
- observing the real system
- simulation
- applying the CPU performance equation

## CPU Performance equation
$$
CPU\ time = (\sum_{i=1}^{n}CPI_i \times IC_i) \times Clock\ cycle\ time
$$
**CPI** = number of clock cycles required by instruction i -> depends on the hardware organization and instruction set architecture
**IC** = number of times instruction i is executed -> depends on the instruction set architecture and compiler technology
**clock cycle time** = inverse of clock frequency -> depends on the hardware technology and organization

In pipelined processors, $CPI_i$ may vary for a given instruction, depending on different parameters
- instructions executed before and after (duration is variable, so some can end before all end, some can start before/after other instructions etc...)
- Memory system behavior (cache miss/hit alter the performance calculation)

=> evaluating the execution time analytically becomes much harder
