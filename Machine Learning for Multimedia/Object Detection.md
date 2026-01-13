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

![[Screenshot 2025-11-06 at 9.39.59 PM.png]]
![[Screenshot 2025-11-06 at 10.05.35 PM.png]]
In Fast R-CNN the ROI generation becomes the bottleneck


Faster R-CNN : Use convolutional network to propose regions
![[Screenshot 2025-11-06 at 10.06.42 PM.png]]

Test-Time speed: from 2.3 seconds to 0.2

## BB Predictions (Basics of YOLO)

Single-shot methods don't use region proposals
Perform localization and classification simultaneously
Decompose the image in a regular grid to handle complexity

![[Screenshot 2025-11-06 at 10.11.21 PM.png]]

Evaluating Object Localization:
![[Screenshot 2025-11-06 at 10.20.03 PM.png]]

Non-Max Suppression Algorithm:
	take all boxes, choose the one with the highest confidence, then check IoU and if it's over a certain threshold, it's the only bounding box that is used

YOLO:
- discard all boxes with p_c less than 0.6
- while there are any remaining boxes
	- pick the box with largest $p_c$ and output that as a prediction
	- Discard any remaining box with IoU greater than 0.5 with the box output in the previous step

What happens when there is more than one object in a grid cell? --> need to be able to make multiple predictions per cell

Solution: Anchor Boxes --> different size and shape, larger vector in y
![[Screenshot 2025-11-06 at 10.29.13 PM.png|500]]

Previously: each object in training image is assigned to grid cell that contains that object's midpoint
with 2 anchor boxes: each object in training image is assigned to grid cell that contains object's midpoint and anchor box for the grid cell with highest IoU

Anchor Boxes are human-encoded priors on the size and aspect ration of objects: designing anchors is deciding how many and which shapes to use


How to evaluate the Object Detection Algorithm?

- True positive: correct class prediction and IoU >= 0.5
- False positive: wrong class prediction or IoU < 0.5
- False Negative : Missed (not detected) objects

only one detection can be matched to an object
Distinctive value of precision and recall defined for each class

The Precision-Recall curve is not necessarily monotonic: when the threshold decreases, both true positives and false positives increase, and the precision can go either way
It depends also on the proportion of True Positive and True negative examples (data imbalance)
The Precision-Recall curve is useful to select the best threshold depending on the user requirements
The area under the curve, or Average Precision, is a measure of how good is a classifier

Object detectors are ranked using the mean AP or mAP which is the average AP over all classes

$$
mAP = \frac{\sum_{c\in C} AP_c}{|C|}
$$

The higher the threshold the better the detector must be at locating objects
in some benchmarks the mAP is calculated at different IoU thresholds and then averaged again