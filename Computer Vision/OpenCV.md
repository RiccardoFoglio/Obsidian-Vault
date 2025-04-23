![[Screenshot 2025-04-23 at 4.30.25 PM.png]]

- specifico per computer vision
- Matlab è generico

- velocità: 30+ fps processati in tempo reale
- Matlab processa solo 4/5 frame al secondo

- efficienza: Matlab ha bisogno di più risorse di sistema di OpenCV

- Matlab è più facile da usare e ha un ambiente di sviluppo integrato

- Matlab è sotto licenza, openCV è gratis

# Main Modules

- Core : basic structures and algorithms
- imgproc : image processing algorithms
- videoio : media I/O
- video : video analytics
- imgcodecs : image file reading and writing
- calib3d : camera calibration and 3D reconstruction
- features2d : 2D features framework
- objdetect : detection of objects and items

- highgui : provides basic UI capabilities (C, C++, Python)
- ml : machine learnong classes for statistical classification, regression and clustering o data
- photo : computational photography
- dnn : deep neural network module

# Data Structures

all OpenCV Classes are in `cv2`
OpenCV si appoggia a `NumPy`

 `cv2.circle()`
 `cv2.line()`
 `cv2.ellipse()`
 `cv2.rectangle()`

`cv2.imread()` --> loads an image from file and returns a NumPy array 
`imread(filename, [, flags])`
Flags specify the way image should be read: 
- `IMREAD_COLOR (1), IMREAD_GRAYSCALE (0), IMREAD_UNCHANGED(-1)`

`cv2.imwrite()` --> saves an image on disk, return a boolean
`imwrite(filename, img[, params])`

`cv2.imshow()` --> mostra un immagine
`imshow(winname, img)`
can create different windows but with different names but the preferred way is via `matplotlib`

`cv2.addWeighted()` --> performs a linear blending between two images of the same size
`addWeighted(src1, weight1, src2, weight2, 0.0, dst)`

`cv2.VideoCapture()` --> opens video stream from connected camera and returns object
`VideoCapture(id)`
`id` is the device index (typically 0) or filename

`cap.read()` --> reads a single frame from the camera

`cap.release()` --> releases the camera

`VideoWriter()` and `write()` are methods for saving videos

`cv2.cvtColor` --> converts input image from one color space to another and returns new image

!! IMPORTANT images in OpenCV are BGR instead of RGB as default !!

# NumPy

highly optimized library for numerical operations
high performance multidimensional array objects and tools for working with them

`import numpy as np`

`np.array([1,2,3,4,5])` --> multidimensionale e più efficiente
`np.array([1,2,3],[4,5,6])` è multi dimensionale

`np.zeros((n,m))` , `np.ones((n,m))`
	genera una matrice n X m piena di zeri o 1

`np.eye(n)` genera una matrice d'identità di dimensione n

