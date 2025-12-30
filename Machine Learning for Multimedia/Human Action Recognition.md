Analyze a video to identify the human actions taking place in the video

Video = 3D signal : spatial coordinates + temporal coordinates
combination of these two pieces of information is what allows action recognition

From short to long-term intervals, according to action and problem context

Why recognizing actions?
- Automatic video tagging/summarization
- Smart User-Interfaces (UI)
- Smart surveillance system
- robotics (for human-robot interaction / robot learning)

Video indexing / retrieval: 500 hours of videos uploaded to youtube per minute in 2019 --> need automatic tools to help video indexing and retrieval

Crowd behavior and event analysis
Fine grained action labeling
Intelligent assisted living and home monitoring
Egocentring vision for tele-monitoring and assistance (elders)

Challenges: 
- people can appear at different scales in different videos, yet performing the same action
- large inter-class variability (lots of ways to do something)
- occlusions: actions may not be fully visible
- Camera movements: hand-held or mounted on something moving causing shakes
- Background clutter: other objects/humans in the video frame
- Human variation: humans are of different sizes/shapes/clothes
- Trimmed vs Untrimmed videos: video cut on the specific action VS unedited
- Collecting training datasets is extremely challenging

Action recognition
- input = video image
- output = action label
to which granularity??
![[Screenshot 2025-12-28 at 6.50.56 PM.png|500]]
different types/levels of activities --> ultimate goal is to recognize all of them reliably

## Approaches to Action Recognition

Recognition approaches:
- Hand-crafted approaches
![[Screenshot 2025-12-28 at 6.52.14 PM.png|500]]
- Learning-based approaches
![[Screenshot 2025-12-28 at 6.52.43 PM.png|500]]

CNN provide state of the art performances in image analysis tasks, how can we extend CNNs from images to image sequences?
2 basic approaches:
- single stream architectures (single-modal approaches)
  ![[Screenshot 2025-12-28 at 6.55.18 PM.png|500]]
- two-stream architectures (multimodal approaches)
  ![[Screenshot 2025-12-28 at 6.55.31 PM.png|500]]

## Single-Stream Networks

Extract image features from CNN backbone and then fuse temporal info from consecutive frames

Fusion is not a simple combination of logits
3 different approaches: late, early, slow fusion
![[Screenshot 2025-12-30 at 4.15.30 PM.png|500]]

Early Fusion: combines info of full time-window (10 frames). Frames are stacked and the filter of the first levels are modified to operate on a T (T=10) temporal extent

Late Fusion: two branches (with shared weight) analyzing frames at a fixed time distance (15 frames). The two identical streams are then merged into the first FC layer of the classifier

Slow Fusion: mix between the two approaches.
the fusion of temporal and saptial info is slowly distributed along the architecture
The number of branches are consecutively halved along the model
The first four branches analyze 4 frames each (2 shared). The last conv layers combines info from all 10 frames

Slow fusion works better than early and late. 
Fusion approaches perform slightly better than simple fusion of the single frame predictions

RNN/GRU/LSTM are well suited for processing sequences (and action recognition can be cast as a frame sequence processing)

First attempt were based on first separately extracting frame features from a backbone and them processing them with LSTM

CNN Long-Term Recurrent (LRCN)
Convolutional Block (encoder) + LSTM (decoder) using an end2end training of the entire model
![[Screenshot 2025-12-30 at 4.52.14 PM.png|500]]

Training by sampling videos in a 16 frame clip 
(test by using 16 frame clips, sampled with a distance of 8 frame each)

given the relevance of motion data, why not using optical flow instead of RGB frames? --> best solution appears to be to combine both

Pros: end2end training framework
Cons: 
- in training breaking the video into clips can result in false label assignments
- LSTM are unable to capture long-range temporal info (further works show that a denser and longer temporal sampling is better)

Video is a 4D tensor (RGB-t)
processing an image with a 2D ConvNet produces an image --> convolutions and pooling are only done spatially
processing an image with 3D ConvNet produces in volume --> convolutions and pooling involve all spatio-temporal dimensions
3D ConvNet as feature extractor:
- best 3D conv kernel and network are found by a search in the hyperparameter space
- classification using a multi-class linear SVM
- training using 5 random clips
- at test time the prediction of 10 random clips is averaged for the final label

Limitations: 
- issues in handling longer videos and/or capturing longer temporal dependencies
- issues with training (3D ConvNet have much larger number of parameters)

## Two-Stream Networks

One CNN for spatial context, one CNN for temporal context
	one RGB frame as input of spatial stream and a stack of N optical flow frame for temporal stream
![[Screenshot 2025-12-30 at 5.01.20 PM.png|500]]

Idea comes from human visual cortex: ventral stream (object detection) + dorsal stream (motion detection)
![[Screenshot 2025-12-30 at 5.01.55 PM.png|500]]

The two streams are trained separately and then combined using SVM. Final prediction obtained by averaging the predictions across all input frames

- improves performance of single stream methods
- still missing long-range temporal information
- suffers from false-label assignment problem
- model cannot be trained end2end --> cannot benefit from joint spatial and temporal information during training

3D-fused two streams : extension of previous approach inflating to 3D the spatio-temporal analysis

![[Screenshot 2025-12-30 at 5.03.52 PM.png|400]]

Combines 3D models with the two stream approach --> two stream inflated 3D  ConvNet

Temporal Segment Netwok (TSN): 
long-range temporal modeling still remains an issue, how to extract relevant info in spatial and temporal domains? (so far mostly dens and uniform frame sampling)
a possible solution is improving the basic two stream architectures in two ways
- provides a sparse sampling of the input video into "clips" (snippets)
- implements a robust scheme for aggregating segments/snippets predictions
- it's trainable end2end

Scatter Sampling: TSN operates on a sequence of short snippets, obtained by first dividing the video into K segments and then sampling (randomly) segment into a small number of frames
![[Screenshot 2025-12-30 at 5.09.20 PM.png|500]]
each snippet is processed by a two stream model (sequence of RGB frames and stacked OF, all models have shared weight)
Consensus function: best option was a simple average
![[Screenshot 2025-12-30 at 5.10.14 PM.png|500]]

CNN limitations: CNNs and 3D CNNs struggle with long-range motion and scaling to longer clips due to local receptive fields and short temporal windows

Transformer contribution: self-attention models join spatiotemporal dependencies over long horizons, enabling end2end learning that scales and improves (long horizon) action recognition

Vision Transformer (ViT) in videos:
- Strength: excels at intra-frame relationships for image classification
- Limitation: no native modeling of temporal dependencies

TimeSformer (time-space transformer)
self-attention over space and time is computationally heavy
represents a video as a sequence of frame-level patches (tokens)
learns spatiotemporal relations directly via attention
it uses Divided Space-Time Attention:
- Spatial attention: captures appearance within each frame
- Temporal attention: captures motion & dynamics

Divided space-time attention: divides video in a set of non overlapping patches and applies self-attention separating temporal and spatial attention

- Spatial attention: each patch compared only with patches at the same location on different frames. 
  N matches -> N comparisons
  ![[Screenshot 2025-12-30 at 5.18.57 PM.png|500]]
- Temporal attention: each patch compared only with matches at the same location on different frames
  T frames -> T comparisons ( total = N+T instead of N\*T )

Pros: 
- much faster train time than 3D CNN -> enables training of larger models on longer videos
- much smaller inference time than 3D CNN -> enables real-time video analysis
- Achieved SOTA on several benchmarks

## Human Pose Estimation (HPE)

Detect keypoint locations that describe an object. Human pose estimation requires to detect and localize the major parts/joints of the body (shoulders, knees, ankles etc...)

Human Body is flexible, has high DOF and suffers from self-occlusions
body appearance includes clothes and self-similar parts
HPE in the wild can suffer from environmental occlusions or similar parts by nearby people

Basic steps:
- localizing human body joints/key-points
- grouping them into a valid configuration

### Single-Person Pose Estimation (SPPE)

DeepPose: single-person estimation model
body joint detection is cast as a regression problem: outputs are the 2D body joint positions
Implements a multi-staged architecture to improbe accuracy
It uses a holistic approach: all joints are estimated even if not visible
backend = AlexNet, with extra layer for predicting (x,y) position of k body joints
the model is trained using an L2 loss for regression
![[Screenshot 2025-12-30 at 5.28.56 PM.png|500]]

A total of 3-stage cascaded regressors to refine the estimated pose
when joins are predicted at a stage, image is cropped around the joints and sent to a refiner
![[Screenshot 2025-12-30 at 5.29.55 PM.png|500]]

The works is the first CNN-based HPE approach
The main limitation is that regressing joint positions is an extremely difficult task
useful to shift the problem to the estimation of heatmaps for jints (where a joint is likely to be)

Given a joint, its heatmap is an image where each pixel contains the probability that the joint is located there

Pose machine: sequence of predictors trained to identify joint locations
the approach combines the information of multiple joints to solve ambiguities --> uses implicit spatial model of the human body
![[Screenshot 2025-12-30 at 5.34.45 PM.png|500]]

CPM Consists of 2 or more stages, where each stage contains a multi-class heatmap predictor
The first stage computes initial predictions for joint locations (based on local info computed on a small receptive field)
![[Screenshot 2025-12-30 at 5.35.55 PM.png|500]]
the following stages take as input the image features and heatmaps computed from previous stage and act as context ffeatures
![[Screenshot 2025-12-30 at 5.36.41 PM.png|500]]

Context features help eliminate wrong and strengthen correct estimations on a single heatmap (all heatmaps contribute to an implicit spatial model)
![[Screenshot 2025-12-30 at 5.38.45 PM.png|500]]

The use of larger receptive fields across the network helps capture long-range spatial dependencies among joints
![[Screenshot 2025-12-30 at 6.09.10 PM.png|500]]

CPMs refine predictions through multiple stages and larger receptive fields.
They still struggle to capture global context
Transformers directly model long-range dependencies via self-attention --> more flexible and tranferable across datasets

ViTPose: image is split into patches and turned into tokens, self-attention captures global relationships among all patches, a lightweight decoder produces heatmaps for keypoints
![[Screenshot 2025-12-30 at 6.11.37 PM.png|500]]
decoder:
![[Screenshot 2025-12-30 at 6.12.16 PM.png|500]]

## Multi-Person Pose Estimation (MPPE)

Multi-person HPE is more difficult than single, because the position and number of persons in the images is unknown
2 ways to approach the problem:
- top-down approach
- bottom-up approach

Top-Down: a person detectors output a list of candidates bounding boxes, further analyzed to extract the pose for each person.
Strongly dependent on the accuracy of the person detector. 
Execution time is optional to the number of people visible in the image

Bottom-Up: detect all possible joins and then try to group the ones belonging to the same person. 
Grouping joints in a complex and cluttered environment is a difficult task.
Execution time is mostly independent from the number of people

OpenPose: bottom-up method that can be seen as extension of multi-branch CPM
combines a joint extraction of heatmaps with that of a non-parametric representation (part Affinity Fields) aimed at learning to associate body parts with individuals
Both heatmaps and PAFs are refined in concatenated processing steps

1. extract frame features: compute fream features F using a fine-tuned VGG-19 as featur extractor (or other backbones)
2. heatmaps and PAFs: from feature F, estimate heatmaps (saliency maps S, one per joint) and Part Affinity Fields (one per limb)

Part Affinity Fields: for each limb a PAF represents association scores between joints, as a set of 2D vector fields
These fields aim at encoding location and orientation of limbs in the image

3. Refining detections and associations: from stage 2 onwards detection and association are refined simultaneously exploiting info from prev. stage
4. Part Association: given the complete bipartite graph of possible connections, assign a weight to each edge as the line integral along the segments in the corresponding PAG for that limb
   ![[Screenshot 2025-12-30 at 6.22.41 PM.png|500]]
   Find connections that maximize the total weight
5. Merging: final step is iteratively merging parts. Start by creating a human from each detected part. If two humans share the same endpoint they are the same human, merge them into the same set, remove one human and repeat

DeepCut: Bottom-up approach. it jointly solves the tasks of detection and pose estimation: infers the number of people in a scene and disambiguate people in close proximity
Approach combines a set of joint hypothesis (computed with a CNN detector) with an instance of an integer linear problem (which does the rest)
The optimization problems aim at jointly solving 3 problems:
- select a subset of joints from an initial set of D candidates
- label each selected candidate as an individual joint (C classes)
- extract skeletal info from each person

ILP: integer linear program is the core of the method. Deals with the prev 3 problems as triples of binary variables
- if $x(d,c) = 1$ then it means that (joint) candidate d belongs to (joint) class c (DxC variables)
- if $x(d,d')=1$ candidates d and d' belong to the same person (DxD variables)
variable z is used to partition pose belonging to different people

![[Screenshot 2025-12-30 at 6.46.34 PM.png|500]]

![[Screenshot 2025-12-30 at 6.47.36 PM.png|500]]

AlphaPose (RMPE) : Regional Multi-person Pose Estimation. Top Down method
Based on observation that the pose estimator module suffers from duplicate/inaccurate predictions of the person detector module
![[Screenshot 2025-12-30 at 6.48.59 PM.png|500]]
![[Screenshot 2025-12-30 at 6.49.07 PM.png|500]]

2-step approach: first apply a top-down single-person extractor pose (SPPE) which results in redundant and inaccurate poses.
Then apply a regional multi-person pose estimation (RPME) to soften the previous issues
The main advantage is that the framework is general: it can be applied to different human detectors and single pose estimators

Human bounding boxes are fed to human pose proposal generator (STN + SPPE + SDTN) which generates pose estimates
Pose estimates are refined by te pose NMS module
![[Screenshot 2025-12-30 at 6.51.05 PM.png|500]]

Proposals: STN (Spatial Transformer Network) selects and centers the dominant human region in the bounding box, simplifying the work of SPPE. SDTN remaps estimates to original coordinates
The parallel SPPE branch is a regularizer that penalizes poses that are not well centered 

Drops: parametric pose non-maximum suppression that eliminates redundancies
First it selects the most confident pose. Then it suppresses poses close to it by applying some elimination criterion (EC)
The process is repeated until all redundant poses are eliminated
