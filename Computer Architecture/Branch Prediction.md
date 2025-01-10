Branches can potentially impact in a very serious way the pipeline performance
It's possible to reduce the performance losses by predicting how branches behave.

Branch prediction scheme aim to correctly forecasting branches, thus reducing the chance that control dependences cause stalls

- Static Techniques : handled by the compiler resorting to a preliminary analysis of the code
- Dynamic Techniques : implemented by hardware based on the run-time behavior of the code

## Static Branch Prediction
it can be useful when combined with other static techniques such as enabling delayed branches and rescheduling to avoid data hazards

The compiler may predict branch behavior in different alternative ways: 
- always predict branches are taken --> 34% average misprediction rate, highly variable
- predict branch behavior depending on branch direction --> based on observation that forward branches are more often untaken and backwards branches more often taken
- predict on the basis of profile information coming from earlier runs --> identifying a typical sequence of input for the program, running the program a limited number of times using these inputs, gathering statistics on the behavior of branches, using these statistics as predictions

## Dynamic Branch Prediction
Dynamic schemes make a prediction in hardware during execution
They use the branch instruction address
Dynamic branch prediction can be based via different techniques:
- branch history table (one and two-bit prediction schemes)
- two -level prediction schemes
- branch-target buffer
- others
# Branch History Table

it's the simplest method for dynamic branch prediction
The Branch History Table (BHT) is a small memory:
- indexed by the lowest bits of the address of the branch instruction
- containing for each entry one or more bits recording whether the branch has been taken or not the last time it has been executed

Each time a branch is decoded: an access is made to the BHT using the lowest portion of its address as an index. The prediction stored in the table is used, and the new PC is computed according to the prediction.
When the branch result is known the BHT is possibly updated.
![[Pasted image 20241014161107.png]]

Each time a branch instruction is decoded:
- The BHT is accessed using as address the least significant $T=\log_2N$ bits in the branch instruction address (the least significant $d$ bits are always 0 and are neglected when instructions are composed of $2^d$ bytes)
- The BHT returns the prediction (T/nT) and the PC is possibly updated
If the prediction was wrong, the BHT is updated

The effectiveness of the method depends on 
- the chance that the BHT entry relates to the branch of interest
- the accuracy of the prediction

In the MIPs processor, condition evaluation is performed in the same stage where branch instructions are identified. Therefore the described technique does not five any advantage.
### Two-bit Prediction Scheme

They provide higher prediction capabilities
For every branch, two bits are maintained and the prediction is changed only after missing twice
This technique is also called bimodal branch prediction
![[Pasted image 20241017130637.png]]
### N-bit Prediction Scheme

General case of prev. one. It's based on an n-bit saturating counter associated to every branch instruction.
Counter incremented each time the branch is taken, decremented each time it's not.
When counter value is greater than the half of its max value, the branch is predicted TAKEN, otherwise it's NOT TAKEN.
Experiments: little advantage when using n > 2, not worth
# Branch-Target Buffer

Reducing the negative effects of control dependencies require knowing as soon as possible
- whether the branch has to be taken or not
- the new value of the PC --> target address

Later issue solved with branch-target buffer, which contains
- address of considered branch
- target value to be loaded in PC

Using branch-target buffer, the PC is loaded with the new value at the end of the IF stage, even before instruction is decoded.

![[Pasted image 20241017144615.png]]

![[Pasted image 20241017144636.png]]

Advanced Issues:
If a two-bit prediction strategy is adopted it's possible to combine a branch-target buffer with a branch-prediction buffer
There are ways for extending the branch-target buffer technique to indirect target addresses

Performance effects:
![[Pasted image 20241017145004.png]]
![[Pasted image 20241017145013.png]]




