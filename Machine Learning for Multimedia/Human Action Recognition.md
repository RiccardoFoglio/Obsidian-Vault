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

13/63



