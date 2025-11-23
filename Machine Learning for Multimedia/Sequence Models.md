Sequence data = data that comes as ordered items
![[Screenshot 2025-11-23 at 4.11.51 PM.png]]



![[Screenshot 2025-11-23 at 4.13.52 PM.png]]
i = index of sentences
t = index of elements in each sentence

representing words: start with a vocabulary (or dictionary), to each word is assigned its position
![[Screenshot 2025-11-23 at 4.48.32 PM.png]]
sequence constructed by a matrix
![[Screenshot 2025-11-23 at 4.49.46 PM.png]]
special tokens for characters or text that is not expected in our vocabulary
![[Screenshot 2025-11-23 at 4.50.23 PM.png]]
Problems:
- Inputs/Outputs can be different lengths in different examples
- Doesn't share features learned across different positions of text
- Doesn't include mechanisms akin to memory

Recurrent Neural Networks (RNN) can solve these problems
- Input will consider the condensed version of what the network has seen the previous step

![[Screenshot 2025-11-23 at 4.58.12 PM.png]]
2 functions for each layer:
- what to output at that step (Y hat)
- what to propagate in the network (a\<n>)

only flow: from beginning to the end

Forward propagation:
the network will learn the weights and if the past is more or less relevant wrt the input
![[Screenshot 2025-11-23 at 5.05.07 PM.png]]

Backward propagation:
difficult to train, because it starts from the end
![[Screenshot 2025-11-23 at 5.07.38 PM.png]]
Loss function for each element of the sequence, then sum all to have a final value
![[Screenshot 2025-11-23 at 5.08.36 PM.png]]
![[Screenshot 2025-11-23 at 5.09.06 PM.png]]
propagating the final loss thru the entire sequence until we reach the 1st element.
now done by systems but helpful to find insights on why there are some limitations

# Architectures and Applications

![[Screenshot 2025-11-23 at 5.13.50 PM.png]]

many-to-one: simplified architecture, we disregard all outputs except the last one

![[Screenshot 2025-11-23 at 5.15.27 PM.png]]

one-to-many: only take one input, then system is autonomous, giving itself the output as input

Many to Many where length of input is different from the length of the output.
Suppress the output, process input, produce a compressed space, then next section produces output
![[Screenshot 2025-11-23 at 5.18.15 PM.png]]

Language modeling:
