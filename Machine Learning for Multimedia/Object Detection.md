Localizing objects is a crucial task for using computer vision in the real world
separate objects from background and predict for each object the bounding box and classification (with confidence score)

![[Screenshot 2025-11-06 at 8.56.08 PM.png]]

![[Screenshot 2025-11-06 at 8.56.53 PM.png]]

![[Screenshot 2025-11-06 at 8.58.48 PM.png]]
![[Screenshot 2025-11-06 at 8.59.29 PM.png]]

![[Screenshot 2025-11-06 at 9.01.59 PM.png]]

turning FC into a CONV layer
![[Screenshot 2025-11-06 at 9.28.44 PM.png]]

moving window with a convolutional network: each of the windows on the initial image will produce 1 result in the final matrix
![[Screenshot 2025-11-06 at 9.32.22 PM.png|500]]

2 stage methods: decompose the problem in 2 stages
- generate sensible region proposals (ROI: Region Of Interest)
- classify each proposed region

![[Screenshot 2025-11-06 at 9.37.43 PM.png]]

R-CNN
- Propose regions
- Classify proposed regions one at a time
- output label + bounding box
- training is slow (84h) and tasks a lot of disk space
- making predictions is slow too 

Fast R-CNN
- propose regions
- use convolutional implementation of sliding windows to classify all the proposed regions at once

![[Screenshot 2025-11-06 at 9.39.59 PM.png]]![[Screenshot 2025-11-06 at 10.05.35 PM.png]]
In Fast R-CNN the ROI generation becomes the bottleneck


Faster R-CNN : Use convolutional network to propose regions
![[Screenshot 2025-11-06 at 10.06.42 PM.png]]

Test-Time speed: from 2.3 seconds to 0.2

## BB Predictions (Basics of YOLO)

Single-shot methods don't use region proposals
Perform localization and classification simultaneously
Decompose the image in a regular grid to handle complexity

![[Screenshot 2025-11-06 at 10.11.21 PM.png]]